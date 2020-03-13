---
layout: post
title: "Tutorial básico de SQLAlchemy"
categories:
  - pt-br
tags:
  - pt-br
  - python
  - machine learning
  - database
  - postgres
  - sql databases
  - nosql databases
  - Python tutorial
  - sqlalchemy
  - orm
  - database
  - SQL
  - Project Jupyter
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
featured-img: bookcase 
last_modified_at: 2019-01-10T21:55:52-05:00
---


[Eu trabalhei com o Projeto Jupyter de dezembro/2018 até março/2019](https://leportella.com/english/2018/12/12/outreachy-I.html) como parte de um estágio no programa [Outreachy](https://www.outreachy.org/). Foi uma experiência maravilhosa e super recomendo! Durante o meu estágio, eu lutei com a biblioteca [SQLAlchemy](https://www.sqlalchemy.org/) que o [JupyterHub](https://github.com/jupyterhub/jupyterhub) utiliza internamente. Como estudei essa biblioteca e tive que fazer algumas buscas no [Stack Overflow](http://stackoverflow.com/) pra entender várias coisas, decidi criar este post para ajudar a digerir algumas das minhas dúvidas e descobertas.

Todo o código está disponível [neste repositório](https://github.com/leportella/sqlalchemy-basics-post/).


## Sumário

* [Criando e entendendo o *Engine* (mecanismo)](#engine)
* [*Engine* ou conexão?](#engine-connection)
* [Criando e entendendo Sessões](#sessions)
* [Criando tabelas](#creating-tables)
* [Adicionando novos usuários](#add-records)
* [Fazendo buscas](#queries)
* [Adicionando tabelas depois de iniciar o banco com create_all](#creating-tables-posterior)
* [Criando uma relação com chave estrangeira (foreign key)](#foreign-key)


<h2 id='engine'>Criando e entendendo o <i>Engine</i> (mecanismo)</h2>

Para começar a trabalhar com o [SQLAlchemy](https://www.sqlalchemy.org/), a primeira coisa que eles ensinam nos tutoriais é que você deve criar um *engine*. O *engine* é como o SQLAlchemy se comunica com o banco de dados. Portanto, ao criar o mecanismo, você deve adicionar a URL do banco de dados (chamada pela abreviação em inglês *db*) e é basicamente isso.

```python
from sqlalchemy import create_engine

engine = create_engine('sqlite:///:memory:', echo=True)
```

Embora você possa acessar o banco de dados por meio de comandos do *engine* (veremos como), geralmente esse não é o recomendado. Você pode, mas não deve 🙂 O `engine` é para ser apenas a ponte de conexão entre o Python e o banco.

Nesse comando, você está apenas dizendo para o SQLAlchemy onde seu banco de dados está localizado. O atributo `echo = True` fará com que SQLAlchemy registre no console todos os comandos SQL que você está executando através dos comandos e os resultados otidos. Esse parâmetro não deve ficar ativado em produção, ok?

<center><img src="https://i.imgur.com/0gVcCUg.png" style="height:200px;"/></center>

Uma vez que seu *engine* conhece seu banco de dados, é fácil executar comandos usando um método chamado `engine.execute(...)`. Veja o exemplo abaixo:

![](https://raw.githubusercontent.com/leportella/sqlalchemy-basics-post/master/gifs/engine_execute.gif)

Portanto, você tem uma via de mão dupla: o *engine* que sabe onde está o seu banco de dados e um método (`engine.execute(...)`) para alterar o banco de dados usando o *engine*:

<center><img src="https://i.imgur.com/yjdhaTZ.png" style="height:200px;"/></center>

<h2 id='engine-connection'><i>Engine</i> ou conexão?</h2>

Também vi em alguns tutoriais que você tem outra maneira de executar comandos SQL através do *engine* que é através de uma `connection` (conexão). Isso acontece da seguinte forma:


```
conn = engine.connect()
conn.execute(...)
```

Isso nos permite criar comandos transientes, o que significa que todos os comandos devem ser executados com êxito no banco de dados ou todos devem ser revertidos em caso de erro [[1](https://docs.microsoft.com/en-us/sql/t-sql/language-elements/transactions-transact-sql?view=sql-server-2017)]:

```python
trans = conn.begin()
conn.execute('INSERT INTO "EX1" (name) '
             'VALUES ("Hello")')
trans.commit()
```

Então, na verdade, a estrutura de comunicação se parece mais com isso:


<center><img src="https://i.imgur.com/Bcp1Zku.png" style="height:200px;"/></center>


No entanto, quando eu continuei investigando as diferenças entre `engine.execute(...)` e `connection.execute(...)` [eu descobri que elas não são diferentes](https://stackoverflow.com/a/34364247/3538098):


> _"Usar engine.execute() e connection.execute() é (quase) a mesma coisa. No primeiro, o objeto connection é criado implicitamente e, no segundo, nós o instanciamos explicitamente."_ 

Então, fique à vontade para usar qualquer uma delas, se quiser :)


<h2 id='sessions'>Criando e entendendo Sessões</h2>

Até agora, nos conectamos no banco de dados e puder executar comandos através de instruções SQL. No entanto, o que torna o SQLAlchemy tão atraente é o [ORM](https://en.wikipedia.org/wiki/Object-relational_mapping) (*Object Relational Mapping*), que eu não comentei até agora. 

O ORM precisa de uma `session` (sessão) para fazer um meio de campo entre os objetos que criamos no Python e o `engine` que realmente se comunica com o banco de dados. Vamos criar usar uma função chamada `sessionmaker` pra passar o `engine` pra nossa sessão atual e criarmos de fato a sessão:

```python
from sqlalchemy.orm import sessionmaker

Session = sessionmaker(bind=engine)
session = Session()
```

Então, daqui pra frente, nós vamos usar o `session` para conversar com as tabelas e fazer consultas, mas é o `engine` que realmente está implementando coisas no seu banco de dados.

<center><img src="https://i.imgur.com/iqV59ky.png" style="height:300px;"/></center>

Embora pareça confuso ter três entidades antes mesmo de começar a mexer com tabelas, na maioria das vezes após a configuração inicial você vai usar a `session` muito mais do que o `engine` e a conexão será feita implicitamente por ele.

<h2 id='creating-tables'>Criando tabelas</h2>

Agora que entendemos a estrutura básica, a primeira coisa a fazer é começar a criar tabelas em nosso banco de dados e finalmente começar a dar uma olhada no ORM do SQLAlchemy. 

Para criar novas tabelas, precisamos criar classes que contêm atributos. Cada classe será uma tabela em nosso banco de dados e cada atributo será uma coluna na tabela. Para mapear qual tabela no banco de dados será relacionada com cada classe em nossos arquivos, usaremos um sistema SQLAlchemy chamado *Declarative* (Declarativo). Para usar isso, a primeira coisa que devemos fazer é instanciar uma `Base`:


```python
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()
```

Agora vamos criar uma classe `User` que herda da `Base`  declarativa que acabamos de criar. Criaremos apenas três atributos para nossa classe: `id` (que é uma chave primária), um nome e uma senha. Como estamos usando Declarativos, devemos adicionar pelo menos dois atributos à nossa classe: 

1)  `__tablename__` indica como sua tabela será realmente chamada dentro do banco de dados 

2) Ao menos um dos atributos deve ser declarado como uma chave primária [[2](https://docs.sqlalchemy.org/en/latest/orm/tutorial.html#declare-a-mapping)].

Também é bom adicionar um método opcional chamado `__repr__` que será um texto (*string*) que deve ser retornado quando tivermos a instância da classe `User`.


```python
from sqlalchemy import Column, Integer, String

class User(Base):
    __tablename__ = 'users'

    id = Column(Integer, primary_key=True)
    name = Column(String)
    password = Column(String)

    def __repr__(self):
        return f'User {self.name}'
```

Agora temos uma classe que indica como nossa tabela deve ser no nosso banco de dados. No entanto, nada mudou até agora. O banco de dados ainda não conhece essa estrutura. Para realmente criar as tabelas em nosso banco de dados seguindo a estrutura que definimos na classe `User`, precisamos usar a `Base` declarativa que acabamos de criar e precisamos do `engine`: 


```python
Base.metadata.create_all(engine)
```

É só nesse momento que o SQLAlchemy realmente vai implementar as mudanças no banco de dados. Como definimos o parâmetro  `echo` como verdadeiro (`True`), podemos ver exatamente quais são instruções aplicadas via SQL que o `engine` está gerando:

<center><img src="https://i.imgur.com/kU4Snpb.png" style="height:400px;"/></center>


<h2 id='add-records'>Adicionando novos usuários</h2>


Agora que a tabela de fato existe no banco de dados, podemos usar a classe para criar um novo registro no banco. Podemos usar a classe `User` para criar um novo usuário e `session.add(...)` para adicionar a instância ao nosso banco de dados como uma nova linha.

```python
user = User(name='John Snow', password='johnspassword')
session.add(user)

print(user.id)  # None
```

Anteriormente eu comentei que sempre precisamos de uma chave primária, mas no exemplo acima eu não passei uma para o modelo. Se eu tentar imprimir o `id` do usuário que acabei de criar, ele vai retornar `None`.

Isso ocorre porque o `session.add` apenas registra as transações que queremos que sejam feitas, mas na verdade não faz nenhuma mudança no banco [[3](https://stackoverflow.com/a/4202016/3538098)].

Conforme explicado [neste link](https://stackoverflow.com/a/4202016/3538098), temos duas operações que podem ser realizadas aqui:


> _session.flush() comunica uma série de operação ao banco de dados (inserir, atualizar, apagar). O banco de dados às mantém como operações pendentes em uma trasação. As mudanças não são persistidas no dsco ou visíveis em outras transações até o banco dados receber um COMMIT para a transação atual (que é o que o session.commit() faz)._ 

ou


>  _session.commit() persiste as mudanças no banco de dados. Esse comando sempre chama `session.flush()` como parte dele._ 


<h2 id='queries'>Fazendo buscas</h2>

Depois de termos registros no banco de dados, precisamos ter acesso a eles :)

Para isso podemos usar o método  `query` presente na nossa  `session`. O método recebe como parâmetro a classe que representa a tabela do banco em que queremos fazer a busca pelo nosso registro. Depois usamos o método `filter_by` para buscar uma característica em uma das colunas (ou atributos da classe) 

```python
query = session.query(User).filter_by(name='John')
```

Por fim, passamos um método para indicar o que queremos fazer com esta consulta: contar o número de registros encontrados (`.count()`), retornar todos os registros encontrados (`.all()`), retornar apenas o primeiro registro (`.first()`) e assim por diante:

```python
query.count()
```

Outra maneira de fazer essa busca é usar o método `filter`, em vez do `filter_by`, que possui uma sintaxe ligeiramente diferente:

```python
session.query(User).filter(User.name=='John').first()
```

Com o método `filter`, você também pode procurar não por *strings* exatas mas por partes de *strings*:

```python
session.query(User).filter(User.name.like('%John%')).first()
```

No [Jupyterhub](https://github.com/jupyterhub/jupyterhub), foi adicionado a cada modelo um método de classe que simplifica essa sintaxe bastante complicada. Nesse caso criamos um método, que é um *classmethod*, que apenas precisa receber a `session`  e consegue fazer a busca de maneira mais simples. O método fica escrito dessa forma:

```python
class User(Base):
  ...

  @classmethod
  def find_by_name(cls, session, name):
    return session.query(cls).filter_by(name=name).all()
```

E a busca fica mais simples para encontrar usuários com o nome John:

```python
Product.find_by_name(session, 'John')
```


<h2 id='creating-tables-posterior'>Adicionando tabelas depois de iniciar o banco com create_all</h2>

Um dos problemas que tive enquanto trabalhava com o [Projeto Jupyter](https://jupyter.org/), é que eu precisava  criar uma nova tabela em um banco de dados e um `engine` que já estavam criados, ou seja, depois do  `Base.metadata.create_all(engine)`.

Então, imagine que agora eu quero uma tabela com Produtos (*Product*) como a seguinte:



```python
from sqlalchemy import Column, Integer, String

class Product(Base):
    __tablename__ = 'products'

    id = Column(Integer, primary_key=True)
    name = Column(String)
```

A maneira mais simples que eu encontrei de criar essa nova tabela no banco de dados foi:

```python
Product.__table__.create(engine)
```


<h2 id='foreign-key'>Criando uma relação com chave estrangeira (foreign key)</h2>

Imagine que você gostaria de conectar cada produto (*product*) a um usuário (*user*) em seu sistema. Portanto, em cada instância da classe `Product`, você gostaria de armazenar uma instância da classe  `User`:


<center><img src="https://i.imgur.com/BJqWSMj.png" style="height:350px;"/></center>

Se você estiver criando todas as tabelas agora, uma classe `Column` como atributo da sua classe `Product` e indique esse atributo faz referência à chave estrangeira da classe `User`  e que vai armazenar o atributo `id` como chave estrangeira:


```python
from sqlalchemy import Column, ForeignKey, Integer, String
from sqlalchemy.orm import relationship

class Product(Base):
    __tablename__ = 'product'

    id = Column(Integer, primary_key=True)
    name = Column(String)
    user_id = Column(Integer, ForeignKey('user.id'))
    user = relationship('User')
```

Agora é preciso ir na classe `User` e adicionar essa relação com `Produto` para que seja possível acessar produtos que estão atrelados a um usuário:

```python
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import relationship

class User(Base):
    __tablename__ = 'user'  # if you use base it is obligatory

    id = Column(Integer, primary_key=True)  # obligatory
    name = Column(String)
    password = Column(String)
    products = relationship(Product, backref="users")
```

É possível criar as tabelas usando o `Base.metadata.create_all(engine)` que vimos anteriormente. E agora, você pode criar um usuário e um produto relacionados entre si da seguinte forma:

```python
user = User(name='John')
product = Product(name='wolf', user=user)

session.add_all([user, product])
session.commit()
```

E é isso 🙂 

<center><img src="https://media.giphy.com/media/3o7btQsLqXMJAPu6Na/giphy.gif"/></center>
