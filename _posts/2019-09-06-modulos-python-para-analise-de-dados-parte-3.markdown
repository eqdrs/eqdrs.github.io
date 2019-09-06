---
layout: post
title:  "Bibliotecas Python para Análise de Dados Parte 3: SciPy"
date:   2019-09-06 01:00:00
categories: ['Data Science']
cover: '/assets/images/python.jpg'
---

Após conhecer os três pacotes fundamentais do [PyData Stack][pydata] ([NumPy][numpy], [Pandas][pandas] e [Matplotlib][matplotlib]), irei agora apresentar um pacote um pouco mais específico: o SciPy.

O [SciPy][scipy] é uma biblioteca voltada para computação científica. O conjunto de algoritmos e funções presentes no SciPy permite estender as funcionalidades do NumPy, transformando uma sessão do Python em um sistema de prototipagem equivalente ao [Matlab][matlab] ou [Octave][octave]. Nele, estão disponíveis uma série de pacotes para FFT (transformada rápida de Fourier), processamento de sinais e imagens, álgebra linear, etc.

*Talk is cheap! **Show me the code**!*

#### Aplicação em Álgebra Linear
<p />

{% highlight python linenos %}
from numpy import *
from pylab import *

A = array([[1,2,3], [4,5,6], [7,8,9]])
b = array([1,2,3])

# Resolvendo um sistema de equações lineares
x = solve(A, b)
x
#=> array([-0.23333333,  0.46666667,  0.1])

A = rand(3,3)
B = rand(3,3)

evals, evecs = eig(A)

evals
#=> array([ 1.12357541, -0.22168406,  0.04558271])

evecs
#=> array([[-0.69511641, -0.72547454,  0.47440231],
#          [-0.42786302,  0.29810437, -0.88013834],
#          [-0.5777079 ,  0.62033901,  0.01728984]])

# Decomposição da matriz em valores singulares
svd(A)
#=> (array([[-0.70675037,  0.70721235,  0.01883096],
#           [-0.42069691, -0.3987242 , -0.81488228],
#           [-0.56878645, -0.58384048,  0.57932052]]),
#    array([1.12603622, 0.29887502, 0.0337361 ]),
#    array([[-0.68149749, -0.38076708, -0.62496208],
#           [-0.50466044, -0.37392978,  0.77813518],
#           [ 0.52998019, -0.84569081, -0.06267413]]))

{% endhighlight %}

#### Transformada de Fourier
<p />

{% highlight python linenos %}
from scipy.fftpack import *

# Transformada de Fourier
N = len(t)
dt = t[1]-t[0]

F = fft(y2[:,0]) 

w = fftfreq(N, dt)

fig, ax = subplots(figsize=(9,3))
ax.plot(w, abs(F));
{% endhighlight %}
![Output][graphic1]

#### Otimização
<p />

{% highlight python linenos %}
from scipy import optimize

def f(x):
    return 4*x**3 + (x-2)**2 + x**4

fig, ax  = subplots()
x = linspace(-5, 3, 100)
ax.plot(x, f(x));
{% endhighlight %}
![Output][graphic2]
{% highlight python linenos %}
x_min = optimize.fmin_bfgs(f, -0.5)
x_min
#=> Optimization terminated successfully.
#            Current function value: 2.804988
#            Iterations: 4
#            Function evaluations: 18
#            Gradient evaluations: 6
#   array([0.46961743])
{% endhighlight %}

----
Lembrando que, para compreender o SciPy é necessário entender alguns conceitos um pouco mais avançados de Matemática e Estatística. [Neste link][doc], você encontra a documentação oficial do SciPy. Abaixo, vou deixar uma lista com o conjunto de pacotes para operações matemáticas e científicas disponíveis nele.

<table class="ArticleTableNoBorder">
	<tbody>
		<tr>
			<td><strong>Pacote</strong></td>
			<td><strong>Descrição</strong></td>
		</tr>
		<tr>
			<td><code>cluster</code></td>
			<td>Clustering algorithms</td>
		</tr>
		<tr>
			<td><code>constants</code></td>
			<td>Mathematical and physical constants</td>
		</tr>
		<tr>
			<td><code>fftpack</code></td>
			<td>Fourier transforms</td>
		</tr>
		<tr>
			<td><code>integrate</code></td>
			<td>Numerical integration</td>
		</tr>
		<tr>
			<td><code>interpolate</code></td>
			<td>Interpolation</td>
		</tr>
		<tr>
			<td><code>io</code></td>
			<td>Input and output</td>
		</tr>
		<tr>
			<td><code>linalg</code></td>
			<td>Linear algebra</td>
		</tr>
		<tr>
			<td><code>maxentropy</code></td>
			<td>Maximum entropy models</td>
		</tr>
		<tr>
			<td><code>misc</code></td>
			<td>Miscellaneous</td>
		</tr>
		<tr>
			<td><code>ndimage</code></td>
			<td>Multi-dimensional image processing</td>
		</tr>
		<tr>
			<td><code>odr</code></td>
			<td>Orthogonal distance regression</td>
		</tr>
		<tr>
			<td><code>optimize</code></td>
			<td>Optimization</td>
		</tr>
		<tr>
			<td><code>signal</code></td>
			<td>Signal processing</td>
		</tr>
		<tr>
			<td><code>sparse</code></td>
			<td>Sparse matrices</td>
		</tr>
		<tr>
			<td><code>spatial</code></td>
			<td>Spatial algorithms and data structures</td>
		</tr>
		<tr>
			<td><code>special</code></td>
			<td>Special functions</td>
		</tr>
		<tr>
			<td><code>stats</code></td>
			<td>Statistical functions</td>
		</tr>
		<tr>
			<td><code>stsci</code></td>
			<td>Image processing</td>
		</tr>
		<tr>
			<td><code>weave</code></td>
			<td>C/C++ integration</td>
		</tr>
	</tbody>
</table>

Até o próximo post, pessoal! 👨‍💻

[pydata]: https://pydata.org/
[matplotlib]: https://matplotlib.org/
[scipy]: https://www.scipy.org/
[numpy]: https://numpy.org/
[pandas]: https://pandas.pydata.org/
[matlab]: https://www.mathworks.com/products/matlab.html
[octave]: https://www.gnu.org/software/octave/
[doc]: https://docs.scipy.org/doc/

[graphic1]: /assets/images/scipy/output_14_0.png
[graphic2]: /assets/images/scipy/output_23_0.png
