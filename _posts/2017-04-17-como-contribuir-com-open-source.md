---
layout: post
title: "Como contribuir para um projeto open-source pela primeira vez sem escrever código"
categories:
  - pt-br
tags:
  - pt-br
  - open-source
  - begginers
  - study
  - code
featured-img: open
last_modified_at: 2017-03-09T14:25:52-05:00
---

**Disclaimer: Em um post antigo, eu frizei como foi decepcionante escrever 3 meses de documentação. Entretanto, existem um ponto que deve ser abordado aqui: existem momentos para se escrever código e momentos para escrever documentação. Me frustrei ao só fazer isso o tempo todo durante um trabalho como programadora. Isso me gerou muito desconforto em escrever documentação durante um tempo. Eu consegui perceber eventualmente que uma boa documentação de código pode poupar tempo, trabalho e frustrações ao se desenvolver o projeto.**

Começar a contribuir para um projeto open-source (de código aberto) é sempre algo muito recomendado para quem está começando a programar. Muitos comentam que é uma excelente forma de mostrar o seu potencial como desenvolvedor e algumas empresas perguntam sobre contribuições como forma de avaliação de currículo. Além disso, quando você contribui para um projeto open-source, você está ajudando e agregando valor a toda uma comunidade empenhada em soluções comuns e abertas (ou seja, é lindo ❤).

Entretanto, quando eu perguntei sobre como eu poderia começar a contribuir (ou sempre que me perguntaram como poderiam contribuir) sempre ouvi/respondi algo tipo 
“procure um projeto no github”. Grande novidade né? **Que projeto? Como? Porque? O que diabos eu deveria estar fazendo?** Eu estou num processo de tentar me 
envolver mais com projetos open-source então fiz um guia de como ajudar um projeto usando como base uma contribuição que quis fazer em um projeto.

Uma coisa que eu gostaria muito que você tivesse em mente é que contribuir nem sempre implica em escrever código. Existem **pull requests**¹ que precisam de revisão, 
**issues** (novos problemas/novas tarefas) que precisam ser criadas para que os desenvolvedores saibam dos problemas do código e existe documentação que é falha. Hoje, vamos focar num caso de documentação.

