## Ejemplos_hadoop
Hola, bienenido a este repositorio.

Aquí encontrarás ejemplos y comandos para utilizar **docker-hadoop**.

Los créditos correspondientes al repositorio son: [github/docker-hadoop](https://github.com/big-data-europe/docker-hadoop)

El propósito de estos ejercicios son que el usuario conozca los comandos de docker-hadoop utilizando el modelo de MapReduce con archivos de texto. Así teniendo libertad de experimentar con sus propios archivos.

## Inciar el contenedor

Se debe abrir una terminal en la carpeta **docker-hadoop** y ejecutar el siguiente comando: `docker-compose up -d`
Este comando iniciará los cinco contenedores requeridos.

Para comprobar si los contenedores están ejecutándose, ponemos: `docker ps`.
Ahora entraremos al nodo maestro (`namenode`) con el comando: `docker exec -it namenode bash` esto quiere decir que accedemos a nuestro contenedor `namenode`.
Esto nos permite administrar nuestro sistema de archivos HDFS.

Para apagar los contenedores. Abrir docker desktop y dar click en el botón **stop** de dicho contenedor.
## MapReduce de WordCount
MapRuduce es un modelo de programación que permite procesar grandes volúmnes de datos.

En este ejercicio se descargará un archivo de texto para contar las palabras.

Usaremos un archivo .jar que tiene las clases necesarias para ejecutar el MapReduce.
Para descargar el archivo nos dirigimos a 
* [hadoop-mapreduce](https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-mapreduce-examples/2.7.1/)
* Se descarga el archivo con nombre `hadoop-mapreduce-examples-2.7.1-sources.jar`. Ese archivo se debe guardar en la misma carpeta de docker-hadoop.

Para el archivo de texto utilizaremos el archivo llaneros (mismo que se encuentra en este GitHub), guardándolo en la misma carpeta de docker-hadoop.

**Regresamos a nuestra terminal, ejecutando el siguiente comando:**
* `docker cp hadoop-mapreduce-examples-2.7.1-sources.jar namenode:/tmp`

Lo que hace es movernos el archivo a un contenedor temporal.

**Haremos lo mismo para el archivo de texto:**
* `docker cp llaneros.txt namenode:/tmp`

**Ingresamos al contenedor** `namenode`: 
* `docker exec -it namenode bash`

**Creamos la carpeta**:
* `hdfs dfs -mkdir /user/root/input_contador`

**Entramos al contenedor temporal utilizando el comando**

* `cd /tmp`

**El siguiente comando especifica el directorio de entrada**: 

* `hdfs dfs -put llaneros.txt /user/root/input_contador`

Si usas un nuevo archivo y no tiene la terminación .txt se pone el nombre tal cual

**Ahora ejecutamos MapReduce**

En una sola línea de comando:

`hadoop jar hadoop-mapreduce-examples-2.7.1-sources.jar org.apache.hadoop.examples.WordCount input_contador output_contador`

**Para ver tus resultados**

Ponemos el comando:

* `hdfs dfs -cat /user/root/output_contador/*`

Para comprobar los resultados:

* `hdfs dfs -ls /user/root/output_contador`

De nuestro resultados lo que nos interesa es el archivo `part-r-00000`, que contiene nuestro recuento de palabras.

Para lo anterior ponemos:

Esta línea lo que hará será guardar nuestro conteo de palabras en un nuevo archivo llamado llaneros_wx.txt

* `hdfs dfs -cat /user/root/output_contador/part-r-00000 > /tmp/llaneros_wc.txt`

Desupés ejecutamos:

Este comando lo que hará será salirnos del nodo maestro (namenode)

* `exit`

Por útimo:

Este comando lo que hará es guardarnos un archivo nuevo llamado _llaneros_wc.txt_ con el resultado de conteno de palabras.

* `docker cp namenode:/tmp/llaneros_wc.txt .`

## Ordenar números de Menor a Mayor (Temperaturas)

Descargamos un nuevo archivo el cual contiene varias temperaturas que registraron a lo largo de varios años como texto plano.

**Se utilizará el mismo archivo .jar del ejemplo pasado.**

* [Maximum_Temp_Calculation_mapreduce](https://github.com/Rkrahul04/Maximum_Temp_Calculation_mapreduce/blob/master/Dataset%20-%20Calculate%20Maximum%20Temperature/Temperature). Descargamos el archivo y se descomprime.

* Teniendo el archivo de `Temperature.txt` se mueve a la misma carpeta donde está docker-hadoop.

Como ya contamos con `docker cp hadoop-mapreduce-examples-2.7.1-sources.jar namenode:/tmp` podemos omitir hacer ese paso. 

Lo que hace es movernos el archivo a un contenedor temporal.

Con el nuevo archivo de `Temperature.txt` ejecutamos lo siguiente:

* `docker cp Temperature.txt namenode:/tmp`

**Ingresamos al contenedor** `namenode`: 
* `docker exec -it namenode bash`

**Creamos la carpeta**:
* `hdfs dfs -mkdir /user/root/input_temperaturas`

**Entramos al contenedor temporal utilizando el comando**

* `cd /tmp`

**El siguiente comando especifica el directorio de entrada**: 

* `hdfs dfs -put Temperature /user/root/input_temperaturas`

**Nota**: Para este ejercicio si se usa el archivo del repositorio se pone el nombre del archivo tal cual, es decir, sin la extensión .txt

**Ahora ejecutamos MapReduce**

En una sola línea de comando:

`hadoop jar hadoop-mapreduce-examples-2.7.1-sources.jar org.apache.hadoop.examples.WordCount input_contador output_contador`

**Para ver tus resultados**

Ponemos el comando:

* `hdfs dfs -cat /user/root/output_temperaturas/*`

Para comprobar los resultados:

* `hdfs dfs -ls /user/root/output_temperaturas`

De nuestro resultados lo que nos interesa es el archivo `part-r-00000`, que contiene nuestro recuento de palabras.

Para lo anterior ponemos:

Esta línea lo que hará será guardar nuestro conteo de palabras en un nuevo archivo llamado temperaturas_ordenadas.txt

* `hdfs dfs -cat /user/root/output_temperaturas/part-r-00000 > /tmp/temperaturas_ordenadas.txt`

**Desupés ejecutamos:**

Este comando lo que hará será salirnos del nodo maestro (namenode)

* `exit`

Por útimo:

Este comando lo que hará es guardarnos un archivo nuevo llamado _llaneros_wc.txt_ con el resultado de conteno de palabras.

* `docker cp namenode:/tmp/temperaturas_ordenadas.txt .`

## Sudoku
Resolveremos sudokus contenidos en un archivo de texto.

Usaremos un archivo .jar que tiene las clases necesarias para ejecutar el MapReduce.

Para descargar el archivo nos dirigimos a 
* Se descarga el archivo con nombre `hadoop-examples-0.20.205.0.jar`.

Ese archivo se debe guardar en la misma carpeta de docker-hadoop.

Se descarga el siguiente archivo:

* [puzzle1.dta](https://2935835229-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVMoUbnSkOwuxDBZq5HyK%2Fuploads%2FJAygXsvKPKvUwThTxh7f%2Fpuzzle1.dta?alt=media&token=8291160e-3730-4125-b34d-12517d084419)

Teniendo el archivo de `puzzle1.dta` se mueve a la misma carpeta donde está docker-hadoop.

**Regresamos a nuestra terminal, ejecutando el siguiente comando:**
* `docker cp hadoop-examples-0.20.205.0.jar namenode:/tmp`

Lo que hace es movernos el archivo a un contenedor temporal.

**Haremos lo mismo para el archivo de texto:**
* `docker cp puzzle1.dta namenode:/tmp`

**Ingresamos al contenedor** `namenode`: 
* `docker exec -it namenode bash`

**Creamos la carpeta**:
* `hdfs dfs -mkdir /user/root/input_puzzle1.dta`

**Entramos al contenedor temporal utilizando el comando**

* `cd /tmp`

**El siguiente comando especifica el directorio de entrada**: 

* `hadoop jar hadoop-examples-0.20.205.0.jar sudoku puzzle1.dta`

**Para ver tus resultados**

Ponemos el comando:

* `hadoop jar hadoop-examples-0.20.205.jar sudoku puzzle1.dta > solucion_puzzle1.txt`

Desupés ejecutamos:

Este comando lo que hará será salirnos del nodo maestro (namenode)

* `exit`

Por útimo:

* `docker cp namenode:/tmp/solucion_puzzle1.txt .`

