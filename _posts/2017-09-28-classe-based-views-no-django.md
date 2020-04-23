---
layout: post
title: "Class Based Views no Django"
categories:
  - pt-br
tags:
  - pt-br
  - python 
  - intermediario 
  - django
  - class based views
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
  - URLS
  - Class based views
  - ORM
  - Views
featured-img: ccbv
permalink: class-based-views-no-django.html
redirect_from: /pt-br/2017/09/28/classe-based-views-no-django.html
last_modified_at: 2017-03-09T14:25:52-05:00
---

[🇬🇧 *Leia em inglês*]({{base}}/class-based-views-on-django.html)

---


Este post também poderia ser chamado de **o que vem depois dos tutoriais** :)

Em vários tutoriais do Django, vemos como receber *requests* e responder *responses* com páginas html contendo diversas informações. 
Isso é bem legal para começar a entender o processo que o Django faz: recebe *requests* e devolve *templates*. Mas ok, e depois disso?

Quando comecei a fazer um sistema um pouco mais complexo, me deparei com diversas funções de *get* e *post* que tinham que fazer um monte de verificações 
e acabaram sendo muito complexas e pouco efetivas para cobrir o todas as possibilidades que eu vislumbrei.

É aí que entram na história as [Class Based Views](https://docs.djangoproject.com/en/1.11/topics/class-based-views/).

De maneira simples, podemos agregar as funções básicas das views dentro de classes como métodos. Mas o grande poder das Class Based Views está em algumas classes que já estão “pré-prontas” e que a sua classe pode herdar. A partir daí as alterações que precisam ser feitas são mínimas!

Vou seguir passo a passo a evolução de um sistema passando por funções, os problemas que podemos esbarrar e um sistema baseado em classes já existentes.

![](https://media.giphy.com/media/IYjiXRV622OBO/giphy.gif)
TLDR → Veja o [código do projeto todo aqui](https://github.com/leportella/class-based-views-example) e siga em paz :)

## Você está construindo seu sistema e precisa de uma função para retornar os templates

Vou usar o exemplo de uma pequena livraria. 
Você deseja retornar uma página de boas vindas. Simples, você recebe um *request* e retorna um template html. 
Cadastra essa função no **urls.py** com alguma url. A função e o cadastro da url ficam mais ou menos assim:

```python
# views.py
from django. shortcuts import render

def welcome(request):
  return render(request, 'welcome.html')

# urls.py

urlpatterns = [
  url(r'welcome/', views.welcome) 
]
```

## As funções funcionam bem, mas quero algo que venha do banco de dados…

Até aí tudo certo. Aí você decide criar uma função para listar os livros que existem no seu banco.

```python
# views.py
from django. shortcuts import render

from .models import Book

def get_books_list(request):
  books = Book.objects.all()
  return render(request, 'my-books.html', {'books': books})

# urls.py

urlpatterns = [
  url(r'my-books/', views.get_books_list) 
]
```

## Quero uma url para ver detalhes de um livro em específico

Agora, queremos que seja possível visualizar os detalhes de um livro apenas. E agora? As coisas começam a se tornar um pouco mais complexas, certo? Eu tenho que fazer uma busca no banco de dados por um livro específico e preciso tratar eventuais erros, caso o livro não exista ou alguma outra exceção. Aumentamos um pouco o grau de complexidade, mas ainda temos um código bem direto:

```python
# views.py
from django.http import Http404
from django. shortcuts import render

from .models import Book

def get_book_by_id(request, pk):
  try:
    books = Book.objects.get(id=pk)
  except Book.DoesNotExist:
    raise Http404("Book does not exist")
  return render(request, 'my-book-detail.html', {'book': book}

# urls.py

urlpatterns = [
  url(r'book-detail-by-id/(?P<pk>[-\w]+)', views.get_book_by_id) 
]
```

## Ainda está um pouco simples. Que tal jogar um update?

A partir daí já temos um sisteminha de visualização básico. Legal! Próximo passo? Vamos permitir que nossos usuários possam editar as informações contidas nos livros.

pra editar as informações precisamos de uma página de *update*. Nesse momento eu vejo que eu preciso de uma função que receba dois métodos diferentes:

a) se o usuário mandar um GET naquele *endpoint*, preciso que ele receba o template de update

b) se ele mandar um POST, quero que a minha instância seja atualizada

Além disso, preciso garantir que o usuário não está fazendo besteira e que os dados são, de fato, válidos. Dessa forma os níveis de complexidade aumentam ainda um pouquinho…

```python
# views.py
from django.http import Http404
from django. shortcuts import redirect, render

from .forms import BookForm
from .models import Book

def edit_book(request, pk):
  try:
    books = Book.objects.get(id=pk)
  except Book.DoesNotExist:
    raise Http404("Book does not exist")
    
  if request.method == 'GET':
    render(request, 'my-book-edit.html', {'form': BookForm, 'object': book}

  if request.method == 'POST':
    form = BookForm(request.POST)
    if form.is_valid():
      form.save()
    return redirect('/my-books')

# urls.py

urlpatterns = [
   url(r'book-update/(?P<pk>[-\w]+)', views.edit_book) 
]
```

É fácil perceber que o nível de complexidade começar a crescer rapidamente, a função já começa a ficar um pouco mais longa (um *try* e dois *if*s) e a coisa toda parece um pouco “feia”.

Para ser bem sincera, nem fizemos todos os tratamentos dos erros possíveis, por exemplo aqueles vindos no caso de um *form* inválido (quando o usuário fez algo errado).

E agora josé?


## O negócio todo parece meio esquisito…

Todas as etapas que eu mostrei até agora, foram exatamente os passos que eu fiz ao começar a criar o meu primeiro sistema mais complexo. Em algum momento eu parei, olhei…. e algo de errado não estava certo. Sabe como é?

Nesse momento comecei a olhar com mais carinho para as Class Based Views.

Django, por padrão, trás uma série de classes que têm as principais funcionalidades necessárias em um projeto: renderizar um template, buscar uma lista de instâncias no banco, 
buscar uma instância específica, criar ou editar uma instância, e por aí vai. 
Os tratamentos de erros, erros de *form*, retorno de um 404, tudo isso vem pré-tratado por padrão. Precisamos “apenas” passar algumas informações para a classe e pronto!

Primeira pergunta que vem provavelmente é: mas e se eu não quiser o comportamento padrão? E se eu quiser modificar algo? Neste caso, podemos sobreescrever os métodos da classe. Um pouco mais complexo, mas ainda sim mais fácil que escrever e tratar todos os possíveis erros e caminhos. Vamos ver como funciona?

## Renderizar templates continua simples

Para isso, vamos criar uma classe que herda da classe TemplateView. Precisamos apenas passar o template como atributo da classe e pronto. Repare que agora você chama no urls.py um método chamado “*as_view*”.

Dessa forma, a sua classe já se preocupa em pegar o request, e renderizar o template de forma correta. A função “*as_view*” é a responsável pelo urls enteder que mesmo sendo uma classe, ela deve ter o comportamento de uma view normal.

Continua simples, certo?


```python
# views.py
from django.views.generic import TemplateView

class WelcomeView(TemplateView):
  template_name = 'welcome.html'
  
# urls.py
urlpatterns = [
  url(r'welcome/', views.WelcomeView.as_view()) 
]
```

## A listagem já ficou mais simples

Esqueça fazer busca no banco e preencher o template com o conteúdo. Deixe que a classe ListView faça isso para você! Defina apenas o template e o modelo que você deseja fazer a listagem :)o

