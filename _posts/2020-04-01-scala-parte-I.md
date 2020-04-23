---
layout: post
title: "Minha saga aprendendo Scala - Parte 1"
categories:
  - pt-br
tags:
  - pt-br
  - english
  - tecnologia
  - Scala
  - Programming Language
  - programming language
  - java
  - jvm
  - java virtual machine
  - spark
  - San Francisco
  - Dublin
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
  - self-learning
  - programmer
  - ruby
  - scala
  - python
featured-img: scala
permalink: scala-parte-I.html
redirect_from: /pt-br/2020/04/01/scala-parte-I.html
last_modified_at: 2020-04-01T18:25:52-05:00
translation: /scala-I.html
---

Eu decidi que eu precisava aprender uma nova linguagem e a linguagem que escolhi foi Scala. Eu adicionei como meta para 2020 eu, pelo menos, me sentir um pouco confortável com essa linguagem, então aqui estou. 

Até agora, minha história com Scala tem sido... frustrante (para dizer o mínimo). Eu estava tentando aprender sozinha e sem muito alarde - porque é muito difícil - mas decidi escrever o que aprendi e o que foi difícil no processo. Isso pode ser útil para outra pessoa, mas principalmente isso é para mim. Eu utilizo artigos como esse para me ajudar a estudar e ajudar a sentir que estou evoluindo:

<blockquote class="twitter-tweet"><p lang="pt" dir="ltr">Uma dica pra quem não acha um ponto final pra um estudo: escreva sobre o assunto que está estudando. Finalizar um post sobre o tema dá a sensação de dever cumprido/etapa ultrapassada além de ajudar na fixação do conteúdo e organização do estudo. <a href="https://t.co/SjliCeQCP6">https://t.co/SjliCeQCP6</a></p>&mdash; Leticia Portella (@leleportella) <a href="https://twitter.com/leleportella/status/1238519625631043585?ref_src=twsrc%5Etfw">March 13, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Então vou me forçar nesses artigos a organizar meus pensamentos, compartilhar coisas interessantes que encontrei e encontrar algum uso para as minhas frustrações. Essas postagens não pretendem ser um tutorial, mas uma forma de deixar registrado o que eu aprendi (conhecido em inglês como *brain dump*, despejo cerebral).

<center><img src="https://media.giphy.com/media/l41m04gr7tRet7Uas/giphy.gif" style="height:300px;"/></center>
<br/>

## Porque Scala?

Para ser sincera, eu não escolhi Scala, Scala me escolheu 😅. Em geral, minhas razões foram:


- ✅ É uma linguagem orientada a objetos e funcional ao mesmo tempo. Isso vai me fazer questionar minhas suposições e mudar a maneira como eu desenvolvo. Todo desenvolvedor incrível que eu conheço recomendou que eu aprendesse uma linguagem funcional para mudar o estilo de programar. Eu poderia escolher qualquer uma, Scala serve como qualquer outra.
- ✅ É uma linguagem que está sendo altamente utilizada para análises de grandes conjuntos de dados e, portanto, super útil no mundo da ciência de dados. Eu adoro ciência de dados e adoro processar dados. Se eu quiser ir para um idioma útil para ciência de dados e *big data*, Scala é uma excelente opção.
- ✅ É uma linguagem JVM de fortemente tipada. Algo que pode me ajudar a apresentar esse mundo e seguir uma direção diferente do que eu vi até agora com Python.
- ✅ É uma das principais tecnologias do meu trabalho atual. No ano passado, tive que trabalhar numa base que usa Scala e isso exigiu muita ajuda de colegas, porque eu não fazia a menor ideia de como resolver o problema. É uma oportunidade de aprender alguma coisa... então por que não apenas parar, estudar e parar de sofrer?


## Mais sobre a linguagem

