---
layout: post
title: "Meus 'truques' preferidos em Python - Parte I"
categories:
  - pt-br 
tags:
  - pt-br
  - python
featured-img: computer
last_modified_at: 2018-05-07T09:29:52-05:00
---


Recentemente, estive ajudando um amigo que transicionava do Matlab para o Python. Dando algumas dicas para ele, percebi 
que muitas das nuances legais que 
eu aprendi no Python foi alguém que me ensinou em um momento de "você conhece isso?" ou 
para resolver um problema bem específico que poderia ser resolvido de forma mais simples. 

Ao ajudar esse meu amigo que está lá do outro lado do mundo, lembrei da época em que não havia ninguém pra me 
ensinar um **truque** legal e, na verdade, não sabia nem que isso poderia existir.

Decidi então escrever meu **truques** favoritos e que, muitas vezes, deixam o código muito mais simples e legível. Esse texto 
vai estar entre o *cheat list* e um post simples e espero que seja útil.

![](https://media.giphy.com/media/o6jPMSGtbHJFC/giphy.gif)


## Permuta de valores entre variáveis

Agora, imagine que temos duas variáveis e queremos inverter os valores delas. Eu quero que a variável **a** passe a ter o 
valor de **b** e vice-versa. Podemos usar a mesma lógica de definição de variáveis que vimos no item anterior:

```python
a, b = 10, 5

a, b = b, a
```

## Recuperando informações de dicionário

Dicionários são estruturas maravilhosas e que usamos frequentemente. Agora, vamos imaginar que você vai receber um dicionário 
de um determinado lugar e você não tem certeza se uma determinada chave vai estar no dicionário. Como você vai fazer isso?
Se tentarmos buscar uma chave que não existe, teremos um retorno de `KeyError`. Uma primeira maneira intuita, seria fazer um 
`try`. Por exemplo:

```python
dicionario = {'Maria': '1235'}

try:
    valor = dicionario['chave'] 
except KeyError:
    valor = None
```

Parece muita complicação para uma coisa simples, né? Pois bem, os dicionários tem um método `get` que busca uma chave e retorna
`None` caso essa chave não exista:

```python
valor = dicionario.get('chave')
```

E melhor ainda! Caso haja um valor padrão, você pode adicioná-lo como um segundo parâmetro.

```python
valor = dicionario.get('chave', 123)

# caso a chave não exista:
>>> valor
123
```

## Os condicionais estão muito compridos?

Uma situação que eu passei também foi ter que passar por `if`s cujas verificações eram muito longas e o código ficava esquisito. 
Imagine algo tipo isso:

```python
if objeto.modelo.atributo['chave'] == 'alguma coisa aqui' and objeto.modelo.outro_atributo['outra_chave'] == 23 and ... :
    pass
```

Você entendeu... muitos caracteres para pouca verificação. Vamos pegar um cenário onde temos 4 variáveis que devem ser 
verificadas em um `if`. Nosso código ficaria algo assim:

```python
a, b, c, d = 1, 2, 3, 4

if a == 1 and b == 2 and c == 3 and d == 4:
    print('all True!')
```
Nesse caso, queremos que todas as condições sejam verdadeiras, certo? Cada verificador `==` vai retornar `True` ou `False` e,
portanto, entraremos no `if` só se todos forem verdadeiros.

Vamos fazer um pouco diferente: vamos colocar todos os condicionais em uma lista:

```python
condicoes = [
    a==1, 
    b==2, 
    c==3, 
    d==4
]
```

Maravilha! Agora vamos usar a maravilhosa função `all` para verificar se todas as condições são verdadeiras:

```python
if all(condicoes):
    print('todas as condições eram verdadeiras!')
```

Pronto! Muito mais simples e limpo e muito mais fácil de entender o que está acontecendo. Legal, né? 

Agora, se quisermos que nem todas as condições sejam verdadeiras (ou seja, apenas 1 ser verdadeira já basta), podemos 
usar a função `any`:

```python
if any(condicoes):
    print('alguma condição era verdadeira!')
```

## Para que precisamos de 'else'?

Esse é um dos meus preferidos! Vamos pegar uma função `modifica_valor` que recebe uma entrada e verifica se ela for igual a 
**a**, ela retorna 0, de outra forma 1. 

```python
def modifica_valor(x):
    if x == 'a':
        return 0
    else:
        return 1
```
Nesse caso, o `else` se torna completamente desnecessário. Porque qualquer caso cair no primeiro `if`, já vai sair da função. 
Dessa forma, nossa função pode ser simplificada para:

```python
def modifica_valor(lista):
    if x == 'a':
        return 0
    return 1
 ```

## Loops de uma linha só

Agora, quero falar sobre *list comprehensions* que eu estou carinhosamente chamando de *loops de uma linha só*. Esse é um 
conceito um pouco complexo, então eu não vou trazer uma longa exposição sobre todas as possibilidades que existem. Quero que 
você apenas visualize que isso é possível e, muitas vezes, útil. Ok?

Vamos para um exemplo inicial. Temos uma lista de inteiros e gostaríamos de criar uma nova lista que contenha os valores ao 
quadrado. O que podemos fazer é criar uma lista nova (`lista_modificada`), iterar sobre cada item da primeira lista e adicionar 
à nova lista o valor ao quadrado. O resultado é exatamente o que gostaríamos.

```python
lista = [1, 2, 3]

lista_modificada = []
for x in lista:
    lista_modificada.append(x**2)

# lista_modificada
[1, 4, 9]    
```

Perceba que "gastamos" 3 linhas em um problema relativamente simples. Com *list comprehension*, nós colocamos o `for` dentro 
de uma nova lista:

```python
lista = [1, 2, 3]

lista_modificada = [x**2 for x in lista]

# lista_modificada
[1, 4, 9]    
```

O que fizemos basicamente foi dizer: *eleve ao quadrado cada item existente na minha `lista`*. Ao fazemos isso dentro de uma nova 
lista, automaticamente dizemos que o novo item ao quadrado deve ser inserido na lista. Simples e "econômico". Claro que existem 
múltiplas formas de fazer isso e potencializar o uso desse método, mas você entendeu a ideia geral, espero! 

## Funções de uma linha só

Do mesmo jeito que podemos fazer loops em uma linha só também podemos criar funções de uma linha só. No Python, vamos 
usar o `lambda` para isso. No entanto, não se engane! Esse conceito, pra mim, foi muito difícil de entender e até hoje 
eu tenho dificuldades em usar o lambda quando as coisas começam a ficar complicadas. De novo, isso aqui é apenas uma "ideia 
geral" para que você se aprofunde.

Vou tentar explicar o porque usar o lambda em um exemplo prático.

Imaginemos o seguinte cenário: temos uma lista de itens e queremos transformar estes itens em valores de 0 ou 1. 

```python
lista = ['a', 'b', 'b', 'c', 'a', 'b']

lista_modificada = [0, 1, 1, 1, 0, 1]
```

Podemos fazer uma função que recebe a lista, varre cada um dos itens e retorn 0 ou 1 dependendo do item. Seria algo parecido 
com isso:

```python
def modifica_valor(lista):
    lista_modificada = []
    for x in lista:
        if x == 'a':
            lista_modificada.append(0)
        else:
            lista_modificada.append(1)
```

Vamos tirar o loop dentro da lista da função. A função ficará responsável apenas por retornar o valor 0 ou 1 dependendo do 
valor do item.

```python
def modifica_valor(x):
    if x == 'a':
        return 0
    return 1
            
lista_modificada = []
for x in lista:
    lista_modificada.append(modifica_valor(x))
```

Legal, agora a nossa função ficou bastante simples. Semelhante ao que fizemos no *list comprehension* podemos representar a 
mesma função da seguinte forma:

```python
transform = lambda x: 0 if x == 'a' else 1

# type(transform)
>>> function
```

Ou seja, ao receber um x retorne 0 caso ele seja igual a **a** ou 1 em qualquer outro caso. Isso pode ser usado, por 
exemplo, ao se passar uma função para um `map`, por exemplo, que vai aplicar essa função em cada item da lista. Aquele loop 
com vários `append` acaba virando isso:

```python
lista_transform = list(map(transform, lista))

# lista_transform
>>> [0, 1, 1, 1, 0, 1]
```

## Loops também tem else! 

Uma das coisas que mais me surpreendeu quando eu aprendi, foi o fato de que loops também tem `else`. São 3 casos que vou usar 
para exemplificar o uso:


### 1. Passamos uma lista vazia

Quando passamos uma lista vazia para que o loop ocorra, nada acontecerá, mas o `else` roda logo em seguida:

```python
lista = []

for x in lista:
    print('dentro do loop')
else:
    print('dentro do else')
    
# resultado
'dentro do else'
```

### 2. Quando o loop acontece normalmente

Quando o loop acontece de forma normal, todo ele é executado normalmente e, ao fim, a condição dentro do `else` é executada.

```python
lista = [1, 2]

for x in lista:
    print('dentro do loop')
else:
    print('dentro do else')
    
# resultado
'dentro do loop'
'dentro do loop'
'dentro do else'
```

### 3. Deixando de executar o 'else'

Se aplicarmos um break dentro do loop, o ciclo todo será interrompido e o código dentro do `else` não será executado.

```python
lista = [1]

for x in lista:
    print('dentro do loop')
    break
else:
    print('dentro do else')
    
# resultado
'dentro do loop'
```

## String comprehension?

Strings funcionam bem semelhantes a listas. Isso porque, na verdade, strings são listas de caracteres. Portanto, se fizermos uma 
iteração por uma string, o retorno será cada um dos caracteres.

```python
palavra = 'ola'

for i in palavra:
    print(i)
    
# resultado
o
l
a
```

Dessa forma, podemos fazer algo semelhante ao que vimos no *list comprehension* para fazer manipulações de strings. Por exemplo, 
vamos supor que você tenha uma string que contenha acentos, e você deseja removê-los. Usando essa ideia simples, podemos fazer:

```python
frase = 'O que? Eu não quero frases com pontuações! Chega!'

frase_sem_pontuacao = ''.join(char for char in frase if char not in [ '!', '?'])
```

A diferença entre o *list comprehension* e esse "*string comprehension*" é que o primeiro precisávamos colocar o loop entre 
as chaves da lista, enquanto agora usamos o método `join` da string vazia (`''`) para adicionar o resultado. Bem legal né?

## Checando um tipo

Se quisermos conferir se uma determinada variável é de um determinado tipo, usamos a função `type`:

```python
>>> type('oi')
str
```

Então seria natural pensar que ao conferirmos uma entrada usássemos algo parecido com: 

```python
type('oi') == str
```

Mas temos um método mais apropriado para fazer isso, que é o `isinstance`, que confere se aquela variável é uma instância de 
uma classe determinada:

```python
isinstance('oi', str):
```

Bem simples e elegante :)


## Adios!

É isso por hoje pessoal! Espero que tenham gostado e espero fazer novos posts conforme eu aprenda mais "truques"! 

![](https://media.giphy.com/media/3o6EhGvKschtbrRjX2/giphy.gif)
