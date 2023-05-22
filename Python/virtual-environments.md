# Entornos virtuales Python

## Instalación de librería

Para la creación de los entornos virtuales es necesario instalar vía pip la librería de virual env. Para eso usaremos el siguiente comando:

```sh
pip3 install virtualenv
```

Lo anterior agregara la librería al equipo. Es importante considerar que para Mac fue necesario agregar `sudo` al inicio para su correcta instalación.

## Creación de entorno virtual

Una vez instalada la librería procedemos a crear el entorno virtual con el siguiente comando:

```sh
virtualenv Geocoding
```

El comando anterior creara una nueva carpeta con el nombre del entorno, así como los archivos correspondientes.

### Activación de entorno virtual

Para activar el entorno accederemos a la carpeta creada y ejecutaremos cualquiera de los dos siguientes comandos:

```sh
. ./bin/activate 
```

```sh
source ./bin/activate 
```

Sabremos que el entorno fue correctamente activado porque veremos el nombre entre paréntesis al inicio de la línea de comandos.

```sh
(Geocoding) jorgelpz@192 Geocoding %   
```

Una vez activado el entorno todas las librerías instaladas se instalarán únicamente en el entorno. Para la instalación de nuevas librerías se utiliza el comando `pip install` como se hace de manera normal.

### Desactivar entorno virtual

Para desactivar el entorno virtual ejecutaremos el siguiente comando en la carpeta que se creó al crear el entorno virtual.

```sh
deactivate
```

### Respaldar librerías

Para almacenar las librerías utilizada en el proyecto y estás puedan ser compartidas con el equipo almacenaremos dichas librerías con su respectiva versión en un archivo. Para eso utilizaremos el siguiente comando:

```sh
pip freeze > requeriments.txt
```

El comando debe ser ejecutado en la carpeta creada por la librería de `virualenv`.

Para restaurar dichas librerías desde un archivo se ejecuta el siguiente comando:

```sh
pip install requeriments.txt
```

## Comandos pip

El siguiente comando instala las librerías en Python, o en el entorno virtual activo.

```sh
pip install  
```

El siguiente comando desinstala las librerías en Python, o en el entorno virtual activo.

```sh
pip uninstall  
```

El siguiente comando muestra las librerías instaladas en Python, o en el entorno virtual activo.

```sh
pip freeze  
```
