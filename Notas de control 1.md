# Notas de control 1

# Temario

1. Representación del sistema en la forma de variables de estado.
   1. Variables de Fase.
   2. Variables canónicas.
   3. Variables físicas.
2. Control Proporcional, control proporcional derivativo, control proporcional integral derivativo.
3. Estabilidad.
4. Asignación de polos: Fórmula de Ackermann.
5. Regulador Cuadrático lineal.
6. Control Adaptable. Servomecanismos de primer orden.
7. Observadores lineales.



## Notación

Los vectores los representaremos con **negritas y minúsculas**  y las matrices con **negritas y mayúsculas**. por ejemplo;
$$
\mathbf{A}\mathbf{x} = \mathbf{b}
$$
$\mathbf{A}$ es una matriz, $\mathbf{x}$ y $\mathbf{b}$ son vectores.

## Sistemas

````mermaid
graph LR
   t1[System Configurations]  --> t2[Open-Loop Systems] 
   						   t1 --> t3[Closed-loop Systems]
   						   
classDef estilos fill:#f0f0f0,stroke:#333333;
class t1,t2,t3 estilos;
````

> **Definición: Estado** 
>
> El estado de un sistema dinámico es el conjunto de variables más pequeño llamadas *variables de estado*, de forma que el conocimiento de estas variables en un tiempo $t = t_0$ junto con el conocimiento de la entrada para $t \geq t_0$ determinan el comportamiento del sistema en cualquier $t \geq t_0$ 



> **Definición: Variables de estado**
>
> Las variables de un sistema dinámico son las variables que constituyen el menor conjunto de variables que determinan el estado del sistema dinámico. Si al menos se necesitan $n$ variables $x_{1},x_{2}, \ldots,x_{n}$ para describir completamente el comportamiento de un sistema dinámico (de forma que una vez que la entrada para  $t \geq t_0$  está dada y el estado inicial en $t = t_0$  está especificado, el estado futuro del sistema está determinado completamente), entonces tales $n$ variables son un conjunto de variables de estado.  Obsérvese que las variables de estado no necesitan ser físicamente medibles o cantidades
> observables. Se pueden seleccionar como variables de estado variables que no representan cantidades físicas y aquellas que no son medibles ni observables.



> **Definición: Vector de estado**
> Si se necesitan $n$ variables de estado para describir completamente el comportamiento de un sistema dado, entonces esas $n$ variables de estado se pueden considerar como las $n$ componentes de un vector $\mathbf{x}$. Este vector se denomina vector de estado. Un vector de estado es, por lo tanto, un vector que determina unívocamente el estado del sistema $\mathbf{x}(t)$ en cualquier instante del tiempo $t \geq t_0$ una vez que se conoce el estado en $t = t_0$ y se especifica la entrada $u(t)$ para $t \geq t_0$ .



## Ecuación en espacio de estados


$$
\begin{equation} 
		\begin{array}{lcl}
		\dot{\mathbf{x}}(t) & = & \mathbf{A}(t) \mathbf{x}(t) + \mathbf{B}(t) \mathbf{u}(t) \\
			\mathbf{y}(t)   & = & \mathbf{C}(t) \mathbf{x}(t) + \mathbf{D}(t) \mathbf{u}(t)\\
		\end{array}
		\label{ec:oscilador}
	\end{equation}
$$
* $\mathbf{A}(t)$ : Matriz de estado
* $\mathbf{B}(t)$ : Matriz de entrada
* $\mathbf{C}(t)$ : Matriz de salida
* $\mathbf{A}(t)$ : Matriz de transmisión directa
* $\mathbf{x}(t)$ : Vector de estados
* $\mathbf{u}(t)$ : Vector de entrada
* $\mathbf{y}(t)$ : Vector de salida


![1.diagrama](img\1.diagrama.png)


## Espacio de estados $\to$ Función de transferencia

$$
\frac{Y(s)}{U(s)} = G(s)
$$

$$
	\begin{equation} 
		\begin{array}{lcl}
		\dot{\mathbf{x}} & = & \mathbf{A} \mathbf{x}+ \mathbf{B} u\\
			y   & = & \mathbf{C}\mathbf{x}+ D u\\
		\end{array}
	\end{equation}
$$

