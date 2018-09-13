---
layout: post
title: "Como revisar código alheio?"
categories:
  - pt-br
tags:
  - pt-br
  - python 
  - testes 
featured-img: feedback
last_modified_at: 2017-03-09T14:25:52-05:00
---

Revisar código é uma tarefa complicada e pode se tornar bastante desesperadora, especialmente quando você não tem ideia de como fazer isso. Entretanto, a revisão de código pode ser uma ferramenta poderosa para aumentar a qualidade do seu código e garantir deploys “saudáveis”.

![](https://cdn-images-1.medium.com/max/800/1*EFsX-ndhmx4CFsI98zSvKA.gif)

Na [Crave Food Services](http://sourcewhatsgood.com/) acabamos por desenvolver uma metodologia que seguimos na hora de corrigir os códigos. Neste artigo, vamos descrever um pouco melhor como funciona nossa metodologia de testes.

Vale ressaltar que este texto não visa discutir a análise estática de código, mas visa dar um guia geral para quem não tem ideia de por onde começar uma revisão de código. O intuito final da metodologia exposta aqui é apenas a qualidade de código e uma ideia prática de como fazer uma boa revisão.

## 1. Funciona?

Todo código é escrito com algum objetivo específico. Pode ser a resolução de um problema, uma nova funcionalidade ou mesmo uma refatoração. Então a primeira pergunta é: “Ele cumpre o objetivo proposto?”. Não apenas acredite que o código funciona. Teste. Se é um endpoint novo, rode localmente e teste-o. Se é uma função, execute-a com os parâmetros corretos e os incorretos também. A função consegue tratar as exceções de forma correta? Tente avaliar possíveis falhas.

## 2. Tem documentação?

Ter documentação é algo que pode te ajudar na primeira tarefa (a de ver se o código funciona). Então talvez verificar se existe documentação e verificar se o código funciona podem ser tarefas atreladas. Entretanto, na maior parte das vezes, o ideal é que o código fale por si só. Caso seja algo que vá ser usado por uma terceira pessoa que não consegue ler o código, ai sim o ideal é documentar o que foi feito.

Documentar adequadamente seu projeto torna tudo mais fácil dado que depois de alguns meses as coisas se perdem. Mesmo que você acredite que é apenas um arquivo com poucas funções, escreva um *readme*. Acredite, você vai esquecer o que fez!

Entretanto, use o bom-senso na hora de usar a documentação! Em excesso, ela pode se tornar cansativa para quem faz e obsoleta para quem usa.


## 3. Está legível? Você conseguiu compreendê-lo facilmente?

Aqui temos uma filosofia de que o código tem que se explicar por si só. Usamos Python que, pra quem não conhece, tem aquela máxima: “simple is better than complex”.

O código normalmente deve falar por si só. Se a coisa está muito complexa para alguém entender, normalmente existe um jeito mais fácil de fazê-la. Existe uma frase ótima que diz tudo: “escreva seu código como se a pessoa que fosse mantê-lo fosse um serial killer que sabe onde você mora”.


## 4. Se o código for complexo, os comentários são suficientes para compreendê-lo?

Ok, nem sempre conseguimos fazer as coisas de um jeito simples. Se o que está implementado é complexo - e deve ser complexo, ou seja, sem condições de simplificação - o código deve conter comentários para ajudar um futuro leitor/mantenedor a compreender o que está escrito. De novo, as memórias se apagam e o código pode ficar completamente indecifrável depois de algum tempo. E sempre que possível lembre-se: “complex is better than complicated”.

## 5. O código está de acordo com o padrão de qualidade?

Em Python, por exemplo, temos a PEP8 que sugere algumas “regras” de boa escrita de código. O código deve estar de acordo com o padrão de código da linguagem e do ambiente de trabalho. Vale lembrar que as regras do ambiente onde você está normalmente sobrepõem as regras gerais. Entenda o ambiente onde o código está inserido e corrija-o pensando em seu “habitat natural”.

Aqui essas verificações são manuais até inserirmos medidores de código automatizados. Por exemplo, nenhum código novo é inserido se não estiver com o Flake8 e o Isort ok.

## 6. Tem testes?

O que falar de testes? Testes ajudam muito na garantia de qualidade do código e evitam muito transtorno (principalmente quando o que você alterou mudou uma parte do sistema que você nem lembrava que existia). Os testes por si só já são uma garantia de qualidade do sistema. Caso exista a política de testes na sua empresa, exija testes. Verifique se os testes adicionados cobrem todos os caminhos (esperados e exceções) e se a cobertura de código não diminuiu.

## 7. Dê feedback

Diga o que achou e sugira melhorias, se for o caso :)

A ideia de uma revisão de código é fazer com que haja uma melhoria (ou manutenção) da qualidade de código no seu ambiente. Uma boa revisão pode contribuir muito para isso. Se essa revisão for aliada a uma suíte de testes e verificações estáticas automatizadas do código, melhor ainda.

Entretanto, é sempre bom lembrar-se que quem escreveu o que você está revisando foi um ser humano como você. Sendo assim, lembre-se que a pessoa pode ter passado por um dia ruim, que pode interpretar um comentário escrito de uma forma diferente da que você escreveu ou mesmo que ela pode não ter pensado numa forma diferente de fazer aquilo. Seja gentil, educado. Proponha, não imponha. Especialmente se você está lidando com quem está começando. Se houver um erro muito grave ou se você acredita que exista uma maneira mais eficiente de fazer a tarefa, chame a pessoa para conversar, troque uma ideia. Códigos são feitos para a máquina, mas quem escreve é bem humano :)
