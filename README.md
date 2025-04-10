[![Gitter chat](https://badges.gitter.im/gitterHQ/gitter.png)](https://gitter.im/big-data-europe/Lobby)

# ¿Cómo levantar el nodo de hadoop?

Una vez instalada la plataforma de sotware Docker y habiendo configurado corrrectamente el Subsistema de Windows para Linux (WLS), se procederá a realizar lo siguiente:

1.- Ejecutar docker y la terminal de comandos de Windows(CMD), ambos en modo admin.
2.- Desde el CMD ingresar a la carpeta donde tenemos el repositorio de Docker-Hadoop
![image](https://github.com/user-attachments/assets/76445368-0e09-4f38-8512-6490008ccbf9)
3.- Ejecutamos el siguiente código: docker-compose up -d
![image](https://github.com/user-attachments/assets/58c595d0-766b-411d-9dc4-f4be98d52a9e)

Este comando va a iniciar los 5 contenedores requeridos. Al ejecutarse por 1era vez, se deberá esperar a que se completen las descargas correspondientes.

# ¿Cómo hacer el mapreduce de la tarea 2?

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
