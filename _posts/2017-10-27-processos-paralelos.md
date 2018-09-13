---
layout: post
title: "Como rodar processos em paralelo?"
categories:
  - pt-br
tags:
  - pt-br
  - python
  - data science
  - paralelismo
  - otimização
featured-img: type-machine
last_modified_at: 2018-08-01T13:25:52-05:00
---

Essa semana caí num caso em que eu tinha diversos scripts contendo análises que poderiam rodar em paralelo. Essas análises eram então usadas como base para uma outra análise, que devia rodar apenas depois de todas as outras acabarem. Algo parecido com isso:

![](https://i.imgur.com/jfX5XMZ.png)

*Temos 3 processos (scripts de análise) que são independentes um dos outros e podem rodar em paralelo. O 4o processo é uma análise que depende do resultado dos demais processos.*

Queríamos algumas coisas:

* Um fluxo de processos: Imagine que esses 3 processos do fluxograma representem, por exemplo, vários processos em diversos diretórios e que todos os processos independentes devem rodar em paralelo. Agora imagine que existem vários scripts de análise que usam os resultados dos processos independentes (nesse caso, chamamos eles de processos dependentes porque dependem do resultado dos primeiros processos). Esses scripts também podem rodar em paralelo e outros que dependem do resultado deste também, e assim vai
* Utilização da capacidade computacional
* Simplificação da forma como cada processo é chamado (de manual para automático)

Para conseguir tudo isso surgiu a ideia de criar um script “maestro” que coordena e chama todos os outros. No fim, o pipeline de processamento ficou bem mais simples e poderoso. Quer ver? :)

## Cenário 1: Rodando 3 processos em paralelo

Vamos assumir que eu tenho 3 scripts chamados **processo1**, **processo2** e **processo3**. 
O primeiro processo é o mais longo e o segundo, o mais rápido.

```python
# processo1.py
# Processo1: demora 10segundos antes de executar o print

from time import sleep                                                          
                                                                                
sleep(10)                                                                       
print('Fim do 1o processo')
```

```python
# processo2.py
# Processo2: print é executado imediatamente

print('Fim do 2o processo')
```


```python
# processo3.py
# Processo3: o print é executado depois de 3 segundos

from time import sleep                                                          
                                                                                
sleep(3)                                                                       
print('Fim do 3o processo')
```

Primeiramente, criamos uma tupla contendo o nome de todos os scripts que queremos que rodem em paralelo.

Também vamos precisar de uma função para executar um comando no sistema. 
Nesse caso vamos usar a biblioteca padrão **os** e o método `.system`, 
que permite que você dê os comandos ao sistema operacional como se fosse em um terminal qualquer. 
Então cada vez que passamos um arquivo pra esse método, é como se digitássemos no terminal 
`python myscript.py`.

Por último, instanciamos uma classe *Pool* da biblioteca de **multiprocessing** do Python. 
De acordo com a documentação, um objeto de processo *pool* controla uma série de *workers* onde 
seus processos podem ser submetidos. Legal. 
Precisamos dizer pra esse objeto quantos *workers* queremos usar ao trabalhar com os nossos processos. 
No nosso caso, vamos usar 3.

Finalmente passamos a nossa função e os processos que queremos paralelizar para o método *map* da 
nossa instância da *Pool*. 

```python
import os                                                                       
from multiprocessing import Pool                                                
                                                                                
                                                                                
processos = ('processo1.py', 'processo2.py', 'processo3.py')                                    
                                                  
                                                                                
def roda_processo(processo):                                                             
    os.system('python {}'.format(processo))                                       
                                                                                
                                                                                
pool = Pool(processes=3)                                                        
pool.map(roda_processo, processos) 
```

Essa função irá iterar por cada um dos processos, passar o processo pela função e 
paralelizá-lo conforme necessário. É como se fizemos em sequência no shell do Python:

`>>> roda_processo('processo1.py')`

`>>> roda_processo('processo2.py')`

`>>> roda_processo('processo3.py')`

Porém, ao invés de interpretar isso de forma sequencial, os *workers* executarão todos ao 
mesmo tempo (cada um em um *worker*).

Como temos 3 processos que executam em tempos diferentes, se os processos rodassem em sequência, teríamos que esperar todo o tempo do *processo1*, depois do *processo2* e só então nosso *processo3* 
seria iniciado. Como estamos executando todos ao mesmo tempo, 
os processos vão ser finalizados na ordem do mais rápido ao mais lento. O resultado portanto é:

![](https://i.imgur.com/GvIoQS5.png)

*Tempo total usado no processamento dos 3 scripts de processo e resultado em ordem de tempo de execução*

Nosso **processo2**, mais rápido, finaliza quase imediatamente. 
Nosso **processo3** toma mais um tempo enquanto o **processo1** ainda demora um 
considerável tempo para finalizar. 
Dessa forma, ao invés de demorar ~13 segundos para rodar (10 seg do **processo1**, 0 do **processo2** e 3 do **processo3**), o tempo total é reduzido para ~10 segundos. Aí sim, vemos vantagem né?

## Cenário 2: Rodando múltiplos processos em paralelo e em sequência

Agora, voltando para o início da conversa. Suponhamos que o **processo3** dependesse dos dois primeiros.
Como poderíamos fazer?

Poderíamos simplesmente separar o terceiro processo e fazer um map separado, exclusivo para ele:


```python
import os                                                                       
from multiprocessing import Pool                                                
                                                                                
                                                                                
processos = ('processo1.py', 'processo2.py')                                    
outros = ('processo3.py',)
                                                  
                                                                                
def roda_processo(processo):                                                             
    os.system('python {}'.format(processo))                                       
                                                                                
                                                                                
pool = Pool(processes=3)                                                        
pool.map(roda_processo, processos) 
pool.map(roda_processo, outros) 
```

Dessa forma, o resultado fica mas parecido com este:

![](https://i.imgur.com/2ja6VBK.png)

Temos os dois primeiros processos rodando em paralelo até o final e apenas então o **processo3** 
é executado.

Gostou? Vê algum lugar em que isso pode ser útil? :)