Aplicando la transformada de Laplace
$$
\begin{equation} 
		\begin{array}{rcl}
		s{\mathbf{X}}(s) - \mathbf{x}(0) & = & \mathbf{A} \mathbf{X}(s) + \mathbf{B} U(s)\\
			Y(s)   & = & \mathbf{C}\mathbf{X}(s)+ DU(s)\\
		\end{array}
	\end{equation}
$$
Cuando las condiciones iniciales son cero, entonces $\mathbf{x}(0)$ es cero:
$$
\begin{equation} 
		\begin{array}{rcl}
		s{\mathbf{X}}(s) - \mathbf{A} \mathbf{X}(s)  & = & \mathbf{B} U(s)\\
		(s\mathbf{I} - \mathbf{A} )\mathbf{X}(s)  & = & \mathbf{B} U(s)\\
		\end{array}
	\end{equation}
$$
Pre-multiplicando por $ (s\mathbf{I} - \mathbf{A} )^{-1}$ en ambos ambos miembros de la ecuación:
$$
	\begin{equation} 
		\begin{array}{rcl}
		s{\mathbf{X}}(s)  & = & (s\mathbf{I} - \mathbf{A} )^{-1} \mathbf{B} U(s)
		\end{array}
	\end{equation}
$$
Sustituyendo la ecuación anterior en la expresión para $y$, se llaga a:
$$
\begin{equation} 
		\begin{array}{rcl}
		Y(s)  & = & [ \,\, \mathbf{C} (s\mathbf{I} - \mathbf{A} )^{-1} \mathbf{B} + D \,\,]U(s)
		\end{array}
	\end{equation}
$$
Finalmente para encontrar $G(s)$:
$$
	\begin{equation} 
		\begin{array}{rcl}
		G(s)  & = & \mathbf{C} (s\mathbf{I} - \mathbf{A} )^{-1} \mathbf{B} + D 
		\end{array}
	\end{equation}
$$
Obsérvese que el segundo miembro de la ecuación anterior contiene  $ (s\mathbf{I} - \mathbf{A} )^{-1}$ . Por lo tanto, $G(s)$ se escribe como:
$$
G(s) = \frac{Q(s)}{  |s\mathbf{I} - \mathbf{A} | }
$$
donde $Q(s)$ es un polinomio en $s$. Por lo tanto,  $ |s\mathbf{I} - \mathbf{A} |$  es igual al polinomio característico de $G(s)$. En otras palabras, los valores propios de $\mathbf{A}$ son idénticos a los polos de $G(s)$.



````matlab
clear;
close all;
clc;

syms s;
A = [-4,-1;3,-1 ];
B = [1 1].';
C = [1 0];
D = 0;
I = eye(size(A));

G = simplify( C*inv((s*I - A))*B + D);
pretty(G);
````

**Código 1:**  Convertir de espacio de estados a función de transferencia.



## Ecuaciones diferenciales escalares $\to$ Espacio de estados

Tenemos:
$$
y^{(n)}+ a_{1}y^{(n-1)} + \dots + a_{n-1}\dot{y} + a_{n}y = u \tag{ODE escalar}
$$
Si se define:
$$
	\begin{equation} 
		\begin{array}{rcl}
		 x_{1} & = & y \\
		 x_{2} & = & \dot{y}\\
		 	  & \vdots & \\
		x_{n} & = & y^{(n-1)}\\
		\end{array}
	\end{equation}
$$
Entonces la ($\text{ODE escalar}$) se escribe como:
$$
\begin{equation} 
		\begin{array}{rcl}
		 \dot{x_{1}} & = & x_{2} \\
		 \dot{x_{2}} & = & x_{3}\\
		 	  & \vdots & \\
		\dot{x_{n-1}} & = & x_{n}\\
		\dot{x_{n}} & = & -a_{n}x_{1} - \dots - a_{1}x_{n} + u\\
		\end{array}
	\end{equation}
$$
o bien:
$$
\mathbf{\dot{x}} = \mathbf{A}\mathbf{x} + \mathbf{B}\mathbf{u}
$$
donde 
$$
\mathbf{x} = \left[ \begin{array}{c}
		x_{1}\\
		x_{2}\\
		\vdots\\
		x_{n}
		\end{array}
		\right] \quad \mathrm{y} \quad
	\mathbf{A} = \left[ \begin{array}{ccccc}
		0 & 1 & 0& \cdots &	0 \\
		0 & 0 & 1& \cdots &	0 \\
		\vdots & \vdots	&  \vdots &\ddots &	\vdots\\
		0 & 0 &  0 & \cdots &	1\\
		-a_{n} & -a_{n-1} & -a_{n-2}& \cdots &	-a_{1}
		\end{array}
		\right] \quad \mathrm{y} \quad
	\mathbf{B} = \left[ \begin{array}{c}
		0\\
		0\\
		\vdots\\
		0\\
		1
		\end{array}
		\right]
