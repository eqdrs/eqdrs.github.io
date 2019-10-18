---
layout: post
title:  "Controlando a trajetória de um robô diferencial: Follow the line!"
date:   2019-09-09 01:00:00
categories: ['Robotics']
cover: '/assets/images/vehicle.png'
---

Olá, pessoal! Tudo bem? Hoje, iremos projetar e simular controladores para um veículo diferencial, que serão responsáveis por fazê-lo seguir uma linha definida em um plano. Para isso, usaremos o [Octave][octave], uma linguagem computacional desenvolvida para computação matemática e que possui compatibilidade com [MATLAB][matlab]. Você pode fazer o download do executável do Octave [clicando aqui][download]. 

![Octave][robo5]

##### Uma breve introdução teórica

Um robô diferencial possui 2 rodas tracionadas independentemente e um ou mais pontos de contato. A única forma de atuação em um robô diferencial é pela imposição de velocidades independentes em cada roda. Neste experimento, faremos um robô móvel diferencial seguir uma linha no plano definido por `ax + by + c = 0`. 

Para isso, serão necessários dois controladores para ajustar a direção do veículo. Um dos controladores será responsável por "dirigir" o robô para minimizar a distância normal dele até a linha, de acordo com a equação:

![eq1][eq1]

Onde *d* é a distância normal do robô até a linha. Temos, então, o nosso primeiro controlador:

![eq3][eq3]

O segundo controlador ajusta o ângulo de orientação do veículo, para que ele fique paralelo à linha, de acordo com a equação:

![eq2][eq2]

Temos, então, o nosso segundo controlador:

![eq4][eq4]

Combinando as equações dos dois controladores, temos a nossa lei de controle do veículo:

![eq5][eq5]

Agora, vamos ao código!

##### Função de simulação do veículo diferencial
<p />

Precisaremos de uma função que simule o veículo diferencial com o qual trabalharemos. No Octave, criaremos um arquivo chamado `vehicle_diff.m`, no qual iremos definir esta função, adicionando características do veículo, como o ruído nas rodas, amplitude, espaço entre as duas rodas, etc.

{% highlight matlab linenos %}

function [P1, vr, wr] = vehicle_diff(P0, v, w, ts)
  % funcao que simula um robo diferencial
  %   P0 = [x; y; theta] posicao atual do robo
  %   P1 = [x1; y1; theta1] posicao apos aplicar os sinais de controle
  %   v = velocidade de referencia linear [m/s]
  %   w = velocidade de referencia angular [rad/s]
  %   vr = velocidade do robo linear [m/s]
  %   wr = velocidade do robo angular [rad/s]
  %   wd = velocidade angular da roda direita
  %   we = velocidade angular da roda esquerda
  %   ts = periodo de amostragem [s]
  %   l = espaco entre as duas rodas [m]
  %   r = raio das rodas [m]
  l = 0.09;
  r = 0.05;

  % Adicao de ruido na leitura das velocidades
  V = [v w]';  
  T = [r/2 (r + 0.002)/2; r/l -(r+0.002)/l];
  W = inv(T)*V;  % W = [wd we]'
  wd = W(1);
  we = W(2);

  % Velocidades estimadas
  Vest = T*[wd we]';
  vr = Vest(1);
  wr = Vest(2);

  % Matriz de rotacao
  theta = P0(3);
  R = [cos(theta) 0; sin(theta) 0; 0 1];

  % A posicao P1 eh a posicao atual mais a evolucao no espaco global
  P1 = P0 + R * (Vest * ts);
end


{% endhighlight %}

##### Implementando os controladores
<p />

Criaremos agora um arquivo chamado `navigation_controller.m`, no qual iremos implementar os nossos dois controladores de acordo com as equações apresentadas acima. Aqui, também iremos atualizar os estados do robô.

{% highlight matlab linenos %}
% Template
% P0 = posicao inical do robo
% P1 = posicao estimada 
% x,y,theta = trajetoria do robo
% i = amostras
% vref = velocidade de referencia linear do centro de massa do robo
% wref = velocidade de referencia angular do centro de massa do robo
% vrobo = velocidade linear do robo [m/s]
% wrobo = velocidade angular do robo [rad/s]
% Kd = Constante de Ganho
% Kh = Constante de H
% compWheel = Comprimento Roda Veiculo

