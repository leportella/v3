---
layout: post
title: "Como usamos Relações Genéricas do Django para adicionar comentários em instâncias de diferentes modelos"
categories:
  - pt-br
tags:
  - pt-br
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
  - CCBV
  - relações genericas
  - generic relation
  - database
  - ORM
  - foreing key
featured-img: comments
permalink: relacoes-genericas-django.html
redirect_from: /pt-br/2016/12/29/relacoes-genericas-do-django.html
last_modified_at: 2017-03-09T14:25:52-05:00
---


[🇬🇧 *Leia em inglês*]({{base}}/generic-relations-django.html)

---

Conversando com algumas pessoas, percebi que poucas conheciam sobre as Relações Genéricas (*Generic Relation*) e Chave Estrangeira Genérica (*Generic Foreign Key*) no Django. Estudando para aplicár esses métodos no nossos sitema foi possível perceber que a documentação pode ser difícil e esparsa — para não falar confusa. Entretanto, relações genéricas nos ajudou muito, então decidi escrever sobre aqui neste blog post :)

Primeiro vamos falar sobre chaves estrangeiras. Quando temos uma chave estrangeira no nosso modelo, estamos conectando uma instância de outro modelo no nosso modelo atual. Certo? Dessa forma, podemos acessar aquela outra instância naquele outro modelo de forma muito fácil. Funciona mais ou menos assim:

```python
class Autor(models.Model):
    nome = models.CharField(max_length=50)

class Livro(models.Model):
    autor = models.ForeignKey(Autor, related_name='livros')
    titulo = models.CharField(max_length=250)
    paginas = models.IntegerField()
```

Aqui teremos uma instância de Autor associada com toda a informação que temos em uma instância de Livro. Ok, isso é legal porque agora podemos associar e armazenar diversos livros, 
todos conectados com um mesmo autor. E podemos acessar todos os livros de um autor de uma forma muito simples:

```python
>>> author.livros.all()
```

Imagine agora que você precisa da mesma coisa: associar uma instância de um modelo com mais informações. Mas ao invés de ter apenas uma instância, de Autor por exemplo, você quer associar um mesmo tipo de infomação através de diversas instâncias de diversos modelos em diversos apps (como livros e cds).

Aqui temos Relações Genéricas (*Generic Relations*) para nos ajudar! De forma resumida podemos dizer que Relações Genéricas é uma chave estrangeira que pode armazenar uma instância de qualquer modelo em qualquer dos seus apps.

Você deve estar se perguntando: porque isto é útil? Você poderia simplesmente adicionar mais campos ao seu modelo original ou algo nesta linha. Sim, isso é verdade na maioria dos casos. Entretanto, em casos específicos, Relações Genéricas podem ser bastante úteis. No nosso caso, precisávamos adicionar comentários em instâncias de modelos presentes em apps completamente diferentes do sistema. Essa funcionalidade era necessárias em lugares diferentes do sistema, de forma que fosse fácil de manter e sem duplicação se código: hora perfeita pras Relações Genéricas.

Vamos a um exemplo mais simples, para facilitar. Começamos criando um modelo de Comentário (Django 1.10):

```python
from django.contrib.contenttypes.fields import GenericForeignKey
from django.contrib.contenttypes.models import ContentType
from django.db import models

class Comentario(models.Model):
    content_type = models.ForeignKey(ContentType)
    object_id = models.Charfield(max_length=50)
    content_object = GenericForeignKey('content_type', 'object_id')
    texto = models.TextField(blank=True)
```

Neste modelo, o campo **texto** é um campo normal de *TextField* do Django, usado para guardar o próprio comentário. Os outros campos, **content_type**, **object_id** e **content_objects** 
são partes necessárias para as Relações Genéricas que iremos usar aqui.

Instâncias de *ContentType* representam e armazenam informações sobre os modelos instalados no seu projeto. 
Toda vez que um novo modelo é criado, novas instâncias de *ContentType* são criadas automaticamente. Aqui, o **content_type** será a chave para o modelo específico que você quer associar.

Já o campo **object_id** é simplesmente um campo *Charfield* normal que irá armazenar o id do objeto armazenado dentro do seu modelo. 
A documentação oficial do Django sugere utilizar o campo do tipo *PositiveIntegerField* aqui. No entanto, nós utilizamos uuid como campo de identificação única e, portanto, acabamos precisando usar o 
*Charfield* (texto) ao invés de usar um campo de inteiros.

Ok, já temos armazenadas as informações sobre o modelo e temos o id do objeto que queremos acessar dentro do modelo... o **content_object** irá representar a instância daquele objeto naquelo modelo. 
O campo *GenericForeignKey* faz a mágica para você!

Vamos aplicar agora em algo útill. Imagine que você tem modelos para Livros e CDs e você que adicionar comentários dos seus usuários em cada livro ou cd armazenado no seu banco de dados.

Para criar um comentário em um livro específico, você precisa fazer isso:

```python
from django.contrib.contenttypes.models import ContentType
from .models import Book, Comment
 
livro = Livro.objects.first()
texto = 'Aqui temos a mensagem do comentario'

novo_comentario = Comment(texto=texto,
                          content_object=livro)
novo_comentario.save()
```

Ou isto:

```python
from django.contrib.contenttypes.models import ContentType
from .models import Book, Comment
 
livro = Livro.objects.first()
texto = 'Aqui temos a mensagem do comentario'
content_type = ContentType.objects.get(app_label='MinhaEstante',     
                                       model='Livro')
  
novo_comentario = Comment(texto=texto,
                           content_type=content_type,
                           object_id=livro.id)
novo_comentario.save()
```

Então primeiro você pode recuperar a instância que você quer associar o seu comentário (**livro** neste caso) e enviá-la ao modelo como um **content_object** (e o Django faz a mágica para você). Ou você pode coletar as informações do app e do modelo pelo método do *ContentType* e enviar o comentário junto com a identificação do livro.

Você então pode adicionar comentários no modelo de Livros, de CDs ou basicament em qualquer modelo em qualquer app do seu sistema. Você não precisa reescrever modelos de comentários em todas as apps do seu sistema!

Agora você pode perguntar: Como eu recupero informações sobre comentários na minha instância de Livro? Ai vem a parte mais simples!


```python
from django.contrib.contenttypes.fields import GenericRelation
from ..models import Comment

class Livro(models.Model):
    autor = models.ForeignKey(Autor)
    titulo = models.CharField(max_length=250)
    paginas = models.IntegerField()
    comentarios = GenericRelation(Comentario)

class Cd(models.Model):
    artista = models.ForeignKey(Artista)
    titulo = models.CharField(max_length=250)
    comentarios = GenericRelation(Comentario)
```

Simples assim! Adicionando um campo de GenericRelation em qualquer modelo dentro do seu sistema, você terá acoplado todo o modelo de Comentarios em uma Relação Genérica. Agora você pode acessar qualquer comentário nas instâncias de Livro ou Cd simplesmente fazendo:


```python
livro.comentarios.all()
```

![](https://cdn-images-1.medium.com/max/800/1*mPUc2fU1VPbW6gjbw1DjeQ.gif)

Outra boa notícia é que pode-se usar o *prefetch_related* para otimizar as buscas no banco de dados sem problema nenhum.

Espero que tenham gostado e que seja útil para vocês também.
