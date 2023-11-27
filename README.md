# Robotica_2023-2_Lab5
## Integrantes

- Wilfer Armando Fiquitiva Mendez.
- Johan Leonardo Castellanos Ruiz.
- Juan Pablo Cardenas Higuera.

## Cinemática 

Inicialmente se utilizará el análisis DHT del anterior laboratorio, donde se establece una posición inicial que denominaremos HOME.

![](https://github.com/jcardenash99/Robotica_2023-2_Lab5/blob/main/Cinematica%20directa%20pincher.png)

### Figura 1 Cinematica inversa del robot pincher.

![](https://github.com/jcardenash99/Robotica_2023-2_Lab5/blob/main/Tabla%20D_H%20Pincher.png)

### Tabla 1 parametros D_H robot pincher.

Teniendo estos parámetros de cinemática directa establecidos, podemos analizar la configuración.


## Cinemática Inversa
![Cinematica Inversa](https://github.com/jcardenash99/Robotica_2023-2_Lab5/assets/61796945/63825440-fd36-4e65-bbd4-3600af5e2ed4)
Para la cinematica inversa se van a establecer 4 angulos principales, por lo que partimos de las posiciones geometricas en la cual 4 de los 5 angulos parten de analisis geometricos, el 5 angulo depende de si el gripper se encuentra abierto o cerrado. Es decir que la posición en X (P<sub>x</sub>) y la posición en Y (P<sub>y</sub>) determinarán el angulo con la función mediante la función arcotangente

### Theta 1
Para el angulo $\theta_1$ partimos de las posciones de x e y del gripper, en esta imagen podemos ver que el angulo $\theta_1$ es el angulo que forma el triangulo con catetos X e Y e hipotenusa r
```math
\theta_1 = atan2(P_y,P_x)
```
### Theta 2 y Theta 3
$\theta_2$ y $\theta_3$ componen un triangulo el cual dependiendo de las configuraciones tenemos la distancia R y la altura Z
```math
D_z = P_z-L_4sin(\beta)-L_1
r = \sqrt{P_x^2+P_y^2}    
D_r = r-L_4*cos(\beta)
```
En este segmento thenemos el angulo $\beta$ es el angulo que tiene el gripper tambien conocido como angulo de ataque
 De estas definiciones podemos partir que los angulos $theta_2$ y $theta_3$ parten de las siguientes definiciones:

 ```math
     \theta_2=-2 atan((2 D_r L_2 - \sqrt{- D_r^4 - 2 D_r^2 D_z^2 + 2 D_r^2 L_2^2 + 2 D_r^2 L_3^2 - D_z^4 + 2 D_z^2 L_2^2 + 2 D_z^2 L_3^2 - L_2^4 + 2 L_2^2 L_3^2 - L_3^4})/(D_r^2 + D_z^2 + 2 D_z L_2 + L_2^2 - L_3^2))
```

```math
    \theta_3=-2atan((\sqrt{(- D_r^2 - D_z^2 + L_2^2 + 2 L_2 L_3 + L_3^2)(D_r^2 + D_z^2 - L_2^2 + 2L_2L_3 - L_3^2)} - 2L_2L_3)/(D_r^2 + D_z^2 - L_2^2 - L_3^2))
```
### Theta 4 
El angulo $\theta_4$ se puede encontrar sencllamente de la suma de los angulos $\beta$ $\theta_2$ y $\theta_3$.

```math
\theta_4=\beta-\theta_2-\theta_3
```
### Theta 5 
El angulo $\theta_5$ es el angulo de cierre del griper el cual salio de experimentacion, en este caso se obtuvieron los valores en radianes del sevo en el cual los valores de abierto y cerrado en radianes son:

Abierto: 1 rad

Cerrado: -1.2 rad

Con estos valores tenemos un amplio margen de apertura para permitir un error de posicion en la ubicaion del porta herrmienta, y una posicion de cerrado firme para mantener firme el marcador.
## rutinas y codigo:
Para asignar unas posiciones partimos del hecho de que medimos la posicion x y la posicion y del robot son positivas al frente y a la izquierda.
Las rutinas del laboratorio son:
• Cargar herramienta: El brazo se desplaza a la base porta-herramienta, sujeta el marcador y se ubica en
una posicion de espera.

• Espacio de trabajo: el brazo dibuja dos arcos que representan los limites de espacio de trabajo diestro
plano sobre la superficie, y regresa a una posici´on de espera.
• Dibujo de Iniciales: El brazo dibuja al menos dos letras, las iniciales de los nombres de los estudiantes,
sobre la superficie y retorna a una posici´on de espera.
• Dibujo de figura libre: Se dibuja sobre la superficie una figura libre que utilice segmentos rectos y curvos.
• Descarga de la herramienta: el brazo se desplaza a la base porta herramienta, suelta el marcador y se
ubica en una posicion de Home.

## Análisis y resultados.
A continucación se muestra las rutinas de movimiento del robot y las opciones de manipulacion por medio de la interfaz de usuario. 
Iniciamos con  la manipulación de la herramienta.

[Coger Herramienta lab 5.webm](https://github.com/jcardenash99/Robotica_2023-2_Lab5/assets/143892609/85e9ca6e-97e1-4593-9c51-1c5a9e062e0d)

Se observa como a partir del modo home se hace un desplazamiento al lugar donde se encuentra la herramienta con el griper abierto, una vez se posiciona para tomar la herramienta, cierra el griper y toma la herramienta desplazandose luego hacia el puento inicial de dnde dara inicio el trazo de trayectorias.

[Dibujar Espacio de Trabajo lab 5.webm](https://github.com/jcardenash99/Robotica_2023-2_Lab5/assets/143892609/6d9f83cd-f5ad-4125-a3a5-0ea201f04741)

Una vez posicionado en el punto incial de trayectorias se hace el trazado del espacio de trbajo en el cual va a delimitar el la zona de trabajo para este caso esta comprendida entre dos semi circulos.








## Conclusiones

