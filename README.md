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
```python
 if caso==1:
                if Cargado==0: 
                    print("Recogiendo herramienta")
                    start_time = time.time()

                    
                    Cal_pos(np.array([0.1,-0.2,0.25,0.17]),0.3,1)
                    Cal_pos(np.array([0.12,-0.21,0.125,0]),0.5,1)
                    
                    print("Presione enter para cerrar gripper")
                    input()
                    
                    print("Cerrando gripper")
                    Cal_pos(np.array([0.12,-0.21,0.152,0]),0.5,gripClose)
                    print("Regresando a home")
                    Cal_pos(np.array([0.1,-0.2,0.25,0.17]),1,-1.22)
                    Cal_pos(np.array([0.2,0,0.242,0]),1,-1.22)
                    
                    end_time = time.time()
                    Tiempo=end_time-start_time
                    print("\ntiempo: %.2f s" % Tiempo)
                    Cargado=1
                else: 
                    print("Herramienta en gripper")
                
```


• Espacio de trabajo: el brazo dibuja dos arcos que representan los limites de espacio de trabajo diestro
plano sobre la superficie, y regresa a una posici´on de espera.

```Python
elif caso==2:
                if Cargado==1:
                    print("Dibujando espacio de trabajo")
                    start_time = time.time()
                    #Vector de puntos de los arcos
                    ArrPos=[PosArcMinIzq[0,:],PosArcMinIzq[1,:],PosArcMinDer[0,:],PosArcMinDer[1,:],PosArcMaxDer[0,:],PosArcMaxDer[1,:],PosArcMaxIzq[0,:],PosArcMaxIzq[1,:],PosArcMinIzq[0,:],np.array([0.2,0,0.242,0])]
                    ArrTiempo=[1.5,0.5,0.5,0.5,1,2,1.5,2,0.5,1]
                    for i in range(len(ArrPos)):
                        Cal_pos(ArrPos[i],ArrTiempo[i],-1.22)
                    #Finaliza volviendo al home
                    Cal_pos(np.array([0.2,0,0.242,0]),1.5,-1.22)

                    end_time = time.time()
                    Tiempo=end_time-start_time
                    print("\ntiempo %.2f s" % Tiempo)
                else: 
                    print("La herramienta no esta en posicion")
```

• Dibujo de Iniciales: El brazo dibuja al menos dos letras, las iniciales de los nombres de los estudiantes,
sobre la superficie y retorna a una posici´on de espera.
```Python
elif caso==3:
                if Cargado==1:
                    print("Dibujando iniciales JWL")
                    start_time = time.time()
                    #Se realizan rutinas para la "A" y la "J"
                    ArrTiempo=[1.5,1.5,1.5,0.5,0.5,0.5,1,1,1.5,0.5,0.5,0.5,0.5,1,1]
                    for i in range(7):
                        Cal_pos(PuntosJ[i,:],ArrTiempo[i],-1.22)
                    time.sleep(1)
                    for i in range(7):
                        Cal_pos(PuntosW[i,:],ArrTiempo[i],-1.22)
                    time.sleep(1)

                    for i in range(7):
                        Cal_pos(PuntosL[i,:],ArrTiempo[i],-1.22)
                    Cal_pos(np.array([0.2,0,0.242,0]),1.5,-1.22)
                    end_time = time.time()
                    Tiempo=end_time-start_time
                    print("\ntiempo %.2f s" % Tiempo)
                else: 
                    print("La herramienta no esta en posicion")

```



• Dibujo de figura libre: Se dibuja sobre la superficie una figura libre que utilice segmentos rectos y curvos.
```Python
elif caso==4:
                if Cargado==1:
                    print("Dibujando")
                    start_time = time.time()
                    #Dibujo de un triangulo

                    ArrTiempo=[1.5,1.5,1.5,1.5,1.5]
                    for i in range(5):
                        Cal_pos(PuntosTri[i,:],ArrTiempo[i],-1.22)
                    #Dibujo de un circulo 
                    N=50
                    Rads=np.linspace(0,2*np.pi,N)
                    ZPuntos=np.ones(N)*(0.138)
                    BetaPuntos=np.ones(N)*0.17
                    t=0.2
                    ArrTiempo=np.ones(N)*t
                    #Arreglo de puntos del circulo
                    PuntosCirc=np.zeros([N,4])
                    PuntosCirc[:,0]=0.225+0.025*np.cos(Rads)
                    PuntosCirc[:,1]=-0.025+0.025*np.sin(Rads)
                    PuntosCirc[:,2]=ZPuntos-0.01+0.005*np.cos(Rads)
                    PuntosCirc[:,3]=BetaPuntos

                    #Se establece una posición de inicio antes de empezar el círculo
                    PosIni=[0.25,0,0.24,0.17]

                    #Se dirige a la posición de inicio
                    Cal_pos(PosIni,1.5,-1.22)
                    
                    #Rutína de movimiento del círculo
                    for i in range(N):
                        Cal_pos(PuntosCirc[i,:],ArrTiempo[i],-1.22)
                    
                    #Vuelve a Home
                    Cal_pos(np.array([0.2,0,0.242,0]),2,-1.22)
                    

                    end_time = time.time()  
                    Tiempo=end_time-start_time
                    print("\ntiempo: %.2f s" % Tiempo)
                else: 
                    print("La herramienta no esta en posicion")
```

• Descarga de la herramienta: el brazo se desplaza a la base porta herramienta, suelta el marcador y se
ubica en una posicion de Home.


