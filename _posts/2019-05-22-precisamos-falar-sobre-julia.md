---
layout: post
title: "Precisamos falar sobre Julia"
categories:
  - pt-br
tags:
  - pt-br
  - community
  - comunidade
  - sponsorship
  - open-source
  - events
  - technology
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
  - data 
  - data science
  - ciência de dados
featured-img: julia
permalink: julia.html
last_modified_at: 2020-06-24T18:25:52-05:00
---


<center><img src="https://media.giphy.com/media/VMmRM3EjhjBII/giphy.gif" style="height:300px;"/></center>
<center><i></i></center>
<br/>


Julia é uma linguagem de programação que eu tenho muito ouvido falar há algum tempo e eu sei que ela merecia minha atenção. No entanto, a quantidade de bibliotecas e frameworks de aprendizado de máquina e deep learning que surgiram e acabaram entrando na frente atrasaram o meu primeiro contato com ela. Nós do Pizza de Dados](http://pizzadedados.com/), querendo dar o melhor conteúdo pros nossos ouvintes, decidimos fazer [um episódio sobre a linguagem](https://podcast.pizzadedados.com/e/episodio-021/). Foi aí que eu precisei sentar o bumbum na cadeira e de fato olhar para essa desconhecida porém intrigante linguagem. E eu confesso que acabei me apaixonando pelo pouco que estudei! Então a gente precisa conversar sobre essa linguagem maravilhosa que não está tendo a visibilidade que ela merece.

Obs: Eu uso muito da minha base de Python como comparação ao que estava vendo em Julia. Se você está começando agora em programação, ciência de dados e Python, recomendo fortemente ler [este outro texto antes de continuar nesse texto]](https://leportella.com/english/2019/01/25/common-data-science-tools.html) 🙃

Obs 2: O [Pizza de Dados](http://pizzadedados.com/) lançou um episódio sobre Julia que fala mais a fundo sobre a linguagem. Confira este post e [o episódio juntos para uma melhor compreensão](https://podcast.pizzadedados.com/e/episodio-021/).




## Descobrindo mais sobre a linguagem

Julia foi criada em 2012 por Alan Edelman, Stefan Karpinski, Jeff Bezanson e Viral Shah ([Bezanson et al., 2012](https://julialang.org/images/julia-dynamic-2012-tr.pdf)). É uma linguagem gratuita e [de código aberto](https://github.com/JuliaLang/julia), assim como R e Python.

Eu já tinha ouvido de algumas pessoas sobre como Julia é uma linguagem performática, e isso também está descrito em diversos locais do site oficial. O que me impressionou bastante foi que, apesar de ser de alto nível como Python, testes de velocidade colocam a linguagem no mesmo nível de linguagens compiladas extremamente rápidas como Rust ou Go. Veja as comparações do tempo de execução de alguns algoritmos em diferentes linguagens:

<center><img src="https://i.imgur.com/Ail3AU6.png" style="height:400px;"/></center>
<center><i>Fonte: <a href="https://julialang.org/benchmarks/">Site Oficial</a></i></center>
<br/>


E, de fato, esse foi o principal objetivo: a performance de uma linguagem estaticamente compilada (como C e Fortran) com o comportamento interativo/dinâmico e produtividade de linguagens como Python e Ruby  ([Bezanson et al., 2012](https://julialang.org/images/julia-dynamic-2012-tr.pdf)).

Muitas pessoas citam que a compilação Just-In-Time (JIT) de Julia é o principal motivo da velocidade da linguagem. No entanto, outras linguagens como R e Python também usam esse tipo de compilação. [Este tutorial](http://ucidatascienceinitiative.github.io/IntroToJulia/Html/WhyJulia), mostra que diversas decisões no design do código foram fatores que contribuíram mais do que o JIT.


Ainda pode-se citar que a linguagem [é feita para permitir concorrência, paralelismo e computação distribuída](https://en.wikipedia.org/wiki/Julia_%28programming_language%29). Também é possível [chamar diretamente bibliotecas em C e Fortran sem necessidade de uma biblioteca intermediária](https://docs.julialang.org/en/v1/).


## Por onde começar?

O próprio site da Julia tem [uma lista de materiais para estudar, fora a documentação oficial](https://julialang.org/learning/). O livro [ThinkJulia](https://benlauwens.github.io/ThinkJulia.jl/latest/book.html), baseado no famoso [ThinkPython](http://shop.oreilly.com/product/0636920025696.do), foi um bom lugar pra começar. Além disso, se você já tem uma base em R, Python, MATLAB ou C/C++, [a documentação lista quais as principais diferenças entre Julia e a sua linguagem base](https://docs.julialang.org/en/v1/manual/noteworthy-differences/). Simplesmente sensacional!

Vale também ressaltar que em Abril de 2018 a maravilhosa Prof. Melissa fez uma palestra chamada “[Julia para Pythonistas](https://www.youtube.com/watch?v=Vhfkl97Zlfg&list=PLUcaP2aQdcK_e5ghxrqJABnjgW_Rxs8MN&index=13)” na Python Sul e foi um material excelente de referência e em português.



## Instalação

Foi bem fácil de instalar. [Na página de Downloads do site oficial eu baixei um instalador](https://julialang.org/downloads/) para MacOs e deu! Eu abri o programa e descobri que, na verdade, o que é aberto é um terminal simples que executa o binário que está localizado numa pasta. Assim:


```
exec /Applications/Julia-1.1.app/Contents/Resources/julia/bin/julia
```


Então, pra facilitar a vida, eu adicionei no meu `.bashrc` um `alias`, de forma que toda vez que eu escrevesse `julia`, ele na verdade chamasse esse arquivo.


```
alias julia="/Applications/Julia-1.1.app/Contents/Resources/julia/bin/julia"
```

Uma vez feito isso, tudo estava rodando normalmente 😜

## Instalando pacotes

Julia conta com [uma extensa coleção de pacotes](https://juliaobserver.com/), semelhante ao [PyPi](https://pypi.org/) do Python. Para instalar um pacote é só digitar `]` dentro do interpretador que ele "transforma" o interpretador num instalador. Dá uma olhada:


Julia has [an extensive collection of packages](https://juliaobserver.com/), similar to Python's [PyPi](https://pypi.org/). 
To install a package just type `]` inside the interpreter that it "transforms" the interpreter into an installer. Take a look:


<center><img src="https://cdn-images-1.medium.com/max/1600/1*DkyKrnt1spV_oFm9Gkyang.gif" style="height:300px;"/></center>
<center><i>Instalando um pacote em Julia</i></center>
<br/>

Nesse caso acima eu instalei o pacote do `ThinkJulia`, o livro que eu segui para estudar para esse texto. Para usar o pacote eu devo declarar dentro do interpretador que eu quero usar o pacote `ThinkJulia`:

```julia
julia> using ThinkJulia
```

[E agora todas as funções do pacote estão disponíveis no sistema:](https://benlauwens.github.io/ThinkJulia.jl/latest/book.html#chap04):

```julia
julia> using ThinkJulia
julia> 🐢 = Turtle()
Luxor.Turtle(0.0, 0.0, true, 0.0, (0.0, 0.0, 0.0))
```

Uma outra forma de adicionar pacotes sem abrir o instalador padrão é usar um pacote para instalar demais pacotes. Podemos declarar que queremos usar o pacote `Pkg` do mesmo jeito que chamamos o `ThinkJulia` e, a partir daí, usar a função `.add()` para fazer a instalação de fato:

```julia
julia> using Pkg
julia> Pkg.add("ThinkJulia")
```

## Usando Julia em Notebooks

É possível usar [Jupyter Notebooks](https://jupyter-notebook-beginner-guide.readthedocs.io/en/latest/what_is_jupyter.html) com linguagens diferente de Python. [Uma longa lista de kernels](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels) aceitos pelo Jupyter está disponível e o IJulia (em referência ao [IPython](https://ipython.org/) que foi a base do Jupyter) está entre eles:


```julia
julia> using Pkg
julia> Pkg.add("IJulia")
```

Uma vez instalado o kernel, podemos chamar o pacote do `IJulia` e chamar a função notebook:

```julia
julia> using IJulia
julia> notebook()
```

No meu caso, o programa pergunta se eu desejo instalar o Jupyter via [Conda](https://docs.conda.io/en/latest/miniconda.html) e, apesar de ter ambos em [Ambientes Virtuais do Python](https://docs.python.org/3/tutorial/venv.html), uma série de pacotes foram instalados juntos:

<center><img src="https://i.imgur.com/BOyniWX.png" style="height:200px;"/></center>
<center><i>Instalações que são feitas quando chamamos o Jupyter Notebook pela primeira vez dentro do terminal do Julia</i></center>
<br/>


Tendo tudo instalado, a tela padrão do Jupyter Notebook abriu e agora além de Python 2 e 3 eu também tinha Julia como opção:

<center><img src="https://i.imgur.com/eQQVhQV.png" style="height:300px;"/></center>
<center><i>Ao tentar criar um Jupyter Notebook, passei a ter a opção de Julia</i></center>
<br/>


## Iniciando os trabalhos


### Nomeando variáveis

As definições de variáveis é igual ao que encontramos em Python. Com uma diferença: Julia tem suporte extensivo a Unicode. Isso significa que você pode ter nomes de variáveis com letras em japonês, acento e até emojis 😰.

<center><img src="https://i.imgur.com/xcFTZxr.png" style="height:300px;"/></center>
<center><i>Julia tem um suporte extenso a Unicode.</i></center>
<br/>


[Nem todos os caracteres unicode estão disponíveis para nomear variáveis](https://benlauwens.github.io/ThinkJulia.jl/latest/book.html#characters). O caracter `@`, por exemplo, não pode ser usado. Mas é bem interessante pensar nas possibilidades, especialmente considerando países cujas línguas nem usam o alfabeto latino.


### Criando um pequeno script


Arquivos Julia tem a extensão `.jl` e podem ser executados da seguinte forma:


```
julia my_file.jl
```

Eu criei um arquivo chamado `julia1.jl` chamando a função `println` para imprimir um texto curto na tela:


```julia
# julia1.jl
println("Minha 🍕 favorita é de 🎲🎲")
```

<i>Meu primeiro script em Julia!</i>

Logo de cara já caí num erro: vindo de Python, eu sempre vario entre o uso de aspas simples (`'`) e aspas duplas (`"`). Ao usar aspas simples, recebi um erro de sintaxe, avisando que eu havia usado um caractere inválido.



### Strings

Algumas coisas nas manipulações de string me chamaram a atenção. A primeira é que a concatenação de strings não usa o item de soma, mas o de multiplicação! Então `"olá" + " mundo"` retorna um erro enquanto a expressão `"olá" * "mundo"` retorna o resultado que queremos: `olá mundo`.

Diversas funções de manipulação de strings já vem por padrão. Como a `isreverse`que retorna se uma string é o inverso da outra, `inboth` que retorna os caracteres comuns em duas strings, e a `findfirst` que retorna o intervalo onde uma determinada string se encontra em outra string.


### Símbolos matemáticos estão na moda!

Nas várias coisas que li sobre Julia, é sempre dito que ela é uma linguagem que traz muita bagagem da matemática. Não pra minha surpresa, para se verificar que uma letra está dentro de uma palavra, usa-se o símbolo matemático de “pertence a um conjunto”:

```
julia> "a" ∈ "banana"
true
```

E o valor de pi também já está disponível...

```
julia> π
π = 3.1415926535897…
```

No interpretador você pode acessar esses e outros símbolos semelhante ao que se faz no LaTex: `\in + Tab` vira o `∈` enquanto `\pi + Tab` gera o `π`.


### Funções

Semelhante ao MATLAB/Octave, Julia fecha os blocos com a instrução `end`. Portanto, podemos adicionar nosso `println` dentro de uma função como esta:

```julia
# julia2.jl
function favorite_pizza()
  println("Minha 🍕 favorita é de 🎲🎲")
end

favorite_pizza()
```

E uma função que recebe dois parâmetros vai ser escrita da seguinte forma:


```julia
function mysum(x, y)
  x + y
end

# returns 5
mysum(2, 3)
```

Mas lembra que Julia é uma linguagem toda focada em matemática? [Podemos reescrever a mesma função da seguinte forma](https://docs.julialang.org/en/v1/manual/functions/):


```
julia> soma(x, y) = x + y
```


Muito legal e muito fácil de visualizar. Mas aí comecei a ler mais e a cabeça foi explodindo… em Julia, os operadores (`+` , por exemplo) são apenas funções com características especiais. Então a soma de alguns elementos pode ser feita chamando a função soma, ou o `+` nesse caso:


<center><img src="https://i.imgur.com/93wMaPR.png" style="height:200px;"/></center>
<br/>


<center><img src="https://media.giphy.com/media/Ysce790SgjJK0/giphy.gif" style="height:200px;"/></center>
<br/>


## Vamos parar por aqui

Uma linguagem é sempre um mundo novo a ser descoberto. Esse texto foi apenas um relato da prazerosa (e louca) experiência de começar a entrar nesse mundo novo e bastante interessante. Como falamos no episódio do Pizza de Dados, a linguagem é recente e ainda há muito a se fazer: muitos pacotes ainda precisam ficar estáveis, a comunidade deve aumentar para criar buscas mais fáceis por erros e assim por diante. Mas com certeza, essa é uma linguagem que nasceu com muito potencial!


---
Se interessou? Não deixe de ouvir o “Episódio 021: Precisamos falar sobre Julia” e conferir o post do episódio para checar os links!

<center>
<iframe width="560" height="315" src="https://www.youtube.com/embed/LDHWQgtcMaI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</center>