**¹Pull requests são visualizações de alterações no código feitas ao se tentar adicioná-lo em um repositório.**
**[Da documentação do GitHub](https://help.github.com/articles/about-pull-requests/): Isso permite que os outros vejam as mudanças que você quer inserir em um repositório. Uma vez que um pull-request é criado em um repositório, você pode discutir e revisar as potenciais mudanças com outros colaboradores e adicionar mais pedaços de código com mudança antes que o seu código seja aceito e adicionado no repositório.**

Eu sei que para um programador ávido isso até pode parecer um pouco estranho ou mesmo desencorajador. Mas existe muito valor em se escrever documentação. Muitos projetos legais têm problema de documentação esparsa ou, às vezes, incoerente. E digo mais: para se escrever uma boa documentação é preciso, muitas vezes, horas em cima do código do projeto para entender o que ele faz e explicar para os usuários de maneira didática. Então, vamos fazer uma contribuição juntos? Vem comigo.


## Crie um projeto seu

Dica simples: é muito difícil você contribuir para um projeto que você desconhece. Então, tente fazer um projeto a parte. Grande ou pequeno, com objetivos profundos ou não. Comece a desenvolver algo que seja do seu interesse. Há grandes chances de que você precise de outros módulos e pacotes para desenvolver o seu projeto. Agregar valor a um projeto que você usa (e vê valor) é algo muito mais motivante do que escolher um projeto ao acaso. No meu caso, comecei um projeto em Django e queria adicionar autenticação via GitHub.

## Encontrando a dificuldade

Descobri que o módulo mais usado para esse caso era a [social-app-django](https://github.com/python-social-auth/social-app-django) do 
[python-social-auth](https://github.com/python-social-auth/). Fui ler a documentação e fiquei confusa. Não entendi o que deveria fazer e nem como fazer. 
Sofri bastante e viajei pelo código tentando entender. Finalmente, encontrei [um artigo](https://simpleisbetterthancomplex.com/tutorial/2016/10/24/how-to-add-social-login-to-django.html#comment-3233186393) do Vitor Freitas que me explicava tudo passo a passo. 
Lindo, funcionou. Vi aí uma oportunidade de melhoria da documentação. Vi outras ferramentas que poderiam ser utilizadas e que não estavam nem na documentação, nem no artigo.
Opa! Veio a ideia: isso aí eu posso contribuir!

## Buscando mais informações

Os grandes projetos normalmente tem um arquivo para indicar como eles incentivam que as contribuições ocorram. No caso do social-app-django, encontrei o guia de contribuição deles:

![](https://i.imgur.com/r69gIJ9.png)

A partir daí eu soube que o primeiro passo é ter uma conta no GitHub (check!), submeter um **issue** falando sobre o problema em si e, se possível, fazer um **fork** do projeto. Vamos por partes.

## Criando uma issue

Seguindo as instruções criei um **issue** explicando meus problemas, mostrando o artigo que encontrei e dizendo que eu estava disposta a fazer essa contribuição para o projeto.
Se fosse um problema com o código, seria recomendado dizer os passos que fez para encontrar o problema. Coloque fotos da sua tela ou do resultado de erro.

É sempre importante lembrar também que, por mais que você esteja tentando ajudar, gerenciar um projeto desse tipo é normalmente complicado. 
As pessoas desenvolvem esse tipo de projeto em seu tempo livre, na maior parte dos casos. Nem sempre elas tem tempo (e vontade) para ler um **issue** mal escrito ou mal explicado. 
Seja educado, prestativo e não se irrite se demorar a receber respostas!

No meu caso, eu expliquei que tive problemas, mostrei aonde consegui ajuda e me coloquei disposta a melhorar a documentação.

![](https://i.imgur.com/m4FqAxB.png)
**[Minha issue aberta no social-app-django](https://github.com/python-social-auth/social-app-django/issues/44) explica o problema, mostra onde encontrei a solução e me digo disposta a ajudar**

Apesar de não ter recebido resposta depois de 8 dias, decidi continuar minha empreitada.

## Criando um fork

Verifiquei  que a documentação não estava no mesmo repositório e sim no repositório **social-docs**. 
Primeira coisa que fiz foi um *fork* do repositório (conforme orientavam no documento de contribuição). Ou seja, uma cópia do projeto todo no [meu GitHub pessoal](https://github.com/leportella/social-docs). 
Dessa forma, eu poderia fazer as alterações e enviar pedidos de **pull request**.

Então, encontrei as duas partes da documentação que eu gostaria de alterar. Eu gostaria de contribuir para a documentação geral do uso do módulo no [Django](https://github.com/python-social-auth/social-docs/blob/master/docs/configuration/django.rst) e a parte mais específica de autenticação com o [GitHub](https://github.com/python-social-auth/social-docs/blob/master/docs/backends/github.rst).

## Alterando os arquivos

Criei uma **branch** separada da **branch** *master* do meu **fork** , para fazer as alterações dos documentos. Essa **branch** seria enviada diretamente para o social-docs em formato de um **pull request**.

Busquei a documentação, li o código e finalmente, consegui fazer dois **pull requests**, um para cada um dos documentos.

Estava feita a minha contribuição **open-source**.

![](https://i.imgur.com/gWQUTE1.png)

Isso pode até parecer simples para algumas pessoas, mas eu acabei demorando algumas horas de desenvolvimento tentando entender e aplicar a autenticação por GitHub. Com sorte, esta minha pequena contribuição poderá ajudar os próximos desenvolvedores a não demorarem tanto tempo. Então quem sabe você que não tem ideia por onde começar a contribuir comece da mesma forma? Teve uma dificuldade usando uma biblioteca? Porque não ajudar alguém a ter uma documentação melhor?

##### Se quer ir rápido, vá sozinho. Se quer ir longe, vá em grupo. (Provérbio Africano)

![](https://cdn-images-1.medium.com/max/800/1*b4np_iaYln1XAJVu_ttd9w.gif)
