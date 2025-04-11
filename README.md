## Ejemplos_hadoop
Hola, bienenido a este repositorio.

Aquí encontrarás ejemplos y comandos para utilizar **docker-hadoop**.

Los créditos correspondientes son para: [github/docker-hadoop](https://github.com/big-data-europe/docker-hadoop)

## Inciar el contenedor
Se debe abrir una terminal en la carpeta **docker-hadoop** y ejecutar el siguiente comando: `docker-compose up -d`
Este comando iniciará los cinco contenedores requeridos.

Para comprobar si los contenedores están ejecutándose, ponemos: `docker ps`.
Ahora entraremos al nodo maestro (`namenode`) con el comando: `docker exec -it namenode bash` esto quiere decir que accedemos a nuestro contenedor `namenode`.
Esto nos permite administrar nuestro sistema de archivos HDFS.

Para apagar los contenedores. Abrir docker desktop y dar click en el botón **stop** de dicho contenedor.
## MapReduce de WordCount
En este ejercicio se descargará un archivo de texto para contar las palabras.

Usaremos un archivo .jar que tiene las clases necesarias para ejecutar el MapReduce.
Para descargar el archivo nos dirigimos a 
* [hadoop-mapreduce](https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-mapreduce-examples/2.7.1/)
* Se descarga el archivo con nombre `hadoop-mapreduce-examples-2.7.1-sources.jar`
Ese archivo se debe guardar en la misma carpeta de docker-hadoop.

Para el archivo de texto utilizaremos el archivo llaneros, guardándolo en la misma carpeta de docker-hadoop.

Regresamos a nuestra terminal, ejecutando el siguiente comando: `docker cp hadoop-mapreduce-examples-2.7.1-sources.jar namenode:/tmp` lo que hace es movernos el archivo a un contenedor temporal. Haremos lo mismo para el archivo de texto: `docker cp llaneros.txt namenode:/tmp`

**Ingresamos al contendor llamado** `namenode`: 
* `docker exec -it namenode bash`

**Luego ponemos (crea una carpeta dentro del contenedor)**:
* `hdfs dfs -mkdir /user/root/input_contador`

**Entramos al contenedor temporal utilizando el comando**

* `cd /tmp`

**El siguiente comando especifica el directorio de entrada**: 

* `hdfs dfs -put llaneros.txt /user/root/input_contador` (si usas un nuevo archivo y no tiene la terminación .txt se pone el nombre tal cual)

**Ahora ejecutamos MapReduce**

Descripción del comando:

* _hadoop-mapreduce-examples-2.7.1-sources.jar_: Es el archivo JAR que contiene ejemplos de MapReduce para Hadoop en la versión 2.7.1. El ejemplo es el de "WordCount".

* _org.apache.hadoop.examples.WordCount_: En este caso, es la clase WordCount, que cuenta palabras en archivos de texto.

* _input_contador_: Aquí es donde están los datos que se van a procesar.

* _output_contador_: Especifica el directorio de salida donde se guardarán los resultados del trabajo.

Es una sola línea de comando:

`hadoop jar hadoop-mapreduce-examples-2.7.1-sources.jar org.apache.hadoop.examples.WordCount input_contador output_contador`

**Para ver tus resultados**

Descripción del comando:

* _hdfs dfs_: Es el comando para interactuar con el sistema de archivos distribuidos (HDFS)

* _-cat_: Muestra el contenido de un archivo

* _/user/root/output_contador/*_: Especifica la ruta de los archivos que quieres leer. El `*` al final indica que se deben mostrar todos los archivos.

Ponemos el comando:

* `hdfs dfs -cat /user/root/output_contador/*`

Para comprobar los resultados:

* `hdfs dfs -ls /user/root/output_contador`

De nuestro resultados lo que nos interesa es el archivo `part-r-00000`, que contiene nuestro recuento de palabras.

Para lo anterior ponemos:

* `hdfs dfs -cat /user/root/output_contador/part-r-00000 > /tmp/llaneros_wc.txt`

Desupés ejecutamos:

* `exit`

Por útimo:

* `docker cp namenode:/tmp/llaneros_wc.txt .`
