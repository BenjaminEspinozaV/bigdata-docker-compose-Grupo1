# Big Data Playground

Proyecto para levantar un entorno de Big Data usando Docker Compose. La idea es tener varios servicios relacionados con Hadoop y procesamiento distribuido funcionando en conjunto, sin tener que instalar cada herramienta directamente en el sistema.

El entorno está compuesto principalmente por Hadoop, Hive, Spark, Zeppelin y Livy. Además, se utiliza una estructura de tipo Master-Workers para distribuir el almacenamiento y el procesamiento entre los contenedores.

## Descripción del proyecto

Este proyecto permite crear un pequeño clúster de Big Data utilizando contenedores Docker.

El contenedor `master` funciona como nodo principal y se encarga de coordinar los servicios. Los contenedores `worker1` y `worker2` funcionan como nodos de trabajo y participan en el almacenamiento y procesamiento de los datos.

De esta forma, el proyecto permite probar herramientas que normalmente se utilizan en plataformas de Big Data sin tener que configurar manualmente todos los componentes en diferentes máquinas.

Entre los principales servicios utilizados se encuentran:

* Hadoop HDFS para el almacenamiento distribuido.
* YARN para la administración de recursos.
* Spark para el procesamiento de datos.
* Hive para realizar consultas sobre los datos.
* Zeppelin para trabajar con notebooks.
* Livy para ejecutar trabajos de Spark mediante una API.

## Estructura general

El clúster está formado por:

* `master` - nodo principal.
* `worker1` - primer nodo de trabajo.
* `worker2` - segundo nodo de trabajo.

El `master` utiliza la dirección `172.28.1.1`, mientras que `worker1` utiliza `172.28.1.2` y `worker2` `172.28.1.3`.

En el nodo principal se encuentran los servicios encargados de coordinar el clúster, mientras que los workers ejecutan las tareas y almacenan los bloques de datos de HDFS.

## Requisitos

Para ejecutar el proyecto es necesario tener instalado:

* Docker
* Docker Compose

Se recomienda utilizar Docker Desktop en Windows, ya que incluye Docker Compose.

Para comprobar la instalación:

```bash
docker --version
docker compose version
```

## Instalación

Primero se debe clonar o descargar el proyecto.

```bash
git clone <URL_DEL_REPOSITORIO>
```

Después ingresar a la carpeta del proyecto:

```bash
cd <NOMBRE_DEL_PROYECTO>
```

Revisar que el archivo `docker-compose.yml` se encuentre en la carpeta principal.

## Ejecución

Para levantar todo el entorno se utiliza:

```bash
docker compose up -d
```

El parámetro `-d` permite ejecutar los contenedores en segundo plano.

Para comprobar que los contenedores se encuentran funcionando:

```bash
docker compose ps
```

También se pueden revisar los logs con:

```bash
docker compose logs
```

o consultar un servicio específico:

```bash
docker compose logs master
```

Para detener el entorno:

```bash
docker compose down
```

Si se desea volver a iniciar posteriormente, solamente se ejecuta nuevamente:

```bash
docker compose up -d
```

## Organización del clúster

La arquitectura utiliza un modelo Master-Workers.

El `master` funciona como nodo principal y coordina los servicios de Hadoop, YARN y Spark.

Los workers son los encargados de ejecutar las tareas y almacenar información. En este caso:

* `worker1` → `172.28.1.2`
* `worker2` → `172.28.1.3`

Cada worker cuenta con los componentes necesarios para participar en HDFS, YARN y Spark.

Esto permite repartir las tareas entre varios contenedores en lugar de ejecutar todo en un solo nodo.

## Servicios utilizados

### Hadoop HDFS

HDFS se utiliza para almacenar los archivos de manera distribuida. Los datos pueden dividirse en bloques y repartirse entre los diferentes DataNodes del clúster.

### YARN

YARN se encarga de administrar los recursos disponibles y distribuir las tareas entre los nodos trabajadores.

### Apache Spark

Spark se utiliza para el procesamiento distribuido de los datos. Los trabajos pueden ejecutarse utilizando los recursos de los workers disponibles en el clúster.

### Apache Hive

Hive permite trabajar con los datos almacenados en HDFS mediante consultas similares a SQL.

### Zeppelin

Zeppelin proporciona una interfaz basada en notebooks para trabajar con los datos y ejecutar consultas o código de procesamiento.

### Livy

Livy permite interactuar con Spark mediante una interfaz REST. Esto facilita el envío y control de trabajos de Spark desde otras aplicaciones.

## Resultados

Después de iniciar el proyecto se comprobó que los diferentes contenedores pudieran comunicarse entre sí y que los servicios principales estuvieran activos.

Entre los resultados obtenidos se encuentran:

* El clúster se levanta mediante un solo archivo `docker-compose.yml`.
* El nodo `master` puede coordinar los workers.
* HDFS distribuye el almacenamiento entre los nodos trabajadores.
* YARN permite administrar los recursos del clúster.
* Spark puede utilizar los workers para ejecutar procesos distribuidos.
* Hive permite realizar consultas sobre los datos almacenados.
* Zeppelin permite trabajar con notebooks.
* Livy permite enviar trabajos de Spark mediante una API.

El estado de los contenedores puede comprobarse con:

```bash
docker compose ps
```

También se pueden revisar los registros de cada servicio para verificar que no existan errores:

```bash
docker compose logs
```

## Detener el proyecto

Para detener y eliminar los contenedores:

```bash
docker compose down
```

Para eliminar también los volúmenes asociados, en caso de ser necesario:

```bash
docker compose down -v
```

## Conclusión

El proyecto permite contar con un entorno básico de Big Data funcionando sobre Docker. La utilización de un nodo principal y varios workers facilita la prueba de herramientas como Hadoop, Hive y Spark en un mismo entorno.

Una de las principales ventajas es que toda la infraestructura puede levantarse y detenerse utilizando Docker Compose, lo que simplifica bastante la configuración y permite repetir las pruebas de forma más rápida.
