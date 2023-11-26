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

Se van a establecer 4 angulos &theta;
&theta;<sub>1</sub> dependerá uncamente de la posición angular de la primera articulación, la cual se encontrará sobre el plano X e Y. Es decir que la posición en X (P<sub>x</sub>) y la posición en Y (P<sub>y</sub>) determinarán el angulo con la función mediante la función arcotangente, sabiendo que

\(\tan(&theta;)\=\frac{P_y}{P_x}\)