$$
La salida se obtiene mediante:
$$
y = \left[ \begin{array}{lccc}
		1 & 0 & \cdots &	0 \\
		\end{array}
		\right]
 \left[ \begin{array}{c} x_{1}\\x_{2}\\\vdots\\x_{n}\end{array}\right]
$$

$$
y = \mathbf{C}\mathbf{x} \qquad \text{donde} \qquad 
 \mathbf{C} =  \left[ \begin{array}{lccc}
		1 & 0 & \cdots &	0 \\
		\end{array}
		\right]
$$

Observar que $D$ es cero y también que se puede convertir fácilmente a función de transferencia utilizando la metodología mostrada anteriormente y obtener:
$$
\frac{Y(s)}{U(s)} = \frac{1}{s^{n} + a_{n}s^{n-1}  + \cdots + a_{n-1}s + a_{n}}
$$




## Ecuaciones diferenciales lineales $\to$ Espacio de estados

Tenemos:
$$
y^{(n)}+ a_{1}y^{(n-1)} + \dots + a_{n-1}\dot{y} + a_{n}y = b_{0}u^{(n)} + b_{1}u^{(n-1)} + \dots + b_{n-1}\dot{u} + b_{n}u \tag{ODE}
$$
Si se define:
$$
\begin{equation} 
		\begin{array}{rclcc}
		 x_{1} & = & y - \beta_{0}u &  &\\
		 x_{2} & = & \dot{y} - \beta_{0}\dot{u} - \beta_{1}u & = & \dot{x_{1}} - \beta_{1}u \\
		 x_{3} & = & \ddot{y} - \beta_{0}\ddot{u} - \beta_{1}\dot{u} - \beta_{2}u & = & \dot{x_{2}} - \beta_{2}u \\
		 	  & \vdots & \\
		x_{n} & = & y^{(n-1)} - \beta_{0} u^{(n-1)} - \beta_{1} u^{(n-2)} - \dots - \beta_{n-1} \dot{u} - \beta_{n-1}u & = & \dot{x_{n-1}} - \beta_{n-1}u\\
		\end{array}
	\end{equation}
$$
donde $\beta_{0},\beta_{1},\beta_{2},\ldots ,\beta_{n-1}$ se determinan a partir de:
$$
\begin{equation} 
		\begin{array}{rcl}
		 \beta_{0} & = & b_{0}\\
		 \beta_{1} & = & b_{1} - a_{1}\beta_{0}\\
		 \beta_{2} & = & b_{2} - a_{1}\beta_{1} - a_{2} \beta_{0}\\
		 \beta_{3} & = & b_{3} - a_{1}\beta_{2} - a_{2}\beta_{1} - a_{3}\beta_{0}\\
		     	   & \vdots & \\
		 \beta_{n-1} & = & b_{n-1} - a_{1}\beta_{n-2} - \ldots - a_{n-1}\beta_{1} - a_{n-1}\beta_{0}\\
		\end{array}
	\end{equation}
$$
Entonces la ($\text{ODE}$) se escribe como:
$$
\begin{equation} 
		\begin{array}{rcl}
		 \dot{x_{1}} & = & x_{2} + \beta_{1}u\\
		 \dot{x_{2}} & = & x_{3} + \beta_{2}u\\
		 	  & \vdots & \\
		\dot{x_{n-1}} & = & x_{n} + \beta_{n-1}u\\
		\dot{x_{n}} & = & -a_{n}x_{1} - a_{n-1}x_{2} - \dots - a_{1}x_{n} + \beta_{n}u\\
		\end{array}
	\end{equation}
