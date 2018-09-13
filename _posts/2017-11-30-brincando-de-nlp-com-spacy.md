---
layout: post
title: "Brincando de Processamento Natural de Linguagem com spaCy"
categories:
  - pt-br
tags:
  - pt-br
  - python
  - data science
  - nlp
  - lpn
featured-img: words
last_modified_at: 2017-03-09T14:25:52-05:00
---
Essa semana eu descobri o [spaCy](https://spacy.io), uma bilbioteca Python para Processamento de Linguagem Natural (PLN) que me pareceu excelente. 
Ao brincar um pouco mais com ela, eu percebi que ela era ainda mais divertida do que eu imaginava e já com um modelo pronto em 
português, o que facilita bastante para dar uma arranhada na superfície do assunto que é o PLN (ou NLP em inglês).

Eu, particularmente, não sei nada muito a fundo sobre PLN, então a minha apresentação aqui vai ser bem superficial :)

Vamos à uma rápida apresentação do que eu vi até agora...

## A problemática

Processamento inteligente de texto puro é algo muito difícil: muitas palavras são raras, palavras completamente 
distintas podem ter significados iguais enquanto a mesma palavra pode ter um significado completamente diferente dependendo 
do contexto. 

![](https://media.giphy.com/media/woOg6RMyfXxSg/giphy.gif)

*Tente explicar para um estrangeiro a diferença entre 'bota a calça' e 'calça a bota'* 

Mesmo a divisão entre palavras pode ser muito difícil em algumas línguas. Mesmo que seja possível utilizar apenas caracteres 
puros, normalmente é melhor usar o conhecimento da linguística para adicionar informações úteis. 

Esse  o objetivo do spaCy: pegar textos puros e retornar um objeto Doc, que contém diversas informações. 

## A biblioteca

spaCy é uma biblioteca em Python relativamente nova para  PLN em escala industrial.  Ela foi desenvolvida pela [Explosion AI](https://explosion.ai/) e é mantida pelo [Matthew Honnibal](https://github.com/honnibal) e [Ines Montani](https://github.com/ines).

O desenvolvimento da spaCy foi feito especificamente para uso em produção e ajudar a criar aplicações que conseguem processar e 
"entender" um grande volume de texto. Ela pode ser usada para extrair informações ou entendimento de linguagem natural ou pré-processar texto para *deep learning*.

Em 2015, uma pesquisa da Emory University e Yahoo! Labs mostrou que o spaCy oferecia o parser sintático mais rápido do mundo e que sua acurácia estava entre as melhores disponveis ([Choi et al, 2015](https://aclweb.org/anthology/P/P15/P15-1038.pdf)). 
A segunda versão foi lançada agora em 2017 e conta com diversas melhorias

## Instalação

O spaCy pode ser facilmente instalado com o pip

```
$ pip install spacy
```

O problema é que muitos exemplos utilizam uma espécie de biblioteca pré-pronta que não vem disponível por padrão. 
Me bati um pouco até entender que esses modelos devem ser baixados de forma separada. (O melhor? Tem em pt!)

```
$ python -m spacy download en
$ python -m spacy download pt
```

## Usando um modelo pré-pronto

Podemos usar direto o modelo de português que acabamos de baixar. 
Para isso, separamos o modelo e chamamos ele genericamente de `nlp`. Como estamos carregando um modelo já pronto, 
essa declaração tende a demorar um pouquinho.

```pythoh
>>> nlp = spacy.load('pt')
```

A partir de agora podemos usar esse modelo para entender frases em português.
Vamos usar o `nlp` para estudar uma frase simples:

```python
>>> doc = nlp(u'Você encontrou o livro que eu te falei, Carla?')
```

Obs: você deve declarar a *string* como unicode para que ele funcione corretamente.

## Um pouco sobre docs e tokens...

Um `Doc`, objeto como aquele que acabamos de criar, é uma sequência de objetos do tipo `Token` e possui diversas 
informações sobre o texto que ele contém. Por dividir a frase em `tokens`, esse documento é uma 
estrutura iterável e portanto, deve ser acessada como tal. Já um `Token` é uma parte da estrutura e pode ser uma frase, palavra, 
uma pontuação, um espaço em branco, etc. No nosso caso, como iremos avaliar uma frase, os `tokens` serão constituídos de palavras e pontuações.

Primeiro, vamos analisar a frase da maneira mais simples: dividindo-a com o método `split` de qualquer *string*.

```python
>>> doc.text.split()
['Você', 'encontrou', 'o', 'livro', 'que', 'eu', 'te', 'falei,', 'Carla?'] 
```

Podemos ver que apesar de ser coerente a divisão por espaços, o verbo `falei` e a vírgula estão dentro de um 
mesmo `token`, assim como `Carla` e a interrogação. 
O `nlp` consegue entender a diferença entre eles e, portanto, quando usamos os `tokens` dentro da 
estrutura do documento, temos uma divisão mais coerente:

```python
>>> [token for token in doc]
[Você, encontrou, o, livro, que, eu, te, falei, ,, Carla, ?]
```

Repare que agora a estrutura considerou a pontução e as palavras como estruturas separadas. Também não temos mais uma lista de *strings*, 
mas uma lista de `Tokens`.

Se não quisermos os objetos `Tokens`, mas sim as *strings* que cada `Token` contém podemos usar o método `.orth_`:

```python
>>> [token.orth_ for token in doc]
['Você',
 'encontrou',
 'o',
 'livro',
 'que',
 'eu',
 'te',
 'falei',
 ',',
 'Carla',
 '?']
```

## Entendendo diferença entre palavras e pontuações

Como o spaCy entende que existe uma diferença entre uma palavra e uma pontuação, também podemos fazer filtragens.
E se eu quisesse apenas as palavras da frase?

```python
>>> [token.orth_ for token in doc if not token.is_punct]
['Você', 'encontrou', 'o', 'livro', 'que', 'eu', 'te', 'falei', 'Carla']
```

## Similaridade

O spaCy também permite avaliar similaridade entre palavras. O método `.similarity` de um `Token` avalia a similaridade 
semântica estimada entre as palavras. Quanto maior o valor, mais similar são as palavras.

Vamos avaliar a similaridade entre 3 palavras: você, livro e eu.

Primeiro vamos armazenar o `tokens` em uma lista para acessá-los de forma independente:

```python
>>> tokens = [token for token in doc]
```

Dessa forma, temos que `tokens[0]` representa o meu `token` da palavra `Você` e `tokens[5]` representa a palavra `eu`.
A análise de similaridade pode ser dada por:

```python
>>> tokens[0].similarity(tokens[5])
0.29921812
```

Legal, temos um valor de 0,29. E o que isso significa? Sabemos intuitivamente que `eu` e `você` devem ser muito mais 
similares que `você` e `livro`, por exemplo. Vamos investigar:

```python
>>> tokens[0].similarity(tokens[3])
-0.067587696
```
O valor é negativo, ou seja, `você` de fato está semânticamente muito mais próximo de `eu` do que de `livro`, o que faz 
todo sentido, certo?


## Análise de classes gramaticais

Podemos também entender as classes gramaticais de cada palavra dentro do nosso contexto:

```python
>>> [(token.orth_, token.pos_) for token in doc]
[('Você', 'PRON'),
 ('encontrou', 'VERB'),
 ('o', 'DET'),
 ('livro', 'NOUN'),
 ('que', 'PRON'),
 ('eu', 'PRON'),
 ('te', 'PRON'),
 ('falei', 'VERB'),
 (',', 'PUNCT'),
 ('Carla', 'PROPN'),
 ('?', 'PUNCT')]
]
```

Então assim conseguimos ver que `encontrou` e `falei` são os verbos da frase. E a vírgula e o ponto de interrogação 
foi corretamente definido como pontuação (`PUNCT`).

## Encontrei, encontraram, encontrarão, encontrariam....

Agora, imagine que você tem um texto enorme e diversos tempos verbais diferentes. A análise passa a ser infinitamente 
mais complicada! O que podemos fazer, então, é analisar não o verbo no tempo verbal que ele foi escrito, mas ele em sua 
raiz.  O nome desse método de encontrar a raiz das palavras é lematização por isso, o método `.lemma_` faz exatamente isso. Vamos olhar:

```python
>>> [token.lemma_ for token in doc if token.pos_ == 'VERB']
['encontrar', 'falar']
```

E isso vale para diversos tempos verbais MESMO!

```python
>>> doc = nlp(u'encontrei, encontraram, encontrarão, encontrariam')
>>> [token.lemma_ for token in doc if token.pos_ == 'VERB']
['encontrar', 'encontrar', 'encontrar', 'encontrar']
```

## Ancestrais

Do mesmo jeito que podemos encontrar as raízes de uma palavra, podemos checar se uma palavra é raíz de outra:

```python
>>> doc = nlp(u'encontrar encontrei)
>>> tokens = [token for token in doc]
>>> tokens[0].is_ancestor(tokens[1])
True
```


## E por fim... entidades

Por fim, podemos avaliar as entidades presentes em uma frase. Por exemplo, peguemos a frase:

```python
doc = nlp(u'Machado de Assis um dos melhores escritores do Brasil, foi o primeiro presidente da Academia Brasileira de Letras')
```

Se formos analisar as entidades presentes nessa frase, percebemos que temos 3 entidades identificadas automaticamente:

```python
>>> doc.ents
(Machado de Assis, Brasil, Academia Brasileira de Letras)
```

Ao analisarmos detalhadamente, podemos ver que o Spacy identificou `Machado de Assis` como uma pessoa 
(`PER` de `person`, em inglês), `Brasil` como um local (`LOC`) e `Academia Brasileira de Letras` como uma organização (`ORG`).


```python
>>> [(entity, entity.label_) for entity in doc.ents]
[(Machado de Assis, 'PER'),
 (Brasil, 'LOC'),
 (Academia Brasileira de Letras, 'ORG')]
```

Claro que em inglês o modelo está bem avançado, e consegue identificar entidades bem mais complexas do que eu pude verificar 
nos textos em português. Por exemplo:

```python
>>>  wiki_obama = """Barack Obama is an American politician who served as
     the 44th President of the United States from 2009 to 2017. He is the first
     African American to have served as president,
     as well as the first born outside the contiguous United States."""
>>> nlp = spacy.load('en')
>>> nlp_obama = nlp(wiki_obama)
>>> [(i, i.label_) for i in nlp_obama.ents]
[(Barack Obama, 'PERSON'),
 (American, 'NORP'),
 (, 'GPE'),
 (44th, 'ORDINAL'),
 (the United States, 'GPE'),
 (2009 to 2017, 'DATE'),
 (first, 'ORDINAL'),
 (African American, 'NORP'),
 (, 'GPE'),
 (first, 'ORDINAL'),
 (United States, 'GPE')]
```

É isso! Espero que tenham se divertido tanto quanto eu :)
