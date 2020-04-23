---
layout: post
title: "Usando MongoDB com Python e MongoEngine <3"
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
permalink: pt-mongoengine.html
redirect_from: /pt-br/2018/08/24/mongo-db-python-e-mongoengine.html
last_modified_at: 2018-08-24T15:41:52-05:00
---

[🇬🇧 *Leia em inglês*]({{base}}/mongoengine.html)

---

No último ano eu comecei a trabalhar com MongoDB por diversão e fiz alguns projetos paralelos com esse banco de dados que 
é bem interessante!
A ideia principal do MongoDB é que ele é extremamente flexível. O [pymongo](https://api.mongodb.com/python/current/) é uma 
excelente biblioteca para trabalhar com o MongoDB, porém em alguns projetos mais complexos às vezes é preciso uma coisa um 
pouco mais robusta. Uma alternativa interessante é a biblioteca [**MongoEngine**](https://github.com/MongoEngine/mongoengine), 
que Mapeamento entre Objeto-Documento (Object-Document Mapper - ODM), que trabalha os documentos do banco de dados como 
uma espécie de ORM.

Como eu estava adorando trabalhar com essas ferramentas, decidi fazer esse post para falar um pouco mais sobre o MongoDB, 
MongoCompass e MongoEngine, minhas principais ferramentas de trabalho. Na sequência, pretendo fazer um post para 
falar de jeitos de testar essas ferramentas :)

Vamos começar?

![](https://media.giphy.com/media/3o6ZsXHLRnkgPtEYVi/giphy.gif)

## Configuração Inicial

Primeira coisa: precisamento configurar o banco de dados. Você pode rodar o MongoDB na sua máquina ou usar o 
[MongoDB Atlas](https://www.mongodb.com/cloud/atlas), onde você tem um banco de dados online e algumas opções gratuitas para 
espaços de até 500mb.

Se quiser o banco rodando localmente, você pode usar o docker:

```
$ docker run -d -p 27017:27017 mongo:3.6.5-jessie
```

Minha ferramenta favorita para conferir os dados e manipulá-los de forma fácil é o 
[Mongo Compass](https://www.mongodb.com/products/compass). A interface é muito simples e intuitiva e ele é bastante rápido 
para uso no dia a dia, mesmo que não esteja conectado em um banco de dados local. Você pode conectá-lo diretamente ao 
seu banco no Atlas ou na sua máquina local e salvar essa conexão como favorita, não precisando digitar as credenciais toda 
vez que quiser acesso.

Pelo Mongo Compass eu criei um banco de dados chamado `imdb` e uma coleção chamada `movie`. Eu peguei um arquivo json com 
informações sobre diversos filmes disponível [nesse link](https://raw.githubusercontent.com/steveren/docs-assets/charts-tutorial/movieDetails.json) usado 
[nesse tutorial](https://docs.mongodb.com/charts/master/tutorial/movie-details/prereqs-and-import-data/). 

Uma vez que criamos o banco de dados e a coleção, podemos adicionar todos os dados armazenados nesse JSON para a nossa 
coleção de uma forma bem simples. Vá na área da sua coleção na interface do Mongo Compass:

![](https://i.imgur.com/HgZHPht.png)

Na barra superior vá em `Collection` > `Import Data`

![](https://i.imgur.com/cSJV9f6.png)

Selecione o arquivo que você baixou do site 

![](https://i.imgur.com/OA8KvZo.png)

Prontinho :) Um monte de filmes para analizarmos as avaliações foram adicionados na nossa coleção.

![](https://media.giphy.com/media/pUeXcg80cO8I8/giphy.gif)

## MongoDB e Pymongo

Primeiro, vamos entender um pouco mais como são os documentos do MongoDB usando o *pymongo*.

Para acessar minha coleção via *pymongo*, eu tenho que conectar com o MongoClient a conexão do MongoDB, o banco de dados
que eu acabei de criar e a coleção inserida dentro do banco de dados. Podemos fazer isso dessa forma:

```python
from pymongo import MongoClient

client = MongoClient() # conecta num cliente do MongoDB rodando na sua máquina
db = client['imdb'] # acessa o banco de dados
collection = db['movie'] # acessa a minha coleção dentro desse banco de dados
```

No entanto, até agora a banco de dados e a coleção não foram efetivamente acessados. Para acessá-los e pegar informações 
sobre um filme particular, podemos usar o método `find_one`:

```python
collection.find_one()
```

O retorno será algo similar à estrutura abaixo:

```python
{'_id': ObjectId('5b107bec1d2952d0da9046e0'),
 'title': 'Once Upon a Time in the West',
 'year': 1968,
 'rated': 'PG-13',
 'runtime': 175,
 'countries': ['Italy', 'USA', 'Spain'],
 'genres': ['Western'],
 'director': 'Sergio Leone',
 'writers': ['Sergio Donati',
  'Sergio Leone',
  'Dario Argento',
  'Bernardo Bertolucci',
  'Sergio Leone'],
 'actors': ['Claudia Cardinale',
  'Henry Fonda',
  'Jason Robards',
  'Charles Bronson'],
 'plot': 'Epic story of a mysterious stranger with a harmonica who joins forces with a notorious desperado to protect a beautiful widow from a ruthless assassin working for the railroad.',
 'poster': 'http://ia.media-imdb.com/images/M/MV5BMTEyODQzNDkzNjVeQTJeQWpwZ15BbWU4MDgyODk1NDEx._V1_SX300.jpg',
 'imdb': {'id': 'tt0064116', 'rating': 8.6, 'votes': 201283},
 'tomato': {'meter': 98,
  'image': 'certified',
  'rating': 9,
  'reviews': 54,
  'fresh': 53,
  'consensus': 'A landmark Sergio Leone spaghetti western masterpiece featuring a classic Morricone score.',
  'userMeter': 95,
  'userRating': 4.3,
  'userReviews': 64006},
 'metacritic': 80,
 'awards': {'wins': 4, 'nominations': 5, 'text': '4 wins & 5 nominations.'},
 'type': 'movie'}
```

Se você checar o tipo desse retorno, você verá que essa estrutura, na verdade, é um simples dicionário. E, como tal, 
você pode acessar qualquer um dos itens e modificá-lo, retornando dicionário alterado para o banco de dados posteriormente.
Muito legal. Então, basicamente, um documento do MongoDB trabalha de forma muito similar ao que conhecemos como dicionários 
no Python. Você pode adicionar *chaves* ao seu documento e essas *chaves* podem ter quaisquer tipo de valor: 
inteiros, listas e até novos dicionários.

A grande vantagem de usar o Mongo Compass, é que os documentos podem ser agregados e expandidos para melhor visualização 
dependendo do contexto. Aqui estão duas visões do mesmo documento acima, mas agregados e expandidos no Mongo Compass:

<img src="https://i.imgur.com/p16d738.png" height="300" style="max-width: 20%" />

<img src="https://i.imgur.com/cPCTlSq.png" height="550" style="max-width: 20%" />


## Preparando o MongoEngine

Agora que entendemos um pouco como um documento do MongoDB funciona no Python, vamos começar a ver como o MongoEngine vai 
funcionar com a nossa coleção. Primeiro, vamos observar como nosso dado é estruturado.

Podemos observar que o documento de um filme contém essas chaves e tipos de valores:

* _id: ObjectId
* title: string
* year: inteiro
* rated: string
* runtime: inteiro
* countries: lista
* genres: lista
* director: string
* writers: lista
* actors: lista
* plot: string
* poster: documento
* imdb: documento
* tomato: documento
* metacritic: inteiro
* awards: documento
* type: string

Agora vamos usar esse conhecimento do nosso dado para criar uma classe baseada no MongoEngine. Vamos ignorar por agora 
os campos que tem valores do tipo documentos e vamos focar nos demais. Vamos criar uma classe chamada `Movie` em um arquivo 
chamado `models.py` e essa classe deve herdar de `mongoengine.Document`. Se você está familiarizado com Django, vai conseguir 
perceber a semelhança.

A ideia é que o nome da classe tenha o mesmo nome da sua coleção. Neste caso, como nossa coleção se chama `movie` nossa classe 
terá o mesmo nome. Agora, vamos criar na classe um atributo para cada uma das chaves que listamos acima. Cada atributo deve 
ter como valor uma instância do MongoEngine. No nosso caso, usaremos `StringField` para atributos de texto,  `IntField` para 
inteiros e `ListField` para  listas. Muitos outros estão disponíveis, mas esses são os que vamos usar no momento. 
Fazendo isso, o MongoEngine vai garantir que você não adicione um inteiro em um campo de texto ou uma lista 
num campo de inteiros. Ele mantém a consistência.

```python
# models.py
import mongoengine


class Movie(mongoengine.Document):

    title = mongoengine.StringField()
    year = mongoengine.IntField()
    rated = mongoengine.StringField()
    runtime = mongoengine.IntField()
    countries = mongoengine.ListField()
    genres = mongoengine.ListField()
    director = mongoengine.StringField()
    writers = mongoengine.ListField()
    actors = mongoengine.ListField()
    plot = mongoengine.StringField()
    poster = mongoengine.StringField()
    metacritic = mongoengine.IntField()
    type = mongoengine.StringField()
```

Vamos tentar usar essa classe que acabamos de criar. Primeiro, precisamos conectar no banco de dados. Com o MongoEngine, 
se estiver usar um banco local, basta passar o nome do banco de dados à função `connect`:

```python
from mongoengine import connect

connect('imdb')
```

Agora estamos conectados. No entanto, quando importamos nossa classe e tentamos usá-la para pegar um documento, recebemos 
um erro...

```python
from models import Movie

Movie.objects.first()
```

Mensagem de erro: `FieldDoesNotExist: The fields "{'awards', 'imdb', 'tomato'}" do not exist on the document "Movie"`:

O que aconteceu? O MongoEngine checou o banco de dados e percebeu que a nossa classe não é igual 
(não tem todos os atributos) do documento armazenado no banco de dados.

Nesse momento nós temos duas opções: podemos mudar a classe para se adaptar ao documento que temos armazenado, ou podemos 
herdar de uma outra classe (`DynamicDocument`) ao invés da classe padrão `Document`. Herdando de `DynamicDocument`, o 
MongoEngine passa a ignorar os campos que existem no banco de dados e não foram mapeados na nossa classe. 

Uma vez que é mais fácil só herdar de uma outra classe e fazer tudo funcionar, vamos pelo outro caminho, ok?

Você pode adicionar Documentos a atributos e esses documentos serão bem similares ao documento `Movie` que acabamos de 
construir.  A diferença é que ao invés de a classe de documento herdar do padrão `mongoengine.Document` ela irá herdar de 
`mongoengine.EmbeddedDocument`, para indicar que este novo documento (classe) 
criado será utilizado dentro de um outro documento.

O nosso arquivo `models.py` passa a se tornar:

```python
import mongoengine


class Imdb(mongoengine.EmbeddedDocument):
    id = mongoengine.StringField()
    rating = mongoengine.DecimalField()
    votes = mongoengine.IntField()


class Tomato(mongoengine.EmbeddedDocument):
    meter = mongoengine.IntField()
    image = mongoengine.StringField()
    rating = mongoengine.IntField()
    reviews = mongoengine.IntField()
    fresh = mongoengine.IntField()
    consensus = mongoengine.StringField()
    userMeter = mongoengine.IntField()
    userRating = mongoengine.DecimalField()
    userReviews = mongoengine.IntField()


class Awards(mongoengine.EmbeddedDocument):
    wins = mongoengine.IntField()
    nominations = mongoengine.IntField()
    text = mongoengine.StringField()


class Movie(mongoengine.Document):
    title = mongoengine.StringField()
    year = mongoengine.IntField()
    rated = mongoengine.StringField()
    runtime = mongoengine.IntField()
    countries = mongoengine.ListField()
    genres = mongoengine.ListField()
    director = mongoengine.StringField()
    writers = mongoengine.ListField()
    actors = mongoengine.ListField()
    plot = mongoengine.StringField()
    poster = mongoengine.StringField()
    imdb = mongoengine.EmbeddedDocumentField(Imdb)
    tomato = mongoengine.EmbeddedDocumentField(Tomato)
    metacritic = mongoengine.IntField()
    awards = mongoengine.EmbeddedDocumentField(Awards)
    type = mongoengine.StringField()
```

E... voilá! Nós temos o nosso novo modelo preparado e pronto para uso!

Podemos acessar nosso banco de dados e buscar um documento utilizando a seguinte expressão:

```python
movie = Movie.objects.first()
```

Você verá que a instância `movie` tem uma série de funções para ajudar a lidar com o documento. Uma função bem legal 
é a que pega a sua instância toda e já transforma ela em um dado no formato JSON:

```python
movie.to_json()
```

## Algumas dicas...

Agora que entedemos como as classes de modelo funcionam, algumas coisas que você deve saber:

* Atributos podem ser obrigadores (`required = True`)
* Atributos podem exigir que eles sejam únicos (`unique = True`)
* MongoEngine lida com o atributo de `_id` automaticamente, e você pode adicionar outro atributo como o identificador único
* Você pode passar escolhas para os campos de texto (`StringField`)
* Você pode usar a classe para criar e salvar novas instâncias: `Movie(title='My new Movie').save()`
* Se você não quer que o nome da sua classe seja o mesmo nome da sua coleção você pode passar o nome da sua coleção como um dos parâmetros de um dicionário em um atributo chamado `meta`. Ficaria dessa forma: `meta = {'collection': 'movie'}` :)

## Buscando dados no dataset

![](https://media.giphy.com/media/3ohhwuzTxy3eQK10Yw/giphy.gif)

MongoEngine nos permite fazer buscas de uma forma bem semelhante ao que o Django permite em seu ORM padrão. 
Por exemplo, nós podemos fazer filtragens por filmes que foram lançados antes de 1988:

```python
Movie.objects(year__lte=1988)
```

Nós podemos fazer filtros de filmes que a avaliação do IMDb é acima de 9:

```python
Movie.objects(imdb__rating__gte=9)
```

Buscar se os títulos possuem uma certa palavra...

```python
Movie.objects(title__contains='Love')
```

Não apenas isso, podemos contar rapidamente o número de filmes que o nosso banco de dados têm:

```python
Movie.objects.count()
```

Ou filmes que tem 2 atores participando:

```python
Movie.objects(actors__size=2)
```

Enfim, as possibilidades são infitinas! Veja a  
[documentação oficial](http://docs.mongoengine.org/tutorial.html) para ter mais ideias sobre o que é possível fazer :)