$$
donde $\beta_{n}$:
$$
\beta_{n} = b_{n} - a_{1}\beta_{n-1} - \ldots - a_{n-1}\beta_{1} - a_{n}\beta_{0}
$$
Reescribiendo en forma matricial: 
$$
	\left[ \begin{array}{c}
		\dot{x_{1}}\\
		\dot{x_{2}}\\
		\vdots\\
		\dot{x_{n-1}}\\
		\dot{x_{n}}
		\end{array}
		\right] =
		
	 \left[ \begin{array}{ccccc}
		0 & 1 & 0& \cdots &	0 \\
		0 & 0 & 1& \cdots &	0 \\
		\vdots & \vdots	&  \vdots &\ddots &	\vdots\\
		0 & 0 &  0 & \cdots &	1\\
		-a_{n} & -a_{n-1} & -a_{n-2}& \cdots &	-a_{1}
		\end{array}
		\right] 
	
	  \left[ \begin{array}{c}
		x_{1}\\
		x_{2}\\
		\vdots\\
		x_{n-1}\\
		x_{n}
		\end{array}
		\right] 
		
		
		
		
	 + \left[ \begin{array}{c}
		\beta_{1}\\
		\beta_{2}\\
		\vdots\\
		\beta_{n-1}\\
		\beta_{n}
		\end{array}
		\right]u
$$

$$
y = \left[ \begin{array}{lccc}
		1 & 0 & \cdots &	0 \\
		\end{array}
		\right]
 \left[ \begin{array}{c} x_{1}\\x_{2}\\\vdots\\x_{n}\end{array}\right] + \beta_{0}u
$$

o bien:
$$
\begin{equation} 
		\begin{array}{lcl}
		\dot{\mathbf{x}} & = & \mathbf{A} \mathbf{x}+ \mathbf{B} u\\
			y   & = & \mathbf{C}\mathbf{x}+ D u\\
		\end{array}
	\end{equation}
$$
También que se puede convertir fácilmente a función de transferencia utilizando la metodología mostrada anteriormente y obtener:
$$
\frac{Y(s)}{U(s)} = \frac{b_{0}s^{n} + b_{1}s^{n-1} + \dots + b_{n-1}s + b_{n}}{s^{n} + a_{n}s^{n-1}  + \cdots + a_{n-1}s + a_{n}}
$$

## Linealización (Nise 6ta Ed.)

Cuando linealizamos una ecuación diferencial no lineal, la linealizamos para entradas de pequeña señal sobre la solución de estado estacionario cuando la entrada de entrada de pequeña señal es igual a cero. Esta solución de estado estacionario se llama
equilibrio y se selecciona como el segundo paso en el proceso de linealización.

Para linealizar podemos utilizar la ecuación:
$$
f(x)  \approx f(x_{0}) + m_{a}(x - x_{0}) \approx f(x_{0}) + m_{a} \delta_{x} \tag{Linealización Nise}
$$
donde $m_{a}$ es la pendiente en el punto $(x_{0}, f(x_{0}))$.



### Ejemplo 1: 

Linealiza $f(x) = 5 \cos (x)$ en el punto $x=\pi/2$

$\frac{df}{dx} = -5 \sin (x)$, en el punto  $x=\pi/2$ es $-5$. Y $f(x_{0}) = f(\pi/2) = 5 \cos (\pi / 2) = 0 $

Usando la ecuación de linealización:
$$
f(x) = -5 \delta_{x}
$$
para valores pequeños alrededor de $\pi/2$.



