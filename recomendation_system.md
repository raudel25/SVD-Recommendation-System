# Sistemas de Recomendación

Desde el nacimiento de internet como tecnología se han abierto nuevas posibilidades que facilitan el acceso a la información, también esta se ha aumentado considerablemente. Tal impacto se ha visto reflejado en todo tipo de áreas, una de ellas es el comercio, donde sitios especializados en internet ofertan millones de productos o servicios, también hay sitios donde la 
mayoría de datos pueden ser efímeros o de poca utilidad, produciendo un caos de información que, al contrario de satisfacer requerimientos de los usuarios, pueden complicar más su respuesta; para que sea efectiva, depende de la habilidad y experiencia que tenga el usuario para expresar sus necesidades mediante una consulta y está demostrado que en la mayoría de casos es imprecisa y vaga. Por lo anterior surgen los sistemas de recuperación de información, que si bien apuntan a mejorar la forma en que se almacena, representa y organiza la información, su función no es devolver la información deseada por el usuario sino únicamente indicar qué documentos son potencialmente relevantes para dicha necesidad de información. A partir de esto, surge una nueva necesidad de encontrar herramientas que se encarguen de buscar por los usuarios, que los conozcan y recomienden un conjunto limitado de opciones acordes a sus intereses. 

Un sistema de recomendación se puede definir, de manera formal, como aquel sistema que tiene como principal tarea seleccionar ciertos objetos de acuerdo con los requerimientos del usuario. Estos sistemas son muy atractivos en situaciones donde la cantidad de información que se ofrece al usuario supera ampliamente cualquier capacidad individual de exploración. Para ello consideran tres fases principales que se mencionan a continuación :

* Captura de preferencias: las preferencias reflejan los gustos e intereses del usuario, estas serán adquiridas a partir de la interacción del usuario con el sistema. 
* Extracción del conocimiento: en esta fase el sistema se encarga de interpretar la información que recopiló en la fase anterior para poder predecir gustos y preferencias del usuario.
* Recomendación: a partir del conocimiento adquirido en la fase de extracción del conocimiento, el sistema ya tiene la capacidad de seleccionar los ítems que podrían interesarle al usuario.

En la mayoría de los casos estos datos recogidos se almacenan en forme de matrices, siendo estas a veces muy grandes y dispersas, por esto las técnicas de reducción de la dimensionalidad tienen como objetivo transformar la matriz en una de menores dimensiones que refleje las características de la matriz original a la hora de realizar la predicción. Esto permite acelerar el proceso de 
recomendación y minimiza los problemas relacionados con la dispersión de la matriz y la presencia de datos erróneos. El enfoque de este trabajo es usar las técnicas de reducción de dimensionalidad de SVD, el cual estaremos viendo a continuación.

# SVD (Singular Value Descomposition)

La descomposición singular de valores es una 
técnica de factorización de matrices que permite 
descomponer una matriz de dimensión nxm A en otras matrices U,
S, V tales que :

$$ 
A = U \cdot \Sigma \cdot  V^T  
$$

Donde :
* *U* de tamaño (n, n) es una matriz ortogonal que contiene los vectores singulares izquierdos de *A*.
* En $\Sigma$ que es una matriz diagonal (n,d), cuyos valores son los valores singulares de la matriz *A* ordenados en valor decreciente
* En *V* que es una matriz transpuesta (d,d), cuyos valores son los vectores singulares derechos de *A*.

*Ortogonal significa que multiplicando la transpuesta por si misma, se obtiene la matriz identidad*

Existe una propiedad 
aplicada a SVD enfocados en los sistemas 
de recomendación, esta consiste en que 
reduciendo el número de valores singulares 
de la matriz $\Sigma$ a los primeros k valores, se 
obtendrá una aproximación de la matriz 
original A, que permite ser reconstruida a 
partir de las versiones reducidas de las otras 
matrices cometiendo un cierto error, pero 
disminuyendo el tamaño. Es decir: 

