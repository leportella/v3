---
layout: post
title: "Um breve estudo sobre outliers"
categories:
  - pt-br
tags:
  - pt-br
  - python
  - data science
  - outliers
featured-img: outlier
last_modified_at: 2018-08-01T13:25:52-05:00
---

Nas últimas semanas tenho me dedicado a estudar e entender melhor sobre dados anômalos ou *outliers* em séries de dados. 

Na oceanografia, nossos dados anômalos são facilmente reconhecíveis. Uma corrente em uma região calma não tem como chegar em 
10m/s de repente ou uma região com um variação de 1m na coluna de água não vai ter 1 dado com 17m. Mas nem todo conjunto de dados 
te permite saber o que é um *outlier* de forma tão "simples". 

Este post é uma primeira tentativa em descrever o que eu encontrei e refleti sobre as técnicas para identificar dados anômalos 
focando em um conjunto de dados em que o meu *outlier* prejudica uma avaliação dos dados ou prejudica uma análise preditiva, por exemplo.

![](https://media.giphy.com/media/26gJA9SSe4m54MYec/giphy.gif)

## Definição de Outliers

"Um *outlier* é uma observação que se diferencia tanto das demais observações que levanta suspeitas de que aquela observação 
foi gerada por um mecanismo distinto" (Hawkins, 1980)

Muitas aplicações precisam definir se uma determinada observação pertence à mesma distribuição das demais observações 
(é um *inlier*) ou se ela deve ser considerada distinta das demais observaçes (é um *outlier*) ([Sklearn](http://scikit-learn.org/stable/modules/outlier_detection.html)). Nesse contexto, devemos fazer duas distinções no contexto em que os dados podem ser encontrados:

* Detecção de *novelty*: Quando o seu dado de treino não é poluído por *outliers* e estamos interessados em observar 
anomalias em novas observações (dados de teste, por exemplo).
* Detecção de *outliers*: O seu dado de treinamento contém *outliers* e nós precisamos em entender o comportamento padrão 
dos dados para ignorar as observaçes anômalas.

Nesse artigo, vamos fazer análises referentes apenas a *outliers*, quando não temos uma referência que nos indique o que de fato é um *outlier* e o que não é. É interessante dizer que é muito difícil detectar *outliers* em um espaço n-dimensional quando n > 2 porque não é mais possível fazer inspeções visuais (Rousseeuw & Van Driessen, 1999). Então, como é possível avaliar outliers em espaços n-dimensionais?

## Porque entender Outliers?

A detecção de anomalias pode ser utilizada para diversos campos, em que o *outlier* na realidade é a expressão de algo que deve 
ser avaliado com cuidado. Podemos citar:

* Identificação de movimentações fraudulentas em cartões de 
crédito;
* Em uma imagem do espaço, uma anomalia pode ser [a forma de identificar uma nova estrela](https://www.technologyreview.com/the-download/609785/artificial-intelligence-just-discovered-new-planets/);
* Sintomas não usuais podem indicar problemas de saúde de um paciente, considerando a sua idade e gênero;
* A ocorrência anômala de um determinado caso de doença em um hospital ou cidade pode gerar alertas para que haja uma 
investigação das possíveis causas para tal.

Além de casos em que os *outliers* são o foco de determinado estudo, também podemos citar os casos em que a identificação destes 
pode ser útil para fazer uma "limpeza" na bases de dados. Muitas vezes estamos estudando processos que sabidamente não conseguem 
gerar esse tipo de observação ou tiveram alguma influência externa que fez com que a observação se apresentasse muito diferente 
das demais. Nesse caso, a identificação de observaçes anômalas e limpeza da base servem para facilitar o estudo do processo e até 
otimizar algortimos de aprendizado de máquina, por exemplo.

### O Caso de Hadlum vs Hadlum [Barnett, 1978]

A identificação de um caso real de *outlier* teve profunda influência no destino da família Hadlum em 1949. O Senhor Hadlum 
estava apelando para uma corte porque o seu pedido inicial de divórcio havia sido negado. O Senhor Hadlum estava entrando com o 
pedido de divórcio alegando adultério da esposa, a Senhora Hadlum. A única evidência apresentada consistia na data que a Senhora 
Hadlum havia dado a luz a uma criança: 349 dias depois do Senhor Haldum ir embora para prestar serviço militar.

Neste caso, o tamanho da gestação (de 349 dias) era muito maior que a média de gestaçes, de 280 dias e, portanto, poderia ser 
considerado um *outlier*. O juiz do caso decidiu que deveria haver uma limite máximo de credibilidade em algum lugar. Ele 
determinou, então, que baseado em evidencias médicas, 349 era um número bem improvável mas cientificamente possível e negou o 
pedido do Senhor Hadlum. Em 1951 uma outra corte definiu o limite como 360 dias. 

Fascinante, não?

![](https://i.imgur.com/8b73OnB.png)

## Causas da ocorrência de um Outlier

*Outliers* podem refletir (Barnett, 1978):

* Erros de medida (Ex: o aparelho quebrou ou está desregulado, sujeira, etc)
* Falhas na execução da medida
* Processos distintos daqueles que geram as demais observações.

## Análise de outliers via Boxplot

Tukey (1977) definiu o conceito de outliers via boxplots, que é um dos jeitos mais simples de se detectar outliers de forma 
visual para uma variável. A metodologia consiste na identificação via quartis. Dado que o quartil de 25% (Q1)  o valor em que 25% dos dados estão abaixo dele e o quartil 75% (Q3) é o valor onde 25% dos dados estão acima desse valor, temos duas "cercas" definidas por Tukey (1977):

Dado o Range Inter-quartil (IQR - Inter-quartile range)
`IQR = Q3 - Q1`

`f1 = Q1 - 1,5IQR` e `f3 = Q3 + 1,5IQR`

e

`F1 = Q1 - 3IQR` e `F3 = Q3 + 3IQR`

Onde dados que ficam entre f1 e F1 ou f3 e F3 são chamados de *outliers* externos ("*outside outliers*") enquanto 
dados maiores que F1 e F3 são chamados de *outliers* longínquos ("*far out outliers").

![](http://flowingdata.com/wp-content/uploads/2008/02/box-plot-explained.gif)

## Estudo de Caso

Um estudo de caso detalhado pode ser visto [aqui](http://leportella.com/pt-br/2018/01/08/um-breve-estudo-sobre-outliers-2.html)!


## Referências

Barnett, V. 1978. The study of Outliers: purpose and model. Appl. Statics, 27, no.3, pp.242-250.

Hawkis, D. M. 1980. Identification of Outliers. Monographs on applied probability and statistics. DOI: 10.1007/978-94-015-3994-4.

Rousseeuw, P. J.; Van Driessen, K.. A Fast Algorithm for the minimum covariance determinant estimator

Tukey, J.W., 1977. Exploratory Data Analysis. Addison-Wesley, Reading, MA.

http://www.dbs.ifi.lmu.de/~zimek/publications/SDM2010/sdm10-outlier-tutorial.pdf
