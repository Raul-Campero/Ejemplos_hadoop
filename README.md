## Ejemplos_hadoop
Hola, bienenido a este repositorio.

Aquí encontrarás ejemplos y comandos para utilizar docker-hadoop.

Los créditos correspondientes son para: [github/docker-hadoop](https://github.com/big-data-europe/docker-hadoop)

Asumiendo que se tiene Docker instaldo seguiremos los siguientes pasos, en caso de no contar con Docker instalado dirigirse a la siguiente página: [Docker Desktop](https://www.docker.com/products/docker-desktop/)

Una vez teniendo Docker-desktop se descargará el archivo ZIP del repositorio correspondiente a los créditos. Para descargar el ZIP dar click en **Code** y después **Download ZIP**.

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
