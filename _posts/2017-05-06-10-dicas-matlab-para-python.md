---
layout: post
title: "10 dicas para mudar do Matlab pro Python"
categories:
  - pt-br
tags:
  - pt-br 
  - python
  - begginers
  - python
  - community 
  - pyladies
  - technology
  - tecnologia
  - programador
  - programadora
  - developer
  - mulheres na tecnologia
  - woman in tech
  - girls in tech
  - computação
  - ciência de computação
  - software development
  - software engineering
  - engenharia de software
  - desenvolvimento
  - auto-ensino
  - self-taught engineer
  - code
  - Django
  - software
  - career
  - tech career
  - open-source
  - no cs degree
  - cs
  - computer science
  - matlab
  - spyder
featured-img: idea
permalink: matlab-para-python.html
redirect_from: /pt-br/2017/05/06/10-dicas-matlab-para-python.html
last_modified_at: 2017-03-09T14:25:52-05:00
---

[🇬🇧 *Leia em inglês*]({{base}}/matlab-to-python.html)

---

Eu comecei estudar lógica de programação quando me deparei com problemas que me exigiam conhecimento em Matlab. Depois de um tempo estudando Matlab me sugeriram trocar para o Python pela sua facilidade, simplicidade e por poder ser aplicado a inúmeras áreas (além de ser gratuito).

Entretanto, ao mudar de uma ferramenta para a outra, tive uma série de probleminhas básicos que tornaram a mudança um pouco chatinha. Às vezes foi preciso mudar um pouco a visão sobre algumas coisas que já estavam enraizadas em mim. Decidi compartilhar minhas principais dificuldades e algumas ferramentas que me ajudaram na transição.