```Python
elif caso==5:
                if Cargado==1:
                    print("Se seleccionó: Descarga de la herramienta")
                    start_time = time.time()
                    #Rutina para descargar la herramienta. Solo la realiza si está cargada.
                    Cal_pos(np.array([0.1,-0.2,0.25,0.17]),0.3,-1.22)
                    Cal_pos(np.array([0.1,-0.185,0.152,0.17]),0.5,-1.22)
                    input()
                    print("Apretar gripper")
                    Cal_pos(np.array([0.1,-0.185,0.152,0.17]),0.5,1)
                    print("Volviendo a home")
                    Cal_pos(np.array([0.1,-0.2,0.25,0.17]),1,1)
                    Cal_pos(np.array([0.2,0,0.242,0]),1,1)

                    end_time = time.time()
                    Tiempo=end_time-start_time
                    print("\ntiempo: %.2f s" % Tiempo)
                    Cargado=0
                else: 
                    print("Ya se retiro la herramienta")
                
            elif caso==6:
                print("Se seleccionó: Salir")
            
            else:
                print("Opción no válida. Saliendo.")
                break
```

Estos codigos son los codigos de desplazamiento basico, el caculo del vector de trayectorias se encuentra en variables de arreglos ue ubican en posicion cada punto de las letras o del triangulo, el calculo del circulo se realiza en la propia rutina.

Para los calculos de los puntos se requiere un vector de tiempo, esto con la intencion de no saturar el bueffer de datos enviados y permitirle al robot llegar a trazar estos puntos.

Sumado a esto tenemos los codigos de publish del nodo de ros usados en los laboratorios anteriores, tambien se tienen los calculos de la cinematica directa usada en el anterior laboratorio.

## Análisis y resultados.
A continuación se muestra las rutinas de movimiento del robot y las opciones de manipulacion por medio de la interfaz de usuario. 
Iniciamos con  la manipulación de la herramienta.

[Coger Herramienta lab 5.webm](https://github.com/jcardenash99/Robotica_2023-2_Lab5/assets/143892609/85e9ca6e-97e1-4593-9c51-1c5a9e062e0d)

Se observa como a partir del modo home se hace un desplazamiento al lugar donde se encuentra la herramienta con el griper abierto, una vez se posiciona para tomar la herramienta, cierra el griper y toma la herramienta desplazandose luego hacia el puento inicial de dnde dara inicio el trazo de trayectorias.

[Dibujar Espacio de Trabajo lab 5.webm](https://github.com/jcardenash99/Robotica_2023-2_Lab5/assets/143892609/6d9f83cd-f5ad-4125-a3a5-0ea201f04741)

Una vez posicionado en el punto incial de trayectorias se hace el trazado del espacio de trbajo en el cual va a delimitar el la zona de trabajo para este caso esta comprendida entre dos semi circulos.

[Dibujo de Iniciales JWL lab 5 ‐ Hecho con Clipchamp.webm](https://github.com/jcardenash99/Robotica_2023-2_Lab5/assets/143892609/1387cadb-8ec2-4631-834b-21b2a4fdf5a0)

Una vez delimitada el área de trabajo dentro de ella se hace el grabado de las inciaales de cada uno de los integrantes del grupo JWL.

[Dibujo de Figuras lab 5 ‐ Hecho con Clipchamp.webm](https://github.com/jcardenash99/Robotica_2023-2_Lab5/assets/143892609/e92ad0cd-7f38-41a9-8215-a11f6b39b877)

Seguido a las inciciales se dibujan dos figuras geometricas un triangulo y un circulo como complemento a las trayectorias.

[Regreso de herramienta lab 5.webm](https://github.com/jcardenash99/Robotica_2023-2_Lab5/assets/143892609/51dc7939-00cd-429c-8db0-575ab02d7a50)

Finalmente al terminar las trayectorias, el robot retorna el marcador a la posición deonde lo cargo, abre el griper y retorna a su posición de home, terminando con ello la rutina.

Como resultado de la programación y la ejecución del programa mediante el robot se evidencia que en el proceso hubo algunos movimientos bruscos en determinados puntos del trazado de las trayectorias. Esto se debe a que los controles de movimiento de los servomotores se deben hacer siguiendo perfiles de velocidad que permitan mantener el control tanto en aceleración, crucero y desaceleración, con el fin de que las trayectorias sean más estilizadas, sin embargo, cabe resaltar que la definición de las iniciales y las figuras geométricas son totalmente legibles cumpliendo con el objetivo de trazar las trayectorias siendo este totalmente funcional.

## Conclusiones.

En la generación de trayectorias se pudo evidenciar que tanto la programación y la parametrización van de la mano basados en la morfología y actuadores que componen el robot siendo estos primordiales para obtener un buen resultado en la programación de robots. La cinemática inversa se vuelve esencial, ya que permite determinar las configuraciones de las articulaciones necesarias para alcanzar una posición y orientación específicas del extremo efectivo del manipulador. La implementación de la cinemática inversa en ROS implica conocer los parámetros geométricos y cinemáticos del robot y realizar cálculos para traducir los movimientos deseados del extremo efectivo a las variables de las articulaciones. La capacidad de llevar a cabo estos cálculos dentro del entorno modular de ROS facilita el diseño y ejecución eficiente de tareas de manipulación. Es esencial considerar limitaciones físicas, como límites de articulación y singularidades, así como realizar pruebas y validaciones en entornos simulados antes de implementar los movimientos en el hardware físico.