Para formalizar lo anterior recurrimos a la serie de Taylor
$$
f(x) \approx f(x_{0}) + \frac{f'(x_{0})}{1!}(x - x_{0}) + \frac{f''(x_{0})}{2!}(x - x_{0}) + \dots = \sum_{n = 0}^{\infty} \frac{f^{(n)}(x_{0})}{n!} (x - x_{0})^{n}
$$
Para valores pequeños de $x$ alrededor de $x_{0}$, podemos omitir los términos de orden superior de modo que obtenemos
$$
f(x) - f(x_{0}) \approx f'(x_{0}) (x - x_{0}) \qquad \text{o} \qquad \delta f(x) \approx m |_{x = x_0} \delta_{x}
$$

### Ejemplo 2:

Linealizar la ecuación diferencial $\frac{d^{2} x}{dt^{2}} + 2\frac{dx}{dt} + \cos{x} = 0$ en el punto $x = \pi / 4$.

El término no lineal es $\cos x$ y como lo queremos linealizar en el punto $x = \pi / 4$, podemos elegir $x = \delta_{x} + \pi / 4$, donde $\delta_{x}$ es un pequeño desplazamiento cerca de $\pi / 4$. sustituyendo la nueva $x$.
$$
\frac{d^{2} \left( \delta_{x} + \frac{\pi}{4}\right)}{dt^{2}} + 2\frac{d\left( \delta_{x} + \frac{\pi}{4}\right)}{dt} + \cos{\left( \delta_{x} + \frac{\pi}{4}\right)} = 0
$$
pero como :
$$
\frac{d^{2} \left( \delta_{x} + \frac{\pi}{4}\right)}{dt^{2}} = \frac{d^{2} \delta_{x} }{dt^{2}}
$$

$$
\frac{d\left( \delta_{x} + \frac{\pi}{4}\right)}{dt}  = \frac{d \delta_{x} }{dt}
$$

y utilizando la serie de Taylor para el término no lineal donde, $f(x) = \cos{\left( \delta_{x} + \frac{\pi}{4}\right)}$, $f(x_{0}) = \cos(\pi/4)$ y  $(x - x_{0}) = \delta_{x}$ entonces:
$$
\cos{ \left( \delta_{x} + \frac{\pi}{4}\right)} - \cos{ \left( \frac{\pi}{4}\right)} = \frac{d \cos x}{dx} \Biggl|_{x = \pi / 4} \delta x = -\sin{\left( \frac{\pi}{4}\right)} \delta_x
$$
entonces:
$$
\cos{ \left( \delta_{x} + \frac{\pi}{4}\right)} = \cos{ \left(\frac{\pi}{4}\right)} - \sin{ \left(\frac{\pi}{4}\right)}\delta_{x} = \frac{\sqrt{2}}{2} - \frac{\sqrt{2}}{2} \delta_{x}
$$
Sustituyendo todo:
$$
\frac{d^{2} \delta_{x} }{dt^{2}} + 2\frac{d \delta_{x} }{dt}  - \frac{\sqrt{2}}{2} \delta_{x} = -\frac{\sqrt{2}}{2}
$$
Ahora podemos resolver la ecuación anterior sin olvidar que  $x = \delta_{x} + \pi / 4$, 



## Linealización (Ogata 5ta Ed.)

Con la finalidad de obtener un modelo matemático lineal para un sistema no lineal, se supone que las variables sólo se desvían ligeramente de alguna condición de operación. Considérese un sistema cuya entrada es $x(t)$ y cuya salida es $y(t)$. La relación entre $y(t)$ y $x(t)$ se obtiene mediante:
$$
y = f(x)
$$
Si la condición de operación normal corresponde a $x_{0}$ y $y_{0} = f(x_{0})$. Utilizando la serie de Taylor 
$$
y = f(x) \approx \sum_{n = 0}^{\infty} \frac{f^{(n)}(x_{0})}{n!} (x - x_{0})^{n}
$$
donde las derivadas se evalúan en $x = x_{0}$. Si la variación $x - x_{0}$ es pequeña, es posible no considerar los términos de orden superior. Entonces:
$$
y = f(x_{0}) + K (x - x_{0})
$$
donde $K = \frac{df}{dx} \Biggl|_{x = x_{0}}$, entonces:
$$
y - y_{0} = K (x - x_{0}) \tag{Linealización Ogata}
$$
La ecuación anterior nos da un modelo para linealizar cerca del punto de operación $x = x_{0}$ y $y = y_{0}$

> **Nota**: La notación original de Ogata se consigue cambiando por la siguiente notación $ x_{0} = \overline{x}$ , $y_{0} = \overline{y}$ , $ \overline{y} = f( \overline{x})$

------

Si tenemos una función no lineal cuya salida $y$ dependa de dos entradas $x_{1}$ y $x_{2}$, es decir:
$$
y = f(x_{1},x_{2})
$$
entonces la ecuación de linealización se convierte en:
$$
y - \overline{y_{1}} = K_{1} (x_{1} - \overline{x_{1}}) + K_{2} (x_{2} - \overline{x_{2}}) \tag{Linealización dos variables}
$$
donde:
$$
K_{1} = \frac{\partial f}{ \partial x_{1}} \Biggl|_{x_{1} = \overline{x_{1}}, x_{2} = \overline{x_{2}} } \qquad K_{2} = \frac{\partial f}{ \partial x_{2}} \Biggl|_{x_{1} = \overline{x_{1}}, x_{2} = \overline{x_{2}} } \qquad \overline{y} = f(\overline{x_{1}},\overline{x_{2}})
$$


## Linealización de modelos matemáticos no lineales

>  **Definición: Sistemas no lineales**
>
>  Un sistema es no lineal si no se aplica el principio de superposición. Por tanto, para un sistema no lineal la respuesta a dos entradas no puede calcularse tratando cada entrada a la vez y sumando los resultados.

La respuesta de un sistema lineal es la superposición de su respuesta **libre** (a condiciones iniciales, sin excitación) y su respuesta **forzada** (a excitación, con condiciones iniciales nulas -relajado).

### Jacobiano

$$
\frac{\partial f}{\partial x} \overset{\Delta}{=} \left[ \begin{array}{ccccc}
		\frac{\partial f_{1}}{\partial x_{1}} & \frac{\partial f_{1}}{\partial x_{2}} &  \cdots &	\frac{\partial f_{1}}{\partial x_{n}} \\
		\frac{\partial f_{2}}{\partial x_{1}} & \frac{\partial f_{2}}{\partial x_{2}} &  \cdots &	\frac{\partial f_{2}}{\partial x_{n}} \\
		\vdots 	&  \vdots &\ddots &	\vdots\\
		\frac{\partial f_{n}}{\partial x_{1}} & \frac{\partial f_{n}}{\partial x_{2}} &  \cdots &	\frac{\partial f_{n}}{\partial x_{n}} \\
		\end{array}
		\right]
$$



## Tarea 1

**B-2-9.** Considere el sistema descrito mediante
$$
\dddot{y} + 3\ddot{y} + 2 \dot{y} = u
$$
Obtenga una representación en el espacio de estado del sistema.

**Solución:**

Se define :
$$
   \begin{equation} 
		\begin{array}{rcl}
		 x_{1} & = & y \\
		 x_{2} & = & \dot{y}\\
		 x_{3} & = & \ddot{y}\\
		\end{array}
	\end{equation}
$$
Entonces:
$$
   \begin{equation} 
		\begin{array}{rcl}
		 \dot{x_{1}} & = & x_{2} \\
		 \dot{x_{2}} & = & x_{3}\\
		 \dot{x_{3}} & = & -2 x_{2} -3 x_{3} + u\\
		\end{array}
	\end{equation}
$$
Finalmente podemos representar el sistema como:
$$
\boxed{ \left[ \begin{array}{c}
		\dot{x_{1}}\\
		\dot{x_{2}}\\
		\dot{x_{3}}
		\end{array}
		\right] =
		
	 \left[ \begin{array}{ccc}
		0 & 1 & 0 \\
		0 & 0 & 1 \\
		0 & -2 & -3
		\end{array}
		\right] 
	
	  \left[ \begin{array}{c}
		x_{1}\\
		x_{2}\\
		x_{3}
		\end{array}
		\right] 
		
	 + \left[ \begin{array}{c}
		0\\
		0\\
		1
		\end{array}
		\right]u}
$$

$$
\boxed{ y = \left[ \begin{array}{lcc}
		1 & 0 & 	0 \\
		\end{array}
		\right]
 \left[ \begin{array}{c} x_{1}\\x_{2}\\ x_{3} \end{array}\right]}
$$

**B-2-10.** Considere el sistema descrito mediante
$$
\left[ \begin{array}{c}
		\dot{x_{1}}\\
		\dot{x_{2}}
		\end{array}
		\right] =
		
	 \left[ \begin{array}{cc}
		-4 & -1 \\
		3 & -1  \\
		\end{array}
		\right] 
	
	  \left[ \begin{array}{c}
		x_{1}\\
		x_{2}
		\end{array}
		\right] 
		
	 + \left[ \begin{array}{c}
		1\\
		1
		\end{array}
		\right]u
$$

$$
y = \left[ \begin{array}{lc}
		1 & 0  \\
		\end{array}
		\right]
 \left[ \begin{array}{c} x_{1}\\x_{2} \end{array}\right]
$$

**Solución:**

Para encontrar la función de transferencia es necesario utilizar la siguiente ecuación:
$$
\begin{array}{rcl}
		G(s)  & = & \mathbf{C} (s\mathbf{I} - \mathbf{A} )^{-1} \mathbf{B} + D 
		\end{array}
$$
De modo que al aplicarla obtenemos:
$$
G(s) = 
		\left[ \begin{array}{cc}
		1 & 0\\
		\end{array}
		\right]


	  \left( 
	  s \left[ \begin{array}{cc}
		1 & 0\\
		0 & 1\\
		\end{array}
		\right] - 
		\left[ \begin{array}{cc}
		-4 & -1\\
		3 & -1\\
		\end{array}
		\right]
		\right)^{-1} 
		
		\left[ \begin{array}{c}
		1 \\
		1 
		\end{array}
		\right] + 
		
		\left[ \begin{array}{c}
		0 \\
		0 
		\end{array}
		\right]
$$

$$
G(s) = \left[ \begin{array}{cc}
		1 & 0\\
		\end{array}
		\right]


	  \left( 
	 	\frac{1}{s^{2} + 5s + 7}
		\left[ \begin{array}{cc}
		s+1 & -1\\
		3 & s+4\\
		\end{array}
		\right]
		\right)
		
		\left[ \begin{array}{c}
		1 \\
		1 
		\end{array}
		\right] + 
		
		\left[ \begin{array}{c}
		0 \\
		0 
		\end{array}
		\right]
$$

$$
G(s) = \left[ \begin{array}{cc}
		\frac{s+1}{s^{2} + 5s + 7} & \frac{-1}{s^{2} + 5s + 7}\\
		\end{array}
		\right]



		
		\left[ \begin{array}{c}
		1 \\
		1 
		\end{array}
		\right] + 
		
		\left[ \begin{array}{c}
		0 \\
		0 
		\end{array}
		\right]
$$

$$
\boxed{G(s) = \frac{s}{s^{2} + 5s + 7}}
$$

````
clear;
close all;
clc;

syms s;
A = [-4,-1;3,-1 ];
B = [1 1].';
C = [1 0];
D = 0;
I = eye(size(A));

G = simplify( C*inv((s*I - A))*B + D);
pretty(G);
````

**Código:** Transformar de SS a TF en MATLAB.



**B-2-13.** Linealice la ecuación no lineal
$$
z = x^2 + 8xy + 3y^2
$$
En la región definida por $2 \leq x \leq 4$ , $10 \leq y \leq 12$ .

**Solución:**

Elegimos como puntos iniciales $\overline{x} = 3$ y $\overline{y} = 11$ sobre los cuales se hará la linealización y definimos:
$$
z = f(x,y) = x^2 + 8xy + 3y^2
$$
Como se trata de una función de dos variables utilizaremos la siguiente ecuación para realizar la linealización:
$$
z - \overline{z} = K_{1} (x - \overline{x}) + K_{2} (y - \overline{y})
$$
donde: 
$$
K_{1} = \frac{\partial f}{ \partial x} \Biggl|_{x = \overline{x}, y = \overline{y} } \qquad

K_{2} = \frac{\partial f}{ \partial y} \Biggl|_{x = \overline{x}, y = \overline{y} } \qquad 

\overline{z} = f(\overline{x},\overline{y})
$$
entonces:
$$
K_{1} =  2x + 8y \,\, \Biggl|_{x = \overline{x}, y = \overline{y} } = 94  
\qquad \qquad  
K_{2} = 8x + 6y \,\, \Biggl|_{x = \overline{x}, y = \overline{y} } = 90
$$

$$
z - 636 = 94(x - 3) + 90(y-11)
$$
simplificando:
$$
\boxed{ z = 94x + 90y - 636}
$$

````matlab
clear;
close all;
clc;

% Calcular coeficientes
x0 = 3;
y0 = 11;
k1 = 2*x0 + 8*y0;
k2 = 8*x0 + 6*y0;
z0 = x0^(2) + 8*x0*y0 + 3*y0^(2);

syms x y z;
eq = z - z0 == k1*(x - x0) + k2*(y - y0);
solve(eq)
````

**Código:** Calcular coeficientes para linealización de dos variables.



**B-2-14** Encuentre para una ecuación linealizada para
$$
y = 0.2 x^3
$$
alrededor de un punto $x = 2$.

**Solución:**

Vamos a linealizar la ecuación alrededor del punto $\overline{x} = 2$, utilizando las siguientes ecuaciones:
$$
y - \overline{y} = K (x - \overline{x}) \quad \text{donde} \quad K = \frac{df}{dx} \Biggl|_{x = \overline{x}} \quad \text{,} \quad \overline{y} = f(\overline{x}) \quad \text{y} \quad f(x) = 0.2 x^3
$$
entonces:
$$
K = 0.6 x^{2} \Biggl|_{x = \overline{x}} = 2.4 \qquad
\overline{y} = f(2) = 1.6
$$
de modo que:
$$
y - 1.6 = 2.4(x - 2)
$$
simplificando:
$$
\boxed{y = 2.4x - 3.2}
$$

````matlab
clear;
close all;
clc;

format short
x0 = 2;
k = 0.6*x0^2;
y0 = 0.2*x0^(3);
syms y x;
eq =  y - y0 == k*(x - x0);
vpa(solve(eq,y))

%% Comprobación
paso = 1e-3;
x = 1.5:paso:2.5;
y1 = 0.2.*x.^(3);
y2 = 2.4*x - 3.2;
plot(x, y1,'DisplayName', ' Normal: $y_{1} = 0.2 x^{3}$');
hold on;
plot(x, y2,'DisplayName', 'Linealizada: $y_{2} = 2.4x - 3.2$');
grid on;
grid minor;
legend('Interpreter','latex','Location','northeast');
title('Linealizaci\''on','Interpreter','latex','FontSize',14);
xlabel('$x$','Interpreter','latex','Color','black','FontSize',12);
ylabel('$f(x)$','Interpreter','latex','Color','black','FontSize',12);
set(gca,'TickLabelInterpreter','latex', 'FontSize', 12);
````

**Código:** Linealizar para una sola variable.

![linealizacion](img\2.linealizacion.svg)

**Figura:** Visualización gráfica de linealización sobre el punto $x = 2$.

## Apéndice funciones de MATLAB

````matlab
function R = sym2tf(sys)
%   Converts a symbolic equation to a transfer function object
%
%   INPUT: symbolic function with 's' as the symbolic variable
%   OUTPUT: transfer function object
%   EXAMPLE: 
%        syms s;
%        sys = s/(s^2 + 5*s + 7);
%        R = sym2tf(sys)
%        R =
% 
%                    s
%               -------------
%               s^2 + 5 s + 7

    probe = isa(sys,'sym');
    if ~probe
          msg = 'Error occurred. \nArgument must be a symbolic object not a %s.';
          error(msg,class(sys));
    else
        [symNum,symDen] = numden(sys); 	% Obtener numerados y denominador
        TFnum = sym2poly(symNum);    	% Convertir Symbolic num to polynomial
        TFden = sym2poly(symDen);    	% Convertit Symbolic den to polynomial
        R = tf(TFnum,TFden);			% Generar funcion de transferencia
    end
end
````

**Código:** Convertir función simbólica a función de transferencia MATLAB.



````matlab
function R = tf2sym(sys)
%   Converts a transfer function object to a symbolic fuction
%
%   INPUT: transfer function object
%   OUTPUT: symbolic function with 's' as the symbolic variable
%   EXAMPLE: 
%        sys = tf([1 2],[1 2 4])
%        R = tf2sym(sys)
%        R =
%
%            (s + 2)/(s^2 + 2*s + 4)

    probe = isa(sys,'tf');
    if ~probe
          msg = 'Error occurred. \nArgument must be a transfer function object not a %s.';
          error(msg,class(sys));
    else
        [num,den] = tfdata(sys,'v');                % Extraer numerador y denominador
        syms s;                                     % Creacion de variable simbolica
        R = poly2sym(num,s)/poly2sym(den,s);        % Convertir en ec simbolica
    end
end
````

**Código:** Convertir función de transferencia a simbólico MATLAB. 



# Notas de clase

1. En términos prácticos un observador de estado es una copia de las variables de estado utilizando un algoritmo y un proceso de iteración aproximamos a las variables de estado por medio de observadores de estado.
2. Linealizamos utilizando la técnica de series de Taylor, también se puede hacer utilizando el Jacobiano.

* La matriz transferencia as racional si el sistema es lineal, estacionario y de dimensión 
* Las representaciones extremas asumen condiciones iniciales nulas.
*  Los sistemas de dimensión infinita no pueden describirse en EE. 
* Mediante linealización, puede describirse el comportamiento de un sistema no lineal en forma aproximada mediante un modelo en EE incremental lineal.
* La linealización se realiza alrededor de una trayectoria de operación nominal conocida, y los modelos incrementales obtenidos serán, en general, no estacionarios. 
* Los sistemas discretos tienen representaciones equivalentes a las de sistemas continuos mediante series convolución, funciones de transferencia en transformada Z, y EE diferencia.
* A diferencia del caso continuo, los retardos puros no necesariamente dan lugar a un sistema discreto distribuido. 