Só para deixar claro, tudo o que é apresentado aqui está em Python 3 porque o Python 2 [está para se aposentar](https://pythonclock.org/) :(

![](https://media.giphy.com/media/nI3Npa6iPC4b6/giphy.gif)

O intuito deste post não é discutir qual ferramenta é melhor, mas mostrar como facilitar a transição de conhecimento de Matlab para Python :)


## 1. Números, letras e outras “cositas mas”…

No Matlab podemos criar vetores e matrizes de números e texto, o que é bastante útil e 
intuitivo. Basicamente, podemos acessar qualquer valor em um vetor ou 
matriz pela sua posição na mesma (Ex: `lista(1,2)`).

Já no Python, além de listas passamos a ver estruturas diferentes e muito potentes, como os dicionários.

Dicionários são estruturas de dados que você pode acessar através da “chave” do valor e não de sua posição na estrutura. Quer saber a utilidade? Em uma lista telefônica seria mais simples encontrar números pela posição na lista ou pelo nome do contato? :D

Além disso, existe uma diferença entre tipos de variáveis decimais (`floats`) e números inteiros (`integers`). Ou seja, é bom se familiarizar primeiro com os tipos de variáveis e estruturas que existem no Python! Essa é uma vantagem bem grande!

![](https://media.giphy.com/media/6FymBmqKeBrl6/giphy.gif)
*Vendo dicionários e tuplas pela primeira vez*

Se você tiver em dúvida do [tipo da sua variável](https://docs.python.org/3/library/stdtypes.html), digite: `type(variavel)` ;)

## 2. Trabalhando com palavras e números

Seja através de dicionários ou listas (estruturas padrão do Python) ou DataFrames (estrutura de matrizes do Pandas), 
Python é facílimo de trabalhar com dados numéricos e de texto em um mesmo contexto. O mesmo não pode ser dito do Matlab, onde inserir textos complica tudo!

Uma lista do tipo `lista = [1, 'dois', [1,2,3], {'chave': 1}]` é totalmente aceitável!

## 3. Acessando valores em listas (e listas de listas)

Em Matlab, contagem de listas começam em `1` e são acessadas através de parênteses. O último valor pode ser acessado através da palavra `end`.

```matlab
% Acessando itens de lista no Matlab

lista = [1, 2, 3];

% retorna 1
disp(lista(1))
  
% retorna 3
disp(lista(end))
```

Em Python, as listas são acessadas com colchetes (parênteses são reservados para chamada de funções, classes e definições) e sua contagem começa em `0`. 
O motivo de terem escolhido essa forma de contagem está além do escopo desse post, mas você pode ver algumas discussões [aqui](https://www.quora.com/Why-do-array-indexes-start-with-0-zero-in-many-programming-languages?share=1). De qualquer forma, isso foi algo muito difícil de eu me acostumar, por mais simples que seja.

Dica: para acessar o último valor da lista, podemos usar o índice `-1`. Já imagina o que o índice `-2` faz? :D

```python
# Acessando itens de lista em Python


lista = [1, 2, 3];

# retorna 1
print(lista[0])
  
# retorna 3
print(lista[-1])

# lista de listas
lista_complexa = [[1, 2, 3], [4, 5, 6]]

# retorna 3
print(lista_complexa[0][2])

# retorna 5
print(lista_complexa[1][1])
```

## 4. Blocos não terminam com “end” e precisam de “:”

No Matlab, a maneira mais simples de fazer um laço é a mostrada abaixo, 
onde você tem uma lista e itera pelos itens dela. Neste laço abaixo, o `for` caracteriza o início do laço 
e o `end` determina seu fim. O interpretador vai ler tudo o que está entre essas duas declarações como parte do laço.

Repare que a identação¹ inserida ali é puramente opcional, apenas uma boa prática.

*¹Identação é quando você marca os blocos dos seus código por espaço para identificar o que está aninhado dentro do código. No exemplo abaixo, os quatro espaços antes da declaração da linha 6 indicam que esta declaração está aninhada (ou seja, dentro) do laço indicado na linha 5 (sem identação).*


```matlab
% Criando a lista
lista = ['a', 'b' , 'c'];

% Fazendo o loop acessando os itens da lista
for item=lista  
    disp(item)
end

disp('Fora do laço')
```

Abaixo vemos o mesmo laço escrito em Python. Dessa vez, o `:` marca o início do laço e não existe `end` para marcar o fim. 
O fim é demarcado pelo fim da identação na linha 7. O interpretador vê que a linha 7 não está aninhada dentro do laço iniciado na linha 5 e, portanto, não considera aquela linha dentro do bloco. Dessa forma, é muito óbvio imaginar que a identação, neste caso, não é opcional e sim obrigatória!

```python
#Criando a lista
lista = ['a', 'b' , 'c']

#Fazendo o loop acessando os itens da lista
for item in lista:  
    print(item)
print('Fora do Laço')
```

## 5. Funções e métodos disponíveis

No Matlab, diversas funções vem carregadas por padrão, por exemplo as funções básicas de matemática como seno e cosseno. 
O Python possui uma filosofia diferente onde [poucas funções](https://docs.python.org/3/library/functions.html#built-in-funcs) são carregadas por padrão e a 
maioria se encontras separada em módulos que você importa no seu código. Isso permite que seu código (e o Python em si) seja mais rápido e carregue apenas o necessário para ele funcionar.

Alguns módulos já vem por padrão no Python puro, por exemplo o módulo matemático. Outros, devem ser baixados de um repositório, como é o caso do Numpy e Pandas. 
Hoje, mais de 107 mil projetos estão cadastros no [repositório oficial](https://pypi.org/). 
Caso você não ache o que procura, é super incentivado que você contrua um módulo novo e inclua no repositório!

```python
import math

print(math.cos(0.5))

# outra forma de se utilizar o mesmo método cosseno

from math import cos

print(cos(0.5))
```

Porém, além dessas funções que vêm nos módulos, cada variável tem funções próprias. 
São funções, específicas de cada tipo de variável, que podem ser acessadas através do nome da variável conforme mostra o exemplo abaixo.

Por exemplo, variáveis do tipo **string** são interpretadas como texto. 
Para variáveis de texto existem funções para verificar se as letras estão todas 
em minúsculo (`.islower()`) ou para deixar todas as letras em minúsculo (`.lower()`) 
ou mesmo conferir se o conteúdo da string é numérico (`.isdigit()`).

Para cada tipo de variável (string, inteiro, float, lista, dicionário, etc) existem diferentes métodos *builtin*. Melhor maneira de conhecê-los é abrir o interpretador, criar variáveis e brincar!

```python
# variável de string
palavra = 'palavra'

# checa se a variável está totalmente minuscula
# retorna True
palavra.islower()

# checa se existe caracter numérico na palavra
# retorna False
palavra.isdigit()

lista = [1, 2, '3']

# adiciona elemento na lista
# retorna [1, 2, '3', 'quatro']
lista.append('quatro')

# conta recorrência de um elemento na lista
# retorna 1
lista.count('3')
```

## 6. Como saber qual módulo usar?

Se você está usando Matlab a maioria de suas necessidades devem ser supridas com o [Numpy](http://www.numpy.org/), [Scipy](https://www.scipy.org/), [Matplotlib](http://matplotlib.org/) e [Pandas](http://pandas.pydata.org/). Essas são as principais ferramentas de análise de dados no Python, mas existem inúmeras outras. Procure por códigos que já fizeram o que você faz, busque no [Github](http://github.com/), [Stackoverflow](http://stackoverflow.com/) ou dê uma passada rápida nos fóruns.

## 7. Aonde eu vejo as variáveis carregadas?

Um dos problemas mais recorrentes era que em Matlab estamos extremamente acostumados 
com a visualização de variáveis. O “terminal” estava ali, mas o script 
também e as variáveis que estavam carregadas também. 
A possibilidade de rodar apenas uma linha de código com um simples `F9` sem necessariamente rodar o código inteiro também me fazia falta.

Em tutoriais de Python vemos aquele “terminal” puro onde você digita as coisas e os resultados saem.
No começo, isso chega a ser assustador. Eventualmente eu me acostumei, mas o que ajudou na transição foi usar o [Spyder](https://pypi.python.org/pypi/spyder).

O Spyder é uma ferramenta para Python, feita em Python, que possui uma interface bem semelhante com a do Matlab: os scripts ficam na parte esquerda, na parte direita superior é possível ver os dados carregados e como eles são compostos (inclusive com um background colorido que ajuda muito a encontrar discrepâncias rapidamente) e um terminal com ipython na parte direita inferior. Até o **F9** funciona da mesma forma! Para mim, foi amor a primeira vista ❤.

![](https://media.giphy.com/media/l3V0dy1zzyjbYTQQM/giphy.gif)

Olha a carinha dele:
![](https://i.imgur.com/RgKovVN.png)

Exemplo do Spyder funcionando com um script de 11 linhas (esquerda), as variáveis carregadas (superior direito) e o terminal ipython (inferior direito)

## 8. Procurando ajuda

Se você não sabe o que uma função faz, simplesmente digite: `help(função)` e 
seja feliz :) Se tiver no Spyder, você pode ir em cima da função e digitar `Ctrl+I`.

## 9. Orientação a objetos

Apesar de o Matlab ter construído algo parecido com Orientação a Objetos, o Python é puramente orientado a objetos. Conforme você avança na linguagem os conceitos de classes (com métodos, atributos e herança de classes) tem que ser estudados mais a fundo. Fique atento nisso!

## 10. Formatação de código

Existem uma séries de guias de boas práticas de código, como tamanho de linhas aceitáveis, como nomear suas variáveis, quantas linhas entre funções, etc. Eu nunca tinha ouvido falar de boas práticas de código antes de estudar outra linguagem… então isso foi meio estranho (e desnecessário) a princípio. Depois de um tempo, você percebe o quanto isso é bom e faz falta!

No caso do Python, esse guia de boas práticas está ligado a uma filosofia pythônica de código limpos, bem escritos e legíveis que é muito enraizada na comunidade. 
Para saber mais sobre o estilo sugerido veja a [PEP8](https://www.python.org/dev/peps/pep-0008/). Para saber mais sobre a filosofia Python vá no terminal e digite import this e seja bem-vindo!

![](https://media.giphy.com/media/3o6ZsYMuMkxBNiy7pC/giphy.gif)

*Bem vindo ao Zen do Python*