Da [wikipedia](https://en.wikipedia.org/wiki/Scala_(programming_language)):


> **Scala** _é uma linguagem de programação de uso geral que fornece suporte para programação funcional e um sistema estático fortemente tipado. Projetado para serem conciso, muitas das decisões de design de Scala visavam abordar as principais críticas feitas a Java._ 

Scala é uma linguagem compilada, o que significa que todo o código que você escreve é compilado em um arquivo binário que é então processado pela [JVM](https://en.wikipedia.org/wiki/Java_virtual_machine) (Java Virtual Machine). A parte interessante é que o arquivo binário gerado é o mesmo tipo de arquivo gerado pelo compilador do Java.

Eu pensei que Scala era uma linguagem nova (~2010), mas sua criação foi iniciada em 2001 por Martin Odersky, em a primeira versão foi lançada em 2003. Há [algumas entrevistas](https://www.artima.com/scalazine/articles/origins_of_scala.html) com Martin falando sobre como ele surgiu com a ideia de criar uma linguagem melhor que Java, mas que ainda usasse a infraestrutura Java. E isso é algo particularmente estranho: você pode usar o código Java e importar bibliotecas Java dentro do seu código Scala. E isso não é algo raro, muitas coisas em Scala são na verdade objetos Java e podem ser usados em conjunto com as bibliotecas Java padrão. Isso é incrível se você está tentando mudar um projeto de uma linguagem para outra, mas se você não sabe nada sobre Java, parece que na verdade está aprendendo duas linguagens ao mesmo tempo.

Eu também queria ver como as pessoas estão vendo Scala no mercado de trabalho. Meu lugar natural de busca é a pesquisa do Stack Overflow que eles fazem todos os anos. Como o relatório de 2020 ainda não foi publicado, verifiquei [o relatório de 2019](https://insights.stackoverflow.com/survey/2019). Em 2019, Scala foi [listada como a 20º das 25 linguagens mais populares](https://insights.stackoverflow.com/survey/2019#technology), [58% das desenvolvedoras que trabalham com Scala adoram](https://insights.stackoverflow.com/survey/2019#most-loved-dreaded-and-wanted) e é a [17a linguagem mais desejada](https://insights.stackoverflow.com/survey/2019#most-loved-dreaded-and-wanted).

## Quais materiais estou usando para estudar?

Primeiro, comecei a procurar alguns livros recomendados. Em seguida, esbarrei no meu primeiro problema ao tentar passar do Python para o Scala: todos os livros e tutoriais assumem que eu sei Java. E, como você pode imaginar, eu não sei nada de Java!

<center><img src="https://media.giphy.com/media/jxcMNv8wJIlb6Jp9ER/giphy.gif" style="height:300px;"/></center>
<br/>

O primeiro livro que alguém me recomendou, [Functional Programming in Scala](https://www.goodreads.com/book/show/13541678-functional-programming-in-scala), era muito confuso e comparava tudo ao Java. Em vez de explicar os recursos, só falava "isso aqui é igual no Java". E era isso. Ótimo! Agora eu preciso aprender uma linguagem para aprender uma outra linguagem? Que??

Depois disso, comecei a procurar alguns recursos online. O [Tutorials Point](https://www.tutorialspoint.com/scala/scala_classes_objects.htm) foi uma boa maneira de começar a entender variáveis, um pouco de classes e como escrever código. Mas senti que algo estava faltando. Eu não estava satisfeita e decidi continuar procurando um material que me fosse adequado

Finalmente, encontrei um link dos Principais Tutoriais para Aprender Scala e encontrei um curso chamado [Scala & Functional Programming for Beginners](https://www.udemy.com/course/rock-the-jvm-scala-for-beginners/) do [Daniel Ciocîrlan](https://www.udemy.com/course/rock-the-jvm-scala-for-beginners/#instructor-1). Foi um alívio ver as aulas gratuitas e decidiu comprá-lo. Os cursos não pressupõem que você conheça Java e explica as maneiras de pensar e escrever código de uma maneira mais funcional. Até agora, estou gostando muito e recomendo totalmente.

Também recebi uma recomendação impressionante da maravilhosa [Dani Petruzalek](https://twitter.com/danicat83) que ainda não estou estudando por completo: [uma aula do Coursera](https://www.coursera.org/learn/progfun1) e [um livro](https://www.goodreads.com/book/show/5680904-programming-in-scala) de Martin Odersky, o criador do Scala, e essa palestra:


<iframe width="560" height="315" src="https://www.youtube.com/embed/LH75sJAR0hc" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


## O que eu aprendi até agora

### Tipos

Scala tem tipos, mas eles não são obrigatórios para conseguir fazer o código compilar. Assim, semelhante ao Python, se eu fizer `val x = 2`, o compilador entenderá que 2 é um número inteiro e, portanto, `x` também é um número inteiro. O mesmo é válido para os tipos retornados por funções. Exemplo:

```scala
def umaFuncao(a: Int) = a

// mesma coisa que

def umaFuncaoExplicita(a: Int): Int = a
```

Isso é verdade, exceto nas funções *recursivas*. Funções recursivas exigem que a saída seja explícita, caso contrário, o compilador gerará um erro.

### Compilador esperto

Da mesma forma que o compilador deduz o tipo com base no que você colocou, ele também pode fazer alterações com base nesses mesmos tipos. Por exemplo, você acha que o código a seguir funcionará?

```scala
val x = 2 + " bananas"
```

Sim, vai! O compilador verá que você está adicionando um número inteiro a uma *string* e vai chamar a função `2.toString` para converter o número inteiro em uma *string* e evitar um erro de compilação. O mesmo é válido para:

```scala
val x = 2 + "3"
```

O que retornará a *string* "23". Ainda não sei como isso é definido, se *strings* tem prioridade sobre inteiros ou não (parece que sim), mas vou tentar descobrir mais sobre isso 🙂

### Uma função retorna a última coisa 

Quando você escreve uma função, não precisa usar `return` para explicitar o que a função vai retornar. Uma função sempre retornará a última coisa que está dentro de um bloco. No exemplo abaixo, temos um valor que é um número inteiro, mas o retorno da função é uma *string*.

```scala
def umaFuncao(a: Int) = {
  val x = 123 + 456
  "Alguma coisa"
}
```

### Muitas coisas não são obrigatórias! 


Parênteses em funções que não recebem parâmetros também não são obrigatórios. Portanto, `"Hello".sorted()` é o mesmo que `"Hello".sorted`.

O mesmo vale para colchetes para definir o bloco de uma função!

```scala
def outraFuncao(n: Int): Int =
  if (n == 1) 1
  else 2

// mesma coisa que 

def outraFuncao(n: Int): Int = {
  if (n == 1) 1
  else 2
}
```

### Métodos são… diferentes 

Os métodos podem ter *qualquer* nome, incluindo caracteres especiais como `+` ou `%`. E como a gente viu se uma função recebe apenas um parâmetro, você não precisa adicionar os parênteses. Então, basicamente, quando você faz `a + b`, está realmente chamando o método `+` na instância `a` com `b` como parâmetro!


```scala
a + b 

//  mesma coisa que

a.+(b)
```
----------

É isso por hoje 🙂 Vejo você na Parte 2! 