$$ 
A_{n, m} \approx U_{n, k} \cdot \Sigma_{k, k} \cdot V^T_{k, m}  
$$

Esta propiedad es derivada del 
teorema de Eckart-Young que aborda 
la mejor aproximación a la matriz original 
A, obteniéndola poniendo a 0 los n valores 
singulares más pequeños, así se reducirán 
las matrices al número de valores singulares 
no nulos que tenga la matriz $\Sigma$. Esto resulta 
entonces en la transformación de gran cantidad 
de datos en su representación reducida, siendo 
por lo tanto una propiedad muy importante 
que permite reducir considerablemente el 
tiempo de cómputo de cálculo y de uso de 
memoria para las tres matrices.

# Filtrado Colaborativo

El filtrado colaborativo es un método para hacer predicciones automáticas (filtrado) sobre los intereses de un usuario mediante la recopilación de las preferencias o gustos de información de muchos usuarios (colaborador). El Filtrado colaborativo se basa, en que si una persona A tiene la misma opinión que una persona B sobre un tema, A es más probable que tenga la misma opinión que B en otro tema diferente que la opinión que tendría una persona elegida azar. 

Dentro del filtrado colaborativo, es necesario el 
manejo de la matriz de votaciones de usuarios 
e ítems, por lo tanto, es posible considerar esta 
matriz como la base para aplicar SVD y obtener la 
factorización de matrices U, S y V. Luego, usando 
el teorema de Eckar-Youn, se puede reducir a k
dimensiones. La matriz de factorización sería:

$$ 
A_{usuarios, ítems} \approx U_{usuarios, k} \cdot \Sigma_{k, k} \cdot V^T_{k, ítems}
$$

Por otro lado, es posible simplificar 
el proceso de SVD obteniendo solo los 
factores de usuarios e ítems, esto es posible 
descomponiendo la matriz $\Sigma$ en dos matrices 
iguales, factores de usuarios *Ufac* y factores de 
ítems *Ifac* de esta manera:

$$
\Sigma_{k,k} = 
\left(\begin{array}{}
\lambda_{1} &  & ...\\
& \lambda_{2}\\
... \\
& & \lambda_{k}
\end{array}\right)
$$

$$
\Sigma_{k,k} = 
\left(\begin{array}{}
\sqrt{\lambda_{1}} &  & ...\\
& \sqrt{\lambda_{2}}\\
... \\
& & \sqrt{\lambda_{k}}
\end{array}\right) 
\cdot 
\left(\begin{array}{}
\sqrt{\lambda_{1}} &  & ...\\
& \sqrt{\lambda_{2}}\\
... \\
& & \sqrt{\lambda_{k}}
\end{array}\right)
$$

Nota : Los $\lambda_{i}$ son valores singulares de A.

Por tanto la matriz de votaciones se puede expresar como *Votos* = *Ufact* $\cdot$ *Ifact*, donde :

$$
Ufact = U_{n, k} \cdot
\left(\begin{array}{}
\sqrt{\lambda_{1}} &  & ...\\
& \sqrt{\lambda_{2}}\\
... \\
& & \sqrt{\lambda_{k}}
\end{array}\right)
$$


$$
Ifact =
\left(\begin{array}{}
\sqrt{\lambda_{1}} &  & ...\\
& \sqrt{\lambda_{2}}\\
... \\
& & \sqrt{\lambda_{k}}
\end{array}\right)
\cdot
V^T_{k, m}  
$$

Para hallar esta aproximacón, la idea es buscar los 
factores de usuarios e ítems a partir de la matriz 
de votación abordándolo como un problema de 
regresión, donde se quieren encontrar los valores 
de las matrices de factores. Así, si se 
considera a $q_{i}$ al vector que represente los 
factores de un ítem y $p_{j}$ al vector que representa 
los factores del usuario se tendría que cumplir :

