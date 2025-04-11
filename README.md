[![Gitter chat](https://badges.gitter.im/gitterHQ/gitter.png)](https://gitter.im/big-data-europe/Lobby)

# ¿Cómo levantar el nodo de Hadoop?

Una vez instalada la plataforma de sotware Docker y habiendo configurado corrrectamente el Subsistema de Windows para Linux (WLS), se procederá a realizar lo siguiente:

1.- Ejecutar docker y la terminal de comandos de Windows(CMD), ambos en modo admin.

2.- Desde el CMD ingresar a la carpeta donde tenemos el repositorio de Docker-Hadoop

![image](https://github.com/user-attachments/assets/115748a8-552d-4083-ac99-fe49a7965fbf)

3.- Ejecutamos el siguiente código: docker-compose up -d

![image](https://github.com/user-attachments/assets/58c595d0-766b-411d-9dc4-f4be98d52a9e)

Este comando va a iniciar los 5 contenedores requeridos. Al ejecutarse por 1era vez, se deberá esperar a que se completen las descargas correspondientes.

# ¿Cómo hacer el mapreduce de la tarea 2?

1.0- Levantar un nodo de Hadoop

2.0- Entrar al nodo maestro

3.0- Crear una carpeta en la raiz(root). Ej: imput_contador_tarea2

4.0- Elegir un archivo de tú elección (se admiten formatos CSV, JSON, XML y otros).

4.1.- Se descargó un archivo de texto de internet llamado robots.txt

4.2.- Para éste ejercicio se requirió de un archivo de java(.jar)

5.0- Ingresar el archivo de texto en la carpeta donde tenemos el repositorio de Docker-Hadoop

5.1- Ingresar el archivo de java en la carpeta donde tenemos el repositorio de Docker-Hadoop

![image](https://github.com/user-attachments/assets/08bd9e19-d25b-482b-aa8d-83be0dd574ca)

6.0.- Ingresar ambos archivos (.txt y .jar) en la carpeta temporal de docker compose

![image](https://github.com/user-attachments/assets/21253196-d636-49d3-b6e6-c0f5e33bb12d)

7.0.- Desde la ruta temporal (tmp), cargar el archivo en el sistema de Hadoop (HDFS)

![image](https://github.com/user-attachments/assets/44671f7d-12e0-488b-8423-a1081bdf6ebb)

8.0.- Desde la misma ruta (tmp), ejecutar MapReduce (archivo .jar)

![image](https://github.com/user-attachments/assets/03708abf-14e7-4a55-9b62-73f8a0b2b5b9)

9.0.- ejecutar el comando hdfs dfs -cat, seguido de la ruta donde se encuentran nuestros archivos en la raiz y el símbolo *
9.1.- Se muestra el contenido con el conteo de cada palabra contada en el archivo robots.txt

![image](https://github.com/user-attachments/assets/fbc58355-2715-46dd-97fc-a513829ab0ed)

10.0.- Verificar los archivos de salida y exportar que el archivo part-r-00000 que es el que tiene el resultado
10-1.- ejecutar de nuevo el comando hdfs dfs -cat para exportar el archivo al formato y nombre deseados. Ej. robots_wc.xt
10.2.- Ejecutar el siguiente comando docker cp namenode:/tmp/ + el nombre del archivo con su extensión y el símbolo (.)
Ej. docker cp namenode:/tmp/robots_wc.txt .

![image](https://github.com/user-attachments/assets/8e3235ea-9280-43e6-aff6-74feacf8ffed)

11.0.- Verificar que el archivo se haya creado en el repositorio

![image](https://github.com/user-attachments/assets/1f9ff6f2-ee72-4e91-8740-cbf73958e74f)


# ¿Cómo hacer el mapreduce de la tarea 3?

## Supported Hadoop Versions
See repository branches for supported hadoop versions

## Quick Start

To deploy an example HDFS cluster, run:
```
  docker-compose up
```

Run example wordcount job:
```
  make wordcount
```

Or deploy in swarm:
```
docker stack deploy -c docker-compose-v3.yml hadoop
```

`docker-compose` creates a docker network that can be found by running `docker network list`, e.g. `dockerhadoop_default`.

Run `docker network inspect` on the network (e.g. `dockerhadoop_default`) to find the IP the hadoop interfaces are published on. Access these interfaces with the following URLs:

* Namenode: http://<dockerhadoop_IP_address>:9870/dfshealth.html#tab-overview
* History server: http://<dockerhadoop_IP_address>:8188/applicationhistory
* Datanode: http://<dockerhadoop_IP_address>:9864/
* Nodemanager: http://<dockerhadoop_IP_address>:8042/node
* Resource manager: http://<dockerhadoop_IP_address>:8088/

## Configure Environment Variables

The configuration parameters can be specified in the hadoop.env file or as environmental variables for specific services (e.g. namenode, datanode etc.):
```
  CORE_CONF_fs_defaultFS=hdfs://namenode:8020
```

CORE_CONF corresponds to core-site.xml. fs_defaultFS=hdfs://namenode:8020 will be transformed into:
```
  <property><name>fs.defaultFS</name><value>hdfs://namenode:8020</value></property>
```
To define dash inside a configuration parameter, use triple underscore, such as YARN_CONF_yarn_log___aggregation___enable=true (yarn-site.xml):
```
  <property><name>yarn.log-aggregation-enable</name><value>true</value></property>
```

The available configurations are:
* /etc/hadoop/core-site.xml CORE_CONF
* /etc/hadoop/hdfs-site.xml HDFS_CONF
* /etc/hadoop/yarn-site.xml YARN_CONF
* /etc/hadoop/httpfs-site.xml HTTPFS_CONF
* /etc/hadoop/kms-site.xml KMS_CONF
* /etc/hadoop/mapred-site.xml  MAPRED_CONF

If you need to extend some other configuration file, refer to base/entrypoint.sh bash script.
