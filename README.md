# Sky130-PD-WorkShop



## Design Preparation Step

Para invocar la herramienta se ejecuta el comando

```python
docker
```

Luego, se ejecuta

```python
./flow.tcl -interactive 
```

Ahora le colocamos como input todos los paquetes que requieren este flujo

```python
package require openlane 0.9
```

<aside>
📌 La carpeta para encontrar los diseños hechos con openlane es openlane/designs. Vamos a trabajar en la carpeta picorv32a

</aside>

En esta carpeta vamos a encontrar los siguientes archivos

![Untitled](Get%20familiar%20to%20Open%20Source%20EDA%20tools%20272774a034a94b91aaf4dd0aea6c0aa0/Untitled%201.png)

La carpeta src es donde estarán los archivos de verilog de nuestro circuito

<aside>
📌 less [name] -> Para abrir un archivo de texto. Presionar la tecla Q para salir

</aside>

config.tcl bypasses any configurations that has been already done 

![Archivo config.tcl por dentro](Get%20familiar%20to%20Open%20Source%20EDA%20tools%20272774a034a94b91aaf4dd0aea6c0aa0/Untitled%202.png)

Archivo config.tcl por dentro

El otro archivo .tcl trata sobre las configuraciones y es el más prioritario. Podemos correr el disño sin el sky130….tcl pero NO sin el config.tcl

Ahora, necesitamos establecer el sistema de archivos de un diseño al flujo especificando una **ubicacion** en openlane

```python
prep -design [design_name]
```

Luego de hacer esto, se nos va a crear en la carpeta de ese diseño la carpeta runs

![carpeta runs](Get%20familiar%20to%20Open%20Source%20EDA%20tools%20272774a034a94b91aaf4dd0aea6c0aa0/Untitled%203.png)

carpeta runs

Por ahora todas las carpetas excepto la tmp estarán vacíos.

![Carpeta tmp](Get%20familiar%20to%20Open%20Source%20EDA%20tools%20272774a034a94b91aaf4dd0aea6c0aa0/Untitled%204.png)

Carpeta tmp

the marged.lef file contains the needed information about the .tech and .ref

Si entramos a synthesis/reports podemos ver que hay un archivo config.tcl. Este archivo tiene todos los parámetros por defecto que “is begin taken by the run”. Revisando este archivo se puede verificar que los cambios que haya hecho a los parámetros hayan sido correctamente reflejados

Ahora, para correr la SÍNTESIS y el ABC se ejecuta desde openlane el comando:

```python
run_synthesis
```

Al final del todo deberíamos obener el mensaje:

![Untitled](Get%20familiar%20to%20Open%20Source%20EDA%20tools%20272774a034a94b91aaf4dd0aea6c0aa0/Untitled%205.png)

# Steps to characterize synthesis results

En la síntesis se pueden ver ya los resultados de lo que se sintetizó, como por ejemplo el área que ocupa el chip:

![Untitled](Get%20familiar%20to%20Open%20Source%20EDA%20tools%20272774a034a94b91aaf4dd0aea6c0aa0/Untitled%206.png)

Como primera tarea, se debe encontrar el “flop ratio”, que es la porción de flip flops tipo D que hay respecto a el número total de celdas

En el reporte final de la síntesis, se pueden encontar la cantidad de flip flops que se están implementando:

![Untitled](Get%20familiar%20to%20Open%20Source%20EDA%20tools%20272774a034a94b91aaf4dd0aea6c0aa0/Untitled%207.png)

También podemos encontrar el número total de celdas usadas:

![Untitled](Get%20familiar%20to%20Open%20Source%20EDA%20tools%20272774a034a94b91aaf4dd0aea6c0aa0/Untitled%208.png)

Para hacer el cálculo del “flip flop ratio” usamos:

$$
ffRatio = \frac{\#ofFlipFlops}{\# ofCells}
$$

En este caso, hacemos 100* 1634/17323 = 9.432%

---

Una vez corrida la síntesis, podemos ir a los resultados de la síntesis en results/synthesis. ahí vamos a encontrar un archivo que se generó

![Untitled](Get%20familiar%20to%20Open%20Source%20EDA%20tools%20272774a034a94b91aaf4dd0aea6c0aa0/Untitled%209.png)

También, para la carpeta de reports, podemos ir a los resultados de la síntesis en reports/synthesis y encontrar todos los reportes:

![Untitled](Get%20familiar%20to%20Open%20Source%20EDA%20tools%20272774a034a94b91aaf4dd0aea6c0aa0/Untitled%2010.png)

El archivo subrayado contiene los resutlados finales del reporte, como el número de celdas, etc
