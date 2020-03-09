---
layout: post
title:  "Bibliotecas Python para Análise de Dados: NumPy e Pandas"
date:   2019-09-01 23:00:00
categories: ['Data Science', 'Pyhton']
cover: '/assets/images/python.jpg'
---

Fala, pessoal! Tudo bem? Hoje vou trazer pra vocês alguns módulos do [PyData Stack][pydata], conjunto de ferramentas do Python voltadas para análise de dados e machine learning.

Dentre os principais pacotes que formam o stack de ciência de dados do Python, estão o [NumPy][numpy], [Pandas][pandas], [Matplotlib][matplotlib], [SciPy][scipy], [Scikit-Learn][scikitlearn], [Bokeh][bokeh], [StatsModels][statsmodels] e o [Seaborn][seaborn]. Pretendo mostrar um pouco de cada um deles nesta sequência de posts.

Lembrando que, caso você já utilize o Anaconda, todos os pacotes citados acima já estão inclusos.

#### NumPy

O NumPy é um pacote voltado para computação matemática, e um dos mais importantes do PyData Stack. Ele oferece as bases matemáticas necessárias para construção de modelos de deep learning, machine learning e, consequentemente, aplicações de inteligência artificial. É possível utilizar os objetos nativos do NumPy para criação arrays ou matrizes, e assim usufruir das funções matemáticas oferecidas para operações com esses objetos.

Vamos começar importando o NumPy. Também é possível utilizar: from numpy import *  . Isso evitará a utilização de np., mas este comando importará todos os módulos do NumPy.

{% highlight python %}
# Importando o NumPy
import numpy as np
{% endhighlight %}

Agora vamos criar uma matriz e utilizar os alguns dos métodos fornecidos pelo NumPy:

{% highlight python %}
# Criando uma matriz a partir de uma lista de listas
lista = [[13,81,22], [0, 34, 59], [21, 48, 94]]
matriz = np.matrix(lista)

type(matriz)
#=> numpy.matrixlib.defmatrix.matrix

# Formato da matriz
np.shape(matriz)
#=> (3, 3)

print(matriz.dtype)
#=> int64
{% endhighlight %}

Também podemos utilizar o NumPy para realizar operações estatísticas, embora tenhamos outros pacotes do Python para isso, como por exemplo, o StatsModel, do qual irei falar na continuação deste post. Vamos ao exemplo:

{% highlight python %}
A = np.array([15, 23, 63, 94, 75])

# Calculando a média dos valores
np.mean(A)
#=> 54.0

# Calculando o desvio padrão (variação ou dispersão que existe em relação à média ou valor esperado)
np.std(A)
#=> 30.34468652004828

# Calculando a variância
np.var(A)
#=> 920.8
{% endhighlight %}

#### Pandas

O Pandas é uma espécie de Excel para linguagem Python, com o qual é possível manipular dados estruturados das mais variadas formas. Ele é um dos componentes principais no portifólio Python para realizar análise de dados, por tornar mais simples o *"slice and dice"* (fatiamento dos dados em diferentes perspectivas), além de seleção e agregações de subsets de dados.

Agora, vejamos como criar e manipular dataframes usando o Pandas. *Dataframes* são estruturas muito similares a uma planilha de Excel, ou uma tabela em um banco de dados relacional: possui linhas e colunas, onde são armazenados dados estruturados.

{% highlight python linenos %}
from pandas import DataFrame

data = {'Estado': ['Santa Catarina', 'Paraná', 'Goiás', 'Bahia'], 
        'Ano': [2002, 2003, 2004, 2005], 
        'População': [1.5, 1.7, 3.6, 2.4]}
frame = DataFrame(data)
frame
#=>            Estado   Ano  População
#   0  Santa Catarina  2002        1.5
#   1          Paraná  2003        1.7
#   2           Goiás  2004        3.6
#   3           Bahia  2005        2.4

type(frame)
#=> pandas.core.frame.DataFrame

frame['Estado']
#=> 0    Santa Catarina
#   1            Paraná
#   2             Goiás
#   3             Bahia

# Resumo do Dataframe
frame.describe()
#=>                Ano  População
#   count     4.000000   4.000000
#   mean   2003.500000   2.300000
#   std       1.290994   0.948683
#   min    2002.000000   1.500000
#   25%    2002.750000   1.650000
#   50%    2003.500000   2.050000
#   75%    2004.250000   2.700000
#   max    2005.000000   3.600000

frame.values
#=> array([['Santa Catarina', 2002, 1.5],
#          ['Paraná', 2003, 1.7],
#          ['Goiás', 2004, 3.6],
#          ['Bahia', 2005, 2.4]], dtype=object)

# Visualizando uma coluna index
print(frame.set_index('Ano'))
#=>               Estado  População
#   Ano                            
#   2002  Santa Catarina        1.5
#   2003          Paraná        1.7
#   2004           Goiás        3.6
#   2005           Bahia        2.4
{% endhighlight %}

No [próximo post][parte2], irei falar um pouco sobre os demais pacotes do PyData Stack, com mais alguns exemplos! Até mais! 👨‍💻🐍

[pydata]: https://pydata.org/
[numpy]: https://numpy.org/
[pandas]: https://pandas.pydata.org/
[matplotlib]: https://matplotlib.org/
[scipy]: https://www.scipy.org/
[scikitlearn]: https://scikit-learn.org/
[bokeh]: https://bokeh.pydata.org/en/latest/index.html
[statsmodels]: https://www.statsmodels.org/
[seaborn]: https://seaborn.pydata.org/
[parte2]: https://eqdrs.github.io/data%20science/pyhton/2019/09/03/modulos-python-para-analise-de-dados-parte-2.html