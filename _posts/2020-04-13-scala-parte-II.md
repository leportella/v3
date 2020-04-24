---
layout: post
title: "Minha saga aprendendo Scala - Parte 2 "
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
permalink: scala-parte-II.html
redirect_from: /pt-br/2020/04/13/scala-parte-II.html
last_modified_at: 2020-04-13T18:25:52-05:00
translation: /scala-II.html
---

Esse artigo é a continuação do artigo [Minha saga aprendendo Scala - Parte 1](https://leportella.com/pt-br/2020/04/01/scala-parte-I.html). 

No último artigo, falei sobre algumas coisas que aprendi sobre Scala. Embora [meu tweet sobre o post](https://twitter.com/leleportella/status/1237322864514281472) tenha gerado alguns comentários sobre a linguagem (bons e ruins), ainda estou decidida a pelo menos [terminar o curso que comecei a fazer](https://www.udemy.com/course/rock-the-jvm-scala-for-beginners/). Até agora, a sensação é de que estou começando a entender algumas coisas e a ficar mais confortável, embora esteja muito longe das coisas mágicas que me assustam.

Outra coisa é que a maioria das comparações e coisas que acho interessantes se baseiam no meu conhecimento de Python e podem ser super normais em outros idiomas. Então lembre-se disso quando estiver fazendo sua leitura 🙂 


## Criando um objeto em Scala

Todas as aulas começam com a criação dee um objeto e escrevemos coisas dentro dele. Esse objeto sempre estende uma classe chamada `App`:


```scala
object MeuObjeto extends App {
  // faça coisas...
}
``

O curso que estou fazendo não foi claro (até agora) por que deveríamos fazer coisas dentro desse objeto ou por que usar um objeto em vez de uma classe. [O tutorial oficial de Scala diz o seguinte](https://www.scala-lang.org/documentation/your-first-lines-of-scala.html):


> *Se esse objeto estender a trait `scala.App`, todas as instruções contidas dentro dessee objeto serão executadas; caso contrário, você precisará adicionar um método `main` que atuará como ponto de entrada do seu programa.*

Então a alternativa seria algo assim:

```scala
object MeuObjeto {
  def main() {
    // faça coisas
  }
}
``

Mas, aparentemente, [também existem algumas vantagens](https://stackoverflow.com/a/11667791/3538098) de usar o `extends App` em vez do método `main`.


## Funções 

Eu **amei** as aulas do curso sobre funções. Foi fantástico. Primeiro porque as funções são definidas de maneira muito simples:

```scala
object MeuObjeto extends App {
  def umaFuncaoSemParametros(): Int = 42

  def umaFuncao(x: Int) = x
}
```

Se você perceber, a primeira função (`umaFuncaoSemParametros`) indica que o tipo de retorno da função é um número inteiro. No entanto, a segunda (`umaFuncao`) não! O compilador do Scala entende que a função retorna um número inteiro e funciona perfeitamente. Eu realmente não gosto de definir tipos enquanto escrevo minhas funções, então isso me parece realmente útil. No entanto, isso não é válido para um tipo de função: funções recursivas. Funções recursivas são os únicos tipos de função que requerem que você especifique o tipo de retorno ou o compilador quebra. 

Falando sobre recursão, gostei muito das aulas sobre recursão no curso. Foi tão incrível que eu gostaria de estudar mais e fazer um post inteiro dedicado a esse tópico.

###  Algumas coisas estranhas**

Enquanto fazia os exercícios, me deparei com um problema. Se eu criar uma função sem parâmetros, posso chamar a função sem parênteses:


```scala
def fn = 1
println(fn)
```

No entanto, se eu definir uma função (mesmo com um valor padrão), eu preciso dos parênteses vazios.

```scala
def fn(i: Int = 1) = 1
println(fn()) // funciona
println(fn) // não funciona
```

Levei algum tempo para descobrir o que estava fazendo de errado quando tentei chamar uma função com um parâmetro padrão sem os parênteses. E a resposta (que os parênteses agora eram necessários) não fazia sentido para mim.

Aparentemente, Scala tem uma diferenciação entre métodos e funções, onde a função é um valor que um método não é. Meu colega Graham me indicou [esta postagem](https://tpolecat.github.io/2014/06/09/methods-functions.html) saber mais, mas a magia Scala já é muito pesada nesse artigo, então não foi tão simples assim entender isso 😅


### Funções dentro de funções**

Você também pode escrever funções dentro de funções, o que é bastante útil para otimizar funções recursivas:

```scala
def funcaoExterna(n: Int): Int = {
  def funcaoInterna(a: Int, b: Int): Int = a + b + n
  funcaoInterna(n, n-1)
}
```


## Strings!

Eu tive uma aula inteira sobre manipulação de *strings* e, pela primeira vez, me senti mais perto do Python! *Strings* são objetos que possuem vários métodos, como o Python, com a diferença de que o Scala é escrito com camelcase: `.toLowerCase`, `.replace`, `.startsWith`, `.length`. 

E você tem três tipos de interpolador de *strings*. Os interpoladores do tipo `s` são bem diretos, eles substituem a variável após o símbolo `$`:

```scala
val nome = "Leticia"
val saudacao = s"Olá, meu nome é $name"
// Olá, meu nome é Leticia
```

Depois você tem `f` interpoladores para lidar com casas decimais em números:

```scala
val altura = 1.7f // isso é um float
println(f"Eu tenho $altura%.2f m de altura")
// Eu tenho 1.70 m de altura
``

Finalmente, você tem os interpoladores do tipo `raw` (brutos), em que caracteres especiais, como o `\n` que indica uma nova linha, não serão interpretados, mas sim interpretados como os caracteres normais `\` e `n` em sequência:

```scala
println(raw"Esta é \n uma nova linha")
// Esta é \n uma nova linha

println("Esta é \n uma nova linha")
// Esta é
// uma nova linha
```
Scala também permite tanto aspas simples `'` quanto duplas `"`, mas o primeiro é um valor do tipo *Char* (caracter) enquanto o segundo é uma *String*. *Char* é basicamente uma estrutura que permite apenas um único caractere de uma *String*. O comportamento padrão é muito diferente se você tenta aplicar funções de String em um Chara:


```scala
println("a" +: 123 :+ "b")
// a123b

println('a' +: 123 :+ 'b')
// Vector(a, 1, 2, 3, b)
```

Ah, sim, os incríveis operadores `+:` e  `:+` apresentados acima também são úteis!

----------

É isso por hoje 🙂 Vejo você na parte 3 (espero)! 

