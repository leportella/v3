---
layout: post
title: ""
categories:
  - pt-br 
tags:
  - pt-br
  - open-source
  - python
  - mongo
  - code
  - mongoengine
  - database
  - databases
  - sql
  - nosql
  - mongoDB
  - technology
  - tecnologia
  - programador
  - programadora
  - mulheres na tecnologia
  - woman in tech
  - girls in tech
  - computação
  - ciência de computação
  - software development
  - engenharia de software
  - desenvolvimento
  - auto-ensino
  - self-taught engineer
featured-img: database
permalink: checklist-ml.html
last_modified_at: 2018-08-24T15:41:52-05:00
---

Quando começamos a analisar um novo conjunto de dados, é normal se sentir um pouco "intimidado" pela quantidade enorme de possibilidades que existem na sua frente.

Eu, particularmente, gosto de tentar, rapidamente, fazer um modelo rápido e "sujo" (quick and dirty) pra ter uma ideia de quão difícil vai ser prever uma variável alvo.

Às vezes, me perco na quantidade de coisas que eu fiz/pretendo fazer e o negócio desanda. Pra garantir a qualidade dos projetos que eu pego pra fazer, criei uma checklist que eu tento sempre passar o olho pra ver se não deixei de fazer algo. Agora, deixo ela pública pra te ajudar!


<center>
  <img src="https://media.giphy.com/media/aSZSj0mT8f6tW/giphy.gif" style="height:300px;"/>
</center>
<br/>


## 1. Os dados que você está usando estão limpos?


A primeira coisa que eu faço é garantir que tenho uma planilha final limpa e organizada. Eu crio um notebook que vai conter todas as minhas limpezas e exporto o resultado final no arquivo que eu de fato vou usar nas minhas análises.

Isso não foi sempre assim. Porém, deixar para fazer alguma limpeza num arquivo cujo objetivo é ter ideias sobre os dados, por exemplo, é uma péssima ideia. Você nunca vai ter certeza quais limpezas você fez no dado ou se as alterações que você precisa estão de fato no dado.


## 2. Existem dados ausentes? Qual o percentual deles?

Você já olhou por todos os seus dados? Sabe quais deles têm altos percentuais de dados ausentes? Como você decidiu resolver isso?

