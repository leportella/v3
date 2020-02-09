---
layout: post
title: "Como é ser uma nova engenheira de software na Stripe"
categories:
  - pt-br
tags:
  - pt-br
  - eventos
  - tecnologia
  - Stripe
  - San Francisco
  - Dublin
  - Software engineer
  - software engineering
  - engenheira de software
  - desenvolvedora
  - mulheres que codam
  - mulhers desenvolvedoras
  - programadora
  - events
  - payments
  - development
  - women in tech
  - woman in tech
  - girls who code
  - career
  - computer science
  - self-taugh engineer
  - self-learning
  - programmer
  - ruby
  - scala
  - python
featured-img: stripe
last_modified_at: 2020-02-09T18:25:52-05:00
---

Ser engenheira de software em uma nova empresa - em qualquer nova empresa - é sempre difícil. A base de código é completamente nova, você precisa se adaptar a novos padrões (tanto de código quanto culturais) e, provavelmente, o tipo de problema que você vai resolver também é completamente novo para você.

Quando entrei na [Stripe](https://stripe.com/au), eu sabia que seria um desafio: esse era meu primeiro emprego em uma empresa de tecnologia em rápido crescimento, eu teria que usar uma nova linguagem de programação, iria trabalhar com pagamentos e, pela primeira vez, trabalharia em inglês em tempo integral. Mesmo sabendo de tudo isso, eu não tinha ideia de como seria desafiador e emocionante. No entanto, mesmo que tenha sido um dos maiores desafios que tive na minha carreira, a experiência de integração foi um processo incrível e interessante. Decidi compartilhar uma visão geral de como é ser uma nova engenheira aqui na Stripe, logo após ter passado por todo esse processo.


## Primeiras semanas: morando no exterior

Eu voei de Dublin para San Francisco em uma manhã de domingo, um dia antes do meu primeiro dia na empresa. O primeiro desafio real foi lidar com o fuso horário: 8 horas de diferença. No dia seguinte, com uma mistura de ansiedade e [*jet lag*](https://pt.wikipedia.org/wiki/Jet_lag), acordei às 4 da manhã. Não conseguia dormir de jeito nenhum.

Cheguei ao escritório e encontrei uma mesa enorme com uma placa: “Mesa para novos contratados”. Tomei café da manhã e conheci a maioria das pessoas da minha turma. Assim como em uma universidade, todo mundo que começou no mesmo dia forma uma turma. Na primeira semana você entra em um ritmo intenso de aulas. Você aprende sobre o que é a Stripe: as coisas boas e as coisas em que precisam ser melhoradas, como conversar com os clientes e como o mundo dos pagamentos opera. Bem-vinda à Stripe 101!

Devido ao ritmo intenso e ao assunto denso, é muito reconfortante ter uma turma de iniciantes para se apoiar. Eles ajudam você a entender as coisas com as quais está tendo dificuldade e você pode compartilhar como está se sentindo. Como todos que estão começando na empresa estão lá, você conhece pessoas de todos os times e lugares do mundo! Eu conheci pessoas do Japão, Cingapura, Irlanda e EUA que estavam se juntando à equipe de vendas, operações, crimes financeiros, jurídica e, é claro, à equipe de engenharia.

Durante essa primeira semana trabalhamos com diferentes equipes todo dia e sempre tentamos almoçar e tomar café da manhã com pessoas diferentes. Embora estivéssemos em uma grande turma - quase 50 pessoas - fui capaz de passar pelo menos um pouco de tempo e conhecer a maioria da turma. No final da semana, eu pude entender tudo sobre os produtos da Stripe, como é feita uma cobrança, como nossos clientes nos veem e para que direção a empresa está indo.

Em nossa segunda semana ainda tínhamos algumas aulas no Stripe 101, mas começamos as aulas da Engenharia 101. Nestas aulas especializadas aprendemos sobre a arquitetura geral do Stripe, padrões de design, linguagens de programação e muito mais. Conseguimos visualizar melhor como os projetos são executados, os tipos de problemas que podemos enfrentar e como as pessoas trabalham no dia-a-dia.

<center><img src="https://imgur.com/NPokNqK.png" style="height:300px;"/></center>
<center><i>Eu depois do primeiro dia!</i></center>
<br/>


## Reconhecendo a dificuldade

Enquanto escrevo isso, vejo que o que eu escrevi não explica o quão difícil foram essas duas primeiras semanas. Uma das coisas que eu mais amei foi que a primeira coisa que nos foi dita em nossa primeira aula foi um lembrete gentil sobre [a síndrome do impostor](https://pt.wikipedia.org/wiki/S%C3%ADndrome_do_impostor). Se você não está familiarizada com o conceito, a síndrome do impostor é um sentimento infundado de que algum dia alguém perceberá que você é uma fraude - e que de alguma forma você será revelada como uma. Geralmente aquelas que tem essa síndrome sentem que realmente não merecem seus sucessos e que tudo o que alcançaram é principalmente o resultado da sorte.

Esse pequeno recado de cinco minutos focava em nos dizer que pertencemos à Stripe, que estávamos lá por uma razão e que todos os começos são difíceis. Não me senti uma impostora quando ouvi essas palavras, mas foi muito reconfortante ouvi-las. No entanto, no final da semana, essas palavras fizeram toda a diferença para mim. Em alguns momentos, eu tive certeza de que não sobreviveria às duas primeiras semanas e seria demitida. Eu tinha certeza de que minha contratação era um erro e que eles perceberiam isso eventualmente. Repeti as palavras do primeiro dia várias vezes para ter certeza de que não iria surtar.


## Projeto /dev/start

Depois de terminar as aulas que eu comentei acima, você é agrupada com outras engenheiras recém-contratadas para desenvolver um projeto em conjunto. Este é um projeto pequeno e bem definido, projetado para ser concluído em duas semanas. Cada equipe recebe um mentor para ajudar a entender como as coisas são construídas na Stripe. A ideia é incrível: quando ninguém sabe nada sobre o sistema, ninguém tem medo de fazer perguntas básicas. Então você tem um grupo de pessoas para se apoiar e aprender juntas. O foco não é apenas finalizar o projeto, mas criar novas interações, aprender o básico dos processos operacionais (incluindo *deploys*) e criar confiança.

Minha equipe /dev/start tinha pessoas que mais tarde se uniram a equipes muito diferentes da minha, pessoas com as quais eu talvez nunca mais venha a trabalhar. Desenvolvemos uma ferramenta para explorar e pré-visualizar arquivos em buckets S3 em uma ferramenta online interna (sem precisar fazer o SSH em uma máquina de produção). Meu projeto usou Scala, uma linguagem que eu nunca trabalhei antes. Eu estava com muito medo de ser inútil para o time, porque nunca havia trabalhado com uma linguagem JVM como Scala. Foi quando comecei a duvidar de mim e do que estava fazendo lá. Conversei com as pessoas da minha equipe e contei sobre minhas lutas. Eu disse que Stripe podia ter cometido um erro ao me contratar. Adivinha? Todos sentiram algo semelhante: estávamos todos inseguros. Acabamos ajudando um ao outro: pareando para escrever código e conversando quando as inseguranças ressurgiam. Não sei dizer o quão incrível e importante essa experiência foi para mim. Depois de apenas alguns dias, estávamos produzindo código, fazendo *deploy* das nossas mudanças e nos sentindo confortáveis com as principais ferramentas. Formamos uma ótima equipe e pudemos implantar nossa ferramenta antes do final da segunda semana do projeto.

<center><img src="https://imgur.com/wKhtZIt.png" style="height:300px;"/></center>
<center><i>Minha equipe e eu jantando juntos</i></center>
<br/>

## Me sentindo confortável

Antes de começar ao Stripe, uma engenheira de sua futura equipe é designado para ser seu *spinup buddy* (colega de início). Essa colega te acompanha desde do dia 0, compartilhando sobre sua experiência e ajudando com todos os tipos de coisas, desde perguntar onde fica o banheiro até como funcionam os processos e normas culturais da empresa.

Como eu morava em Dublin, meu *spinup buddy* estava em Dublin enquanto eu estava em San Francisco. Os fusos horários e a distância não facilitam a comunicação então você também recebe um *buddy* local em San Francisco nas primeiras semanas. Durante meu período em SF, eu me encontrava com meu *spinup buddy* local três vezes por semana para falar sobre meu projeto, a empresa ou qualquer outra coisa em minha mente.

Quando cheguei a Dublin, conheci minha equipe de longo prazo. Uma das tarefas de um *spinup buddy* é definir um projeto inicial para você. Esse também deve ser um projeto pequeno e bem definido dentro da sua equipe, que deve durar algumas semanas. A ideia é que você comece a entender mais sobre sua equipe e o projeto que estará trabalhando. Passei muito tempo sozinha tentando aprender mais sobre as coisas, mas, na maioria das vezes, sentava e fazia pareava (*p*air programming) com meu colega, para que eu pudesse obter ajuda sobre problemas em que estivesse com dificuldade.


## Cultura 1: 1

Uma parte importante da cultura da Stripe é ter reuniões chamadas de 1:1 (um a um) com muitas pessoas. As pessoas começam a adicionar reuniões ao seu calendário logo nas primeiras semanas! Embora muito diferente no começo, essa foi uma experiência interessante: eu conheci pessoas de todas as equipes e escritórios e fiz vínculos que seriam muito difíceis na comunicação apenas virtual. Isso aconteceu nas minhas primeiras duas semanas, mas nunca o hábito nunca foi realmente embora. É normal que as pessoas que visitam seu escritório agendem algum tempo para conhecê-la ou discutir sobre algo em que estão trabalhando. Agora que tenho alguns meses de empresa, sou eu quem está agendando esses encontros com as novas contratadas.


## Oportunidades de aprender

Dentro da empresa existe uma equipe específica, chamada "Educação", responsável pelo lançamento de novos tutoriais, palestras e cursos. Temos um sistema interno de aprendizado que permite pesquisar novos cursos ou assistir novamente uma aula do Stripe 101. A equipe está constantemente criando novos conteúdos e solicitando feedback após o término de um curso, o que significa que a qualidade do material educacional aumenta com o tempo.

No escritório de Dublin, a equipe de engenharia hospeda palestras chamadas “Lunch and Learn” (Almoce e Aprenda), onde alguém apresenta sobre um assunto enquanto todos estão almoçando. Isso pode ser específico da empresa ou sobre algum tópico aleatório - já tive até aula de photoshop numa dessas!

Também temos grupos de estudo. A cada duas semanas, você pode se reunir com um grupo de estudos sobre Ruby ou participar de uma discussão sobre um artigo. Você também pode encontrar muitos livros espalhados pelos escritórios, a maioria deles da [Stripe Press](https://press.stripe.com/), mas também temos uma biblioteca on-line e orçamento para comprar novos livros para nossos escritórios.

Não há desculpa para não aprender algo novo!


## Lidando com uma quantidade esmagadora de informações

Como você pode imaginar, era (é) MUITA informação. E quanto mais tempo passa, apenas aumenta. Rapidamente eu descobri que é extremamente importante desenvolver estratégias para aprender e absorver tanto conteúdo útil.

**Checklists** 
Recentemente, algumas engenheiras e eu nos reunimos para criar uma checklist de coisas que cada nova engenheira deve fazer e ler nas primeiras semanas.

Essa checklist vai da semana 1 à semana 12 (~90 dias desde o seu início) e inclui tarefas a serem realizadas, pessoas que você deve conhecer e coisas que você pode ler. Para evitar ser enterrada no que fazer, dividimos as tarefas de cada semana em obrigatórias e opcionais. Dessa forma, uma nova engenheira pode priorizar as tarefas que devem ser executadas e ainda ter recursos para ler quando quiserem, mas sem a pressão de fazer tudo.

**Mantendo um diário**
Algo que funciona muito bem para mim ao longo do dia-a-dia é criar um diário de trabalho. Isso significa que todos os dias listo as tarefas que realizei, as pessoas com quem conversei (e sobre o que falamos), links úteis etc. etc. Atualmente, estou usando o aplicativo desktop do [Bear App](https://bear.app/). Esta é a minha ferramenta favorita de todos os tempos!

Com o [Bear App](https://bear.app/), crio uma anotação por dia e adiciono informações sobre o que fiz, pessoas que conheci e coisas que não posso esquecer; tudo isso organizado através de hashtags e sub-hashtags. Dessa forma, é possível filtrar as anotações sobre reuniões que eu me encontrei com o `#People/Bob` para falar sobre o `#Project/ABC`. É maravilhoso. Quase toda a minha equipe agora usa o Bear para manter suas anotações e se organizar.

Aqui está um exemplo de como eu organizo minhas anotações diariamente:

<center><img src="https://imgur.com/d14maPo.png" style="height:300px;"/></center>
<center><i></i></center>
<br/>

Eu também uso este diário como forma de guardar meus aprendizados. Se descubro como fazer algo que não é tão simples, ou se passei muito tempo tentando resolver um bug, registro o problema e as soluções lá. Esse diário me faz ter a  sensação pessoal de melhoria, de que estou evoluindo mesmo nos dias que eu passei lutando com alguma coisa. E é realmente útil ter uma lista do que você fez nas reuniões diárias da equipe. Por exemplo, se houver uma sequência de comandos que desejo armazenar, posso adicionar tags como `#code/tool` e `#code/project-abc`.


## Brag document 

O último recurso que eu uso é um *brag document,* ou, em tradução literal, um documento para gabar-se. Neste documento você deve compartilhar suas principais realizações ao longo do tempo. Embora meu diário de trabalho seja bem granular, com pequenos detalhes que ocorrem diariamente, meu *brag document* registra grandes realizações que acho que deveriam ser consideradas na minha avaliação de performance. [A Julia Evans fez um trabalho incrível explicando isso, então vá lá e descubra mais](https://jvns.ca/blog/brag-documents/) (o post está disponível apenas em inglês, infelizmente). Os *brag documents* são úteis tanto para seus colegas no momento de escreverem a sua avaliação de performance, como também para criar confiança, olhando para trás em todas as coisas que você realizou. 


----------

Bem, foi uma jornada louca e emocionante, e eu não posso acreditar na rapidez com que seis meses se passaram. Espero que você tenha gostado de aprender mais sobre como é ser uma nova engenheira na Stripe.