Obs: a listagem é passada para o template sob o nome “*object_list*”.

```python
# views.py
from django.views.generic import ListView

from .models import Book

class BooksView(ListView):
  model = Book
  template_name = 'my-books.html'
  
# urls.py
urlpatterns = [
  url(r'my-books/', views.BooksView.as_view()) 
]
```

## ...página de detalhe em apenas 2 linhas? Wut?

A criação de urls de detalhe é semelhante à de listagem. Deve-se definir o modelo e o template. Por padrão, o sistema procura pelo id da instância. Pronto! Tratamentos de erros embutidos. Começou a ficar interessante?

Obs: a instância é passada para o template sob o nome “*object*”.


```python
# views.py
from django.views.generic import DetailView

from .models import Book

class BookDetailView(DetailView):
  model = Book
  template_name = 'my-book-detail.html'
  
# urls.py
urlpatterns = [
  url(r'book-detail/(?P<pk>[-\w]+)', views.BookDetailView.as_view()) 
]
```

## E o update?

A ideia é a mesma para *updates* também, apenas algumas informações devem ser passadas. No caso do *update*, além do modelo e do template inicial, podemos passar um *form* e 
precisamos de uma url para onde o usuário deve ser redirecionado em caso do sucesso.

```python
# views.py
from django.views.generic import UpdateView

from .forms import BookForm
from .models import Book

class BookUpdateView(UpdateView):
  model = Book
  template_name = 'my-book-edit.html'
  form_class = BookForm
  success_url = '/my-books'
  
# urls.py
urlpatterns = [
  url(r'book-update/(?P<pk>[-\w]+)', views.BookUpdateView.as_view()) 
]
```