Muitas vezes remover as linhas que tem dados relevantes ausentes pode ser uma solução ideal. [Às vezes, precisamos de outras técnicas mais complexas](https://medium.com/databootcamp/o-que-fazer-quando-faltam-dados-255ef5508a4f). Mas é sempre bom saber exatamente qual informação está amplamente disponível e qual terá que ser descartada.


## 3. Como as features se relacionam?

Estudar o comportamento de cada *feature*, se elas são normais ou não, e como elas se relacionam é fundamental. Você já estudou cada uma das suas *features*, entendeu seus padrões e anotou os resultados?

Por exemplo, o preço de uma casa pode ter uma alta correlação com sua metragem em metros quadrados. Claro que esse caso é bem intuitivo, o foco é tentar encontrar correlações que não são rápidas de se pensar.

Eu tento sempre criar um notebook (ou um pra cada variável dependendo do caso), contendo todas as minhas análises descritivas das variáveis.

## 4. Você visualizou os dados? Tentou buscar padrões?


Gráficos, gráficos, gráficos! Faça gráficos. Seja criativo!

Estatísticas podem te enganar. Nossos cérebros conseguem interpretar gráficos de forma muito mais fácil do que números numa planilha. O exemplo abaixo é sensacional: todos os gráficos mostrados tem as mesmas características estatísticas em duas casas decimais de precisão, mas com uma imagem gráfica completamente diferente [[1]](https://www.researchgate.net/publication/316652618_Same_Stats_Different_Graphs_Generating_Datasets_with_Varied_Appearance_and_Identical_Statistics_through_Simulated_Annealing).

<center>
  <img src="{{ site.url }}{{ site.baseurl }}/assets/img/posts/multi-charts-single-mean.png" style="height:300px;"/>
</center>
<center><i>Média em x: 54.02. Média em y: 48.09. Desvio padrão: 24.79. R de Pearson: +0.32.</i></center>
<br/>

## 5. Você normalizou os dados?

*Features* que variam muito de valor tem maior impacto em alguns algoritmos de predição. Pra evitar que uma variável tenha uma importância muito maior que outra, é importante [normalizar os dados](https://en.wikipedia.org/wiki/Normalization_(statistics)) (também conhecido como *feature scaling*).


## 6. Você já analisou quais features tem maior relevância?

Se sua variável alvo é numérica, encontrar correlações com variáveis numéricas é bastante simples. Mas existem outras formas: você pode rodar um [PCA](https://en.wikipedia.org/wiki/Principal_component_analysis) pra ver o peso de cada dimensão no seu sistema, ou rodar um [Random Forest](https://en.wikipedia.org/wiki/Random_forest) e encontrar as *features* de maior relevância. Às vezes uma feature que você considera relevante está com peso demais e desbalanceando o sistema. Às vezes o inverso ocorre. Fique atento(a)!

## 7. Suas classes estão desbalanceadas?

Classes desbalanceadas tendem a dar modelos com alta acurácia, mas normalmente uma das classes tem baixa [Precisão](https://github.com/leportella/datascience-pizza/blob/master/dicionario.md#precis%C3%A3o) e/ou [Revocação](https://github.com/leportella/datascience-pizza/blob/master/dicionario.md#revoca%C3%A7%C3%A3o-recall) (*Recall*). Técnicas como [*undersampling*](https://en.wikipedia.org/wiki/Undersampling) ou [*oversampling*](https://en.wikipedia.org/wiki/Oversampling) podem ser usadas para evitar esses tipos de problemas.

## 8. Diminuiu a dimensionalidade?

Suas análises podem ser mais eficientes diminuindo dimensionalidades. Fazer um [PCA](https://en.wikipedia.org/wiki/Principal_component_analysis) com um menor número de dimensões, ajuda a diminuir a quantidade de dados que você está trabalhando. Verifique se essa é uma possibilidade 🙃


## 9. Existe a possibilidade de conseguir mais dados?

Nem sempre o dado que você tem é o suficiente para cumprir os objetivos. É sempre bom analisar se existem dados (públicos ou não) que poderiam ser considerados para ampliar seus horizontes de análise.

## 10. Fez engenharia de variáveis?

Muitas vezes combinar variáveis para gerar uma nova variável, processo conhecido como [engenharia de variáveis](https://en.wikipedia.org/wiki/Feature_engineering), pode tornar o seu modelo infinitamente melhor. Tente trabalhar com as variáveis que você tem, explore possibilidades. [No desafio do Titanic](https://www.kaggle.com/c/titanic), por exemplo, [criar novas variáveis](https://www.kaggle.com/jschnab/titanic-survival-feature-engineering-svm) aumenta consideravelmente as chances de conseguir uma boa acurácia com modelos bem simples.


## 11. Você buscou referências de projetos que fizeram algo semelhante?

É sempre importante olhar para o passado e crescer no ombro de gigantes. Busque trabalhos que tentaram fazer algo parecido, referências de onde você pode tirar ideias. Se falar no contexto de Deep Learning, é muito importante buscar arquiteturas que já deram certo em contextos semelhantes. Lembre-se: nossa área envolve ciência e tem muita ciência descrita em detalhes em artigos acadêmicos.

[ResearchGate](https://www.researchgate.net/) e [Google Scholar](https://scholar.google.com.br/) são dois ótimos portais de busca de artigos 🙃

## 12. Suas tentativas foram registradas?

Depois de muitas tentativas, é fácil se perder num mar de números de acurácia, precisão, revocação, variáveis utilizadas, modelos utilizados… ufa!

[Quando eu estava fazendo meu estudo sobre vítimas de acidentes de trânsito](https://leportella.com/english/2019/01/02/federal-road-accidents-II.html), eu comecei a perder a noção de qual modelo estava melhor. Eram muitos modelos e 3 classes. Pra isso, fiz uma planilha no Google Drive que continha informações de precisão e revocação para cada uma das minhas 3 classes, mais a acurácia e o *F1 score* geral para cada um dos modelos que eu testei. Fiz com que as células aparecessem em escalas de cor de vermelho escuro (0) para verde (1). Isso facilitou muito! Se quiser, [você pode baixar um exemplo dessa planilha](https://docs.google.com/spreadsheets/d/1G0UfnlyHtR_aMCPGND4_KZXvYn5xs86nWLkW0Is_qqU/edit?usp=sharing).

<center>
  <img src="{{ site.url }}{{ site.baseurl }}/assets/img/posts/ml-sheet.png" style="height:300px;"/>
</center>
<center><i>Exemplo da minha planilha no meu estudo</i></center>
<br/>

## 13. Seu projeto está organizado?

Bom, eu tenho pa-vor de projeto desorganizado. Então desde cedo eu tento criar um padrão pro projeto que estou trabalhando (mesmo que eu mude ele depois caso achar mais conveniente).

Mesmo que não seja sua prioridade logo de cara, é muito importante pensar que suas coisas devem estar organizadas. Outras pessoas (incluindo você no futuro) vão precisar entender o que foi feito, e ninguém merece ficar rodando que nem barata tonta em uma pilha de códigos sem sentido. Se você não tem ideia de como fazer isso, você pode seguir o padrão sugerido pelo [Cookiecutter-datascience](https://github.com/drivendata/cookiecutter-data-science) ou mesmo olhar outros projetos que admire e ver como eles organizaram as coisas.

É sempre legal também escrever um relatório ou README sobre qual era o objetivo do projeto, suas premissas e os resultados encontrados. Mesmo que não seja algo extenso, também facilita a vida de quem vem depois!

## 14. Qual seu critério de sucesso?

Lembre-se sempre: ["Todos os modelos são ruins, alguns modelos são úteis"](https://en.wikipedia.org/wiki/All_models_are_wrong). É importante definir critérios de sucesso. Qual acurácia mínima aceita? Quantos modelos você vai testar?

No caso do meu projeto com acidentes de trânsito, meu objetivo era 60% de acurácia com 3 classes. Eu não consegui isso. Remodelei meus objetivos alcancei 78% com 2 classes apenas. Mas eu tinha um objetivo inicial claro, mesmo que meu resultado final tenha sido diferente.

## 15. Você descreveu possibilidade de melhorias?

Descreva oportunidades de melhoria: o que seria possível fazer de diferente? Quais premissas foram assumidas que podem ser revistas no futuro? Quais dados a mais você poderia adicionar para tentar melhorar a análise? Sempre deixe espaço para crescer caso você volte no projeto no futuro :)


<center>
  <img src="https://media.giphy.com/media/dQpUkK59l5Imxsh8jN/giphy.gif" style="height:300px;"/>
</center>
<br/>

