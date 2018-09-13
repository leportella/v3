---
layout: post
title: "Um breve estudo sobre outliers - Estudo de Caso"
categories:
  - pt-br
tags:
  - pt-br
  - python
  - data science
  - outliers
last_modified_at: 2018-08-01T13:25:52-05:00
featured-img: outlier
---

Continuação [deste post](http://leportella.com/pt-br/2018/01/08/um-breve-estudo-sobre-outliers.html)

Nesta sessão vamos fazer análises mais complexas usando o algoritmo EllipticEnvelope do Sklearn para tentar entender análises de *outliers* em ambientes mais complexos e multidimensionais.

Lembrando que detalhes das análises e gráficos estão disponíveis [aqui](https://github.com/leportella/outlier-analysis).

### 1. Criação de datasets

Para fazer análise de *outliers* vamos criar 1 dataset especfico para o que queremos. Detalhes da criação deste set de dados 
pode ser encontrada [aqui](https://github.com/leportella/outlier-analysis/blob/master/create_dataset.ipynb).

Basicamente temos uma variável y (*store_profit*) que tem um hitograma de cauda longa:

![](https://i.imgur.com/ygXccBS.png)

Duas variáveis independentes distribuda em torno de um centróide (*products in stock* e *product rating*):

![](https://i.imgur.com/EvbWMC5.png)

E mais uma variável categrica (*business_type*) e duas variáveis que são números internos, sem padrão de distribuição.

O Dataframe resultante:

![](https://i.imgur.com/sH7Rk7F.png)

Com essas características:

![](https://i.imgur.com/EvbWMC5.png)

### EllipticEnvelope em X e variáveis numéricas

Uma forma simples de identificar *outliers* é assumir que o dado tem uma determinada distribuição conhecida (por exemplo, 
Gaussiana). Uma vez que assumimos isso, podemos definir uma "forma" para o dado, e definir que observações que ficam 
longe desse formato como anomalias.

No Sklearn temos a técnica do *covariance.EllipticEnvelope* que estima uma covariância robusta ao dado, adaptando uma 
elipse na parte central da nuvem de dados e ignorando o fora desse centro.

Vamos usar o Elliptic Envelope para avaliar nossos dados considerando que temos 1% de contaminação. Para facilitar a compreensão, vamos utilizar apenas as variáveis *products_ind_stock* e *product_rating* como as variáveis no nosso X:

```python
X = df[['products_in_stock', 'product_rating']]
y = df.store_profit
```

Agora vamos analisar nossos dados apenas considerando nossas variáveis independentes:

```python
from sklearn.covariance import EllipticEnvelope

clf = EllipticEnvelope(contamination=0.01)
clf.fit(X)
inliers = clf.predict(X)
```

O resultado da análise é um vetor com o mesmo tamanho do nosso DataFrame contendo valores ou 1 ou -1. 
Todos as observações que tem valor de 1 no vetor foram consideradas pelo algoritmo como *inliers*, ou seja, valores que são 
coerentes com a distribuição, enquanto que valores -1 são observaçes consideradas outliers. 

O resultado, quando observamos graficamente, é este:

![](https://i.imgur.com/Os4hDhg.png)

Mas repare que não consideramos que os valor da nossa variável y nesse processo. Dessa forma, ao observamos as amostras em y que 
foram consideradas *outliers* elas estão distribudas quase aleatóriamente:

![](https://i.imgur.com/2ooxSD3.png)

Muito legal hein?

### EllipticEnvelope em Y

Agora vamos fazer a mesma análise. Mas, ao invés de passarmos minhas variáveis X para o algoritmo, vamos analisar a minha 
variável y. Qual resultado é esperado?

```python
y  = y.values.reshape(-1,1)

from sklearn.covariance import EllipticEnvelope

clf = EllipticEnvelope(contamination=0.01)

clf.fit(y)
inliers = clf.predict(y)
```

Na verdade, quando avaliamos uma variável só, especialmente quando ela tem uma longa cauda como observamos lá em cima, o algoritmo basicamente faz um "corte" da cauda. 

Dessa forma, temos que o nosso gráfico de y vai aparecer com um "corte", removendo valores muito extremos:

![](https://i.imgur.com/xUj90Kg.png)

Agora, se olharmos o gráfico dos valores das variáveis independentes temos a aleatoriadade das amostras:

![](https://i.imgur.com/JMddE1Q.png)

### EllipticEnvelope em X e variáveis categóricas

E se, ao invés de usarmos apenas duas variáveis numéricas como vimos anteriormente, usássemos todas as variáveis que temos 
disponíveis? Categóricas ou não? 

```python
y = df.store_profit
X = df.drop('store_profit', axis=1) # pega todos os valores exceto a coluna do y
```

Bom, primeiro temos que fazer um *encoding* das variáveis categóricas para que o algoritmo as entenda:

```python
from sklearn.preprocessing import LabelEncoder

encoder = LabelEncoder()
X['business'] = encoder.fit_transform(X['business'])
```

Depois a análise fica exatamente do mesmo jeito que anteriormente:

```python
from sklearn.covariance import EllipticEnvelope

clf = EllipticEnvelope(contamination=0.01)
clf.fit(X)
inliers = clf.predict(X)
```

O resultado é diferente daquele que encontramos anteriormente, considerando apenas duas variveis:

![](https://i.imgur.com/sbQvPqO.png)

Entretanto, não tem como fazermos a avaliação visual de como isso se comporta no espaço, como fizemos no exemplo anterior :/


## Reflexão

A análise de *outliers* depende muito do dado que você está vendo e o que você quer observar. Isso já é um senso-comum. 
Porém, ao se fazer uma análise, devemos sempre ter em mente que diferentes técnicas resultam em diferentes *outliers*, dependendo do enfoque que você der para ela ou da forma como ela faz a identificação. Muitas vezes, o melhor é tentar fazer diferentes técnicas para ver qual delas é mais eficiente. 

Considerando os casos que apresentei, a conclusão que eu obtive é que existem 3 casos diferentes que podem ocorrer quando 
se tenta identificar um *outlier* para fazer análises preditivas a partir dos dados:

**Caso 1: Temos uma anomalia em y**

O primeiro caso teríamos a anomalia presente em y, como mostra a tabela abaixo:

<center>
<img src="https://i.imgur.com/NPKmR2U.png" height="100" style="max-width: 20%" />
</center>

Se fizermos a análise baseando-nos em y, temos a análise removendo corretamente o quinto elemento, que é claramente um *outlier*.
Agora, se fizermos a análise baseando-nos em X, essa observação não será removida. 

**Caso 2: Temos uma anomalia em X**

No segundo caso, a observação anômala ocorrem X. Dessa vez, a análise apenas de y pode não remover esse caso que é, claramente, 
um anômalo.

<center>
<img src="https://i.imgur.com/xCetOdk.png" height="100" style="max-width: 20%"/>
</center>


**Caso 3: Temos anomalia em X e y**

No terceiro caso temos a aparição de dados anômalos justamente em X e em y. A análise, de qualquer forma que se olhe, poderia nos dar uma impressão errada de um dado anômalo quando, na verdade, temos um comportamento relativamente fácil de prever. Quando X é grande, y será grande também.

<center>
<img src="https://i.imgur.com/BsPPlVU.png" height="100" style="max-width: 20%" />
</center>


## Fim?

Por enquanto foram esses os tópicos que eu cheguei e as análises que fiz para tentar entender melhor o assunto. O tema é vasto, 
as técnicas são várias e tem muita coisa pela frente. Se você sabe de algo que não está aqui, 
por favor se sinta a vontade para comentar :)

![](https://media.giphy.com/media/q9lNzUPfLAbBK/giphy.gif)

## Referências

Barnett, V. 1978. The study of Outliers: purpose and model. Appl. Statics, 27, no.3, pp.242-250.

Hawkis, D. M. 1980. Identification of Outliers. Monographs on applied probability and statistics. DOI: 10.1007/978-94-015-3994-4.

Rousseeuw, P. J.; Van Driessen, K.. A Fast Algorithm for the minimum covariance determinant estimator

Tukey, J.W., 1977. Exploratory Data Analysis. Addison-Wesley, Reading, MA.

http://www.dbs.ifi.lmu.de/~zimek/publications/SDM2010/sdm10-outlier-tutorial.pdf
