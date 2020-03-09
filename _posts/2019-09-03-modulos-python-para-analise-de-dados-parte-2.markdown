---
layout: post
title:  "Bibliotecas Python para Análise de Dados: Matplotlib"
date:   2019-09-03 00:00:00
categories: ['Data Science', 'Pyhton']
cover: '/assets/images/python.jpg'
---

Olá, pessoal! Neste post, irei dar continuação à apresentação dos pacotes que integram o [PyData Stack][pydata]. Você pode conferir a primeira parte [clicando aqui][parte1].

Desta vez, irei apresentar mais um importante pacote do stack de ciência de dados do Python: o [Matplotlib][matplotlib].

O Matplotlib é um pacote voltado para visualização de dados e geração de gráficos. Junto com o [NumPy][numpy] e o [Pandas][pandas], forma a base do que fazemos com análise de dados em Python. Ele possui diversas ferramentas que facilitam a criação de gráficos e exportação dos mesmos em vários formatos.

Importante ressaltar que, com a visualização dos dados, nós pretendemos apresentar um resultado e, para isso, não precisamos de um gráfico super-poluído e de difícil leitura. *Menos é mais!*

Agora vejamos como ele funciona com alguns exemplos!

#### Construindo plots com o Matplotlib
<p />

```python
import matplotlib as mpl
import matplotlib.pyplot as plt

# O método plot() define os eixos do gráfico
plt.plot([1, 3, 5], [2, 5, 7])
plt.show()
```
![Output][graphic1]
```python
x = [1, 4, 5]
y = [3, 7, 2]
plt.plot(x, y)
plt.xlabel('Variável 1')
plt.ylabel('Variável 2')
plt.title('Teste Plot')
plt.show()
```
![Output][graphic2]
```python
x2 = [1, 2, 3]
y2 = [11, 12, 15]
plt.plot(x2, y2, label = 'Primeira Linha')
plt.legend()
plt.show()
```
![Output][graphic3]

#### Construindo gráficos de barras
<p />

```python
x = [2,4,6,8,10]
y = [6,7,8,2,4]
plt.bar(x, y, label = 'Barras', color = 'r')
plt.legend()
plt.show()
```
![Output][graphic4]
```python
x2 = [1,3,5,7,9]
y2 = [7,8,2,4,2]
plt.bar(x, y, label = 'Barras1', color = 'r')
plt.bar(x2, y2, label = 'Barras2', color = 'y')
plt.legend()
plt.show()
```
![Output][graphic5]
```python
idades = [22,65,45,55,21,22,34,42,41,4,99,101,120,122,130,111,115,80,75,54,44,64,13,18,48]

ids = [x for x in range(len(idades))]
plt.bar(ids, idades)
plt.show()
```
![Output][graphic6]

#### Construindo um scatterplot
<p />

```python
x = [1,2,3,4,5,6,7,8]
y = [5,2,4,5,6,8,4,8]
plt.scatter(x, y, label = 'Pontos', color = 'r', marker = 'o', s = 100)
plt.legend()
plt.show()
```
![Output][graphic7]

#### Gráfico de pizza
<p />

```python
fatias = [7, 2, 2, 13]
atividades = ['dormir','comer','passear','trabalhar']
colunas = ['c','m','r','k']

plt.pie(fatias, labels = atividades, colors = colunas, startangle = 90, shadow = True, explode = (0,0.1,0,0))
plt.show()
```
![Output][graphic8]

#### Stack plot
<p />

```python
dias = [1,2,3,4,5]
dormir = [7,8,6,77,7]
comer = [2,3,4,5,3]
trabalhar = [7,8,7,2,2]
passear = [8,5,7,8,13]

plt.stackplot(dias, dormir, comer, trabalhar, passear, colors = ['m','c','r','k','b'])
plt.show()
```
![Output][graphic9]

Por hoje é só, pessoal! No próximo post, irei falar sobre o **SciPy**, pacote do Python voltado para computação científica! Até mais! 👨‍💻

[pydata]: https://pydata.org/
[matplotlib]: https://matplotlib.org/
[scipy]: https://www.scipy.org/
[parte1]: https://eqdrs.github.io/data%20science/2019/09/01/modulos-python-para-analise-de-dados-parte-1.html
[numpy]: https://numpy.org/
[pandas]: https://pandas.pydata.org/

[graphic1]: /assets/images/matplotlib/output_5_0.png
[graphic2]: /assets/images/matplotlib/output_8_0.png
[graphic3]: /assets/images/matplotlib/output_10_0.png
[graphic4]: /assets/images/matplotlib/output_13_0.png
[graphic5]: /assets/images/matplotlib/output_15_0.png
[graphic6]: /assets/images/matplotlib/output_18_0.png
[graphic7]: /assets/images/matplotlib/output_24_0.png
[graphic8]: /assets/images/matplotlib/grafico-pizza.png
[graphic9]: /assets/images/matplotlib/output_27_0.png