$$
A_{i, j} = q^T_{i} \cdot p_{j} \approx \sum_{n = 1}^{k} q_{i, n}\cdot p_{n, j}
$$

Ahora solo se ajustan los factores de 
aquellos usuarios que hayan definido un voto 
reduciendo considerablemente el sistema de 
ecuaciones y solucionando el problema de la 
dispersión, cometiendo un pequeño error $e_{i, j}$ = |$A_{i, j}$ - $q^T_{i} \cdot p_{j}$|. Debido a que el error absoluto es una 
función complicada de tratar, se usará el error 
cuadrado medio:

$$
(e_{i, j})^2 = (A_{i, j} - q^T_{i} \cdot p_{j})^2 \approx (A_{i, j} - \sum q_{i, n}\cdot p_{n, j})^2
$$

O sea, intentar encontrar el error mínimo para hacer la mejor aproximacón de la siguiente forma:

$$
min_{p, q} = \sum (A_{i, j} - q_{i, n}\cdot p_{n, j})^2
$$

Pero es necesario además hacer una regularización para evitar el *Outfitting*, así, es posible llegar a:

$$
(e_{i, j})^2 = (A_{i, j} - \sum q_{i, n}\cdot p_{n, j})^2 + \frac{\lambda}{2} \cdot (||q_{i}||^2 + ||p_{j}||^2) \space (1)
$$

Donde $\lambda$ es una constante de regularización.

Para encontrar el mínimo de la función ya detallada 
existen técnicas como la del descenso del gradiente que 
buscan ajustar las matrices de factores poco a 
poco de manera automática. Esto se realizará en la 
función (1), con el fin de encontrar el mínimo de 
la función, o acercarse lo suficiente a la solución. Esta técnica consiste en ir 
moviéndose poco a poco por la función hasta encontrar el mínimo evaluando, la función en un punto dado para 
luego moverse una distancia hacia otro punto; 
ahora se calcula la derivada en ese punto y 
se desplaza al lado opuesto de la derivada de 
la función que se quiere minimizar.

Aplicando descenso del gradiente en (1) se tiene:

$$
\frac{\delta(e_{i, j})^2}{\delta p_{i, n}} = -2q_{n, j}\cdot (A_{i, j} - \sum q_{i, n}\cdot p_{n, j}) + \lambda p_{i, n} \space (2)
$$

$$
\frac{\delta(e_{i, j})^2}{\delta q_{n, j}} = -2p_{i, n}\cdot (A_{i, j} - \sum q_{i, n}\cdot p_{n, j}) + \lambda q_{n, j} \space (3)
$$

Teniendo ya el gradiente, solo queda 
aplicar las reglas de actualización tanto para $p_{i, n}$
como para $q_{n, j}$ en (2), (3) con las constantes de 
regularización y aprendizaje. Así, la regla de 
actualización parte de $\sigma_{n + 1} = \sigma_{n} - \alpha \nabla f(x)$, donde $\sigma_{n + 1}$ es el nuevo valor en la matriz de votaciones, $\sigma_{n}$ su valor actual, $\alpha$ constante de aprendizaje y $\nabla f(x)$ es el gradiente ya obtenido. Dando como resultado :

Para $p_{i, n}$ :

$$
p_{i, n}^{'} = p_{i, n} + \alpha[2q_{n, j}\cdot (A_{i, j} - \sum q_{i, n}\cdot p_{n, j}) - \lambda p_{i, n}] \space (4)
$$

Para $p_{n, j}$ :

$$
q_{n, j}^{'} = q_{n, j} + \alpha[2p_{i, n}\cdot (A_{i, j} - \sum q_{i, n}\cdot p_{n, j}) - \lambda q_{n, j}] \space (5)
$$

El enfoque se plasma en las expresiones (4) y 
(5) que serán las que actualizarán los valores de 
la matriz de votaciones a partir de cada *Ephochs*, 
tanto para los factores de usuarios, como para los 
factores de ítems.