## Legal... mas ainda não entendi como mudar o comportamento padrão :/

As *class based views* possuem diversos métodos que podem ser sobreescritos para gerarem a informação que você precisa! Aqui vamos ver um exemplo do DetailView. Suponhamos que você deseje buscar os livros não pelo id mas por um código interno da livraria, certo?

Neste caso, queremos alterar a forma como a classe busca a instância dentro do meu banco de dados. A função que devemos sobreescrever é a “*get_objects*”. 
O retorno da função vai ser o resultado da busca pelo código. Pronto! Agora temos um *endpoint* de detalhe com busca personalizada :)

```python
# views.py
from django.views.generic import DetailView

from .models import Book

class BookDetailView(DetailView):
  model = Book
  template_name = 'my-book-detail.html'
  
  def get_object(self):
    return Book.objects.get(code=self.kwargs['code'])
  
# urls.py
urlpatterns = [
  url(r'book-detail/(?P<code>[-\w]+)', views.BookDetailView.as_view()) 
]
```

Nesse momento, você deve estar pensando: E como eu vou saber que função reescrever?

No meu caso, eu uso [este site](http://ccbv.co.uk/) que contém todos os métodos disponíveis para cada uma das classes. Vou seguindo os métodos que vão sendo chamados até encontrar aquele que faz mais ou menos o que eu gostaria. A partir daí, é compreender a função e sobreescrevê-la!

## E a autenticação? Como fica?

Mamão com açúcar! A classe LoginRequiredMixin serve justamente para isso. Faça a classe herdar também desta classe e defina o atributo “*login_url*” que é a url de 
*login* que o usuário deverá ser redirecionado, caso não esteja logado no sistema e tente acessar a página

```python
# views.py
from django.contrib.auth.mixins import LoginRequiredMixin
from django.views.generic import DetailView

from .models import Book

class BookDetailView(LoginRequiredMixin, DetailView):
  model = Book
  template_name = 'my-book-detail.html'
  login_url = '/'
  
# urls.py
urlpatterns = [
  url(r'book-detail/(?P<pk>[-\w]+)', views.BookDetailView.as_view()) 
]
```

## Finalmente...

Ufa! Um monte de coisa, um monte de texto! Parece meio mágico, mas essa mágica facilita demais a vida! E confesso que sem esse site ficaria bem mais difícil entender o que está acontecendo!

Para facilitar a compreensão do todo, eu fiz [este projetinho](https://github.com/leportella/class-based-views-example) que aplica um pouco do código que eu mostrei aqui.

Espero que seja útil para você como foi para mim!

![](https://media.giphy.com/media/6gy2ySS6nXaDe/giphy.gif)