% Define posicao inicial do robo
P0 = [0 0 pi/2]';
P1 = P0;

i = 1; 
x(i) = P0(1);
y(i) = P0(2);
theta(i) = P0(3);

% Velocidade de referencia 
vref = 0.4;

% Amostragem
ts = 0.1;

% Parametros da linha
L = [1 -2 4];

% Constante de Ganho
Kd = 0.1;

% Constante de H
Kh = 0.8;

% Comprimento da roda Veiculo
compWheel = 0.1;

while (i < 500) % navegando por 60 segundos
  d = (L(1) * x(i) + L(2) * y(i) + L(3)) / sqrt((L(1))^2 + (L(2))^2);

  diffAng = angdiff(atan2(-L(1), L(2)), theta(i));

  gama = (-Kd * d) + (Kh * diffAng);

  %Calculando velocidade angular de referencia
  wref = (vref / compWheel) * (tan(gama));

  % Funcao que simula o veiculo diferencial
  [P1, vr, wr] = vehicle_diff(P0, vref, wref, ts);
  i = i + 1;

  % Atualiza estado
  P0 = P1;

  % Logs da trajetoria
  x(i) = P1(1);
  y(i) = P1(2);
  theta(i) = P1(3);
  vrobo(i) = vr;
  wrobo(i) = wr;
  ghama(i) = gama;
end

plot_navigation;

{% endhighlight %}

##### Plotando a trajetória do veículo
<p />

Agora criaremos o arquivo `plot_navigation.m`, para visualizarmos a trajetória do nosso robô, assim como suas velocidades linear e angular e sua angulação.

{% highlight matlab linenos %}
plotsize = 10;
xmin = -plotsize;
ymin = -plotsize;
xmax = plotsize;
ymax = plotsize;

% Plota linha
alpha = -(L(1)/L(2));
beta = -(L(3)/L(2));
t = -20:0.1:15;
f = alpha*t+beta;

% Plotando trajetoria do robo
figure
plot(x,y,'bo');
hold on;
plot(x,y,'r-');
title('Trajetoria de robo diferencial');
xlabel('x (m)');
ylabel('y (m)');

plot(t,f,'r');

% Plotando angulo do robo em radianos
figure
plot(theta,'r-x');
xlabel('amostras');
ylabel('theta (rad)');

% Plotando velocidade linear robo em m/s
figure
plot(vrobo,'b-o');
xlabel('amostras');
ylabel('v (m/s)');

% Plotando velocidade angular do robo em rad/s
figure
plot(wrobo,'b-o');
xlabel('amostras');
ylabel('w (rad/s)');
{% endhighlight %}

##### Executando o código

Podemos executar nosso código pelo terminal do Octave, simplesmente digitando o nome do arquivo que queremos executar, neste caso, o `navigation_controller`. Não é necessário digitar o nome do arquivo com a extensão `.m`. Vamos aos resultados!

![robo3][robo3]
![robo4][robo4]
![robo1][robo1]
![robo2][robo2]

Como podemos ver, à medida que o robô se aproxima da linha, sua velocidade angular vai se tornando mais estável e tendendo a zero. Após aproximadamente 20 amostras, o robô consegue se guiar sobre a linha que foi definida!

Se quiserem se aprofundar um pouco mais nesta experiência, podem consultar o livro do Peter Corke *"Robotics, Vision and Control – Fundamental Algorithms in Matlab"*. O método *Following a line* é apresentado a partir da página 72 do livro, na seção 4.2.2.

Espero que tenham gostado! Até o próximo post! 💕

[matlab]: https://www.mathworks.com/products/matlab.html
[octave]: https://www.gnu.org/software/octave/
[download]: https://www.gnu.org/software/octave/download.html

[robo1]: /assets/images/robo1.jpg
[robo2]: /assets/images/robo2.jpg
[robo3]: /assets/images/robo3.jpg
[robo4]: /assets/images/robo4.jpg
[robo5]: /assets/images/robo5.jpeg
[eq1]: /assets/images/eq1.jpeg
[eq2]: /assets/images/eq2.jpeg
[eq3]: /assets/images/eq3.jpeg
[eq4]: /assets/images/eq4.jpeg
[eq5]: /assets/images/eq5.jpeg
