# Instalación de servidor de SonarQube

Documento para la instalación guiada del servidor SonarQube.

## Instalación de Java

SonarQube utiliza la última versión de java, por lo que será necesario validar que el equipo tenga la versión más actualizada.

Actualizar los paquetes del sistema

```sh
sudo apt update
```

Install OpenJDK

```sh
sudo apt install openjdk-17-jdk
```

## Instalar PostgreSQL

Para instalar la base de datos se utilizó el siguiente articulo: <https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-20-04-es>

Instalar PostgreSQL.

```sh
sudo apt install postgresql postgresql-contrib
```

Habilitar servicio de posgresql

```sh
sudo systemctl enable postgresql
```

Iniciar la base de datos

```sh
sudo systemctl start postgresql
```

Monitorear el estado del servicio

```sh
sudo systemctl status postgresql
```

## Configuración de usuario postgresql

Utilizamos el usuario postgresql para realizar la configuración necesaria en la base de datos.

Actualizar la contraseña default del usuario

```sh
sudo passwd postgres
|--> Admin123.
```

Cambiarnos al usuario postgresql

```sh
su - postgres
|--> Admin123.
```

En el usuario de la base de datos crear el usuario sonar

```sh
createuser sonar
```

Nos conectamos a la base de datos

```sh
psql
```

Crear una contraseña segura para el usuario sonar en la base de datos

```sql
ALTER USER sonar WITH ENCRYPTED password 'Admin1.';
```

Crear la base de datos SonarQube y asignar al usuario sonar como dueño.

```sql
CREATE DATABASE sonarqube OWNER sonar;
```

Asignar todos los permisos de la base de datos SonarQube al usuario sonar.

```sql
GRANT ALL PRIVILEGES ON DATABASE sonarqube to sonar;
```

Salir de la base de datos PostgreSQL.

```sh
\q
```

Regresar a la cuenta del usuario no root de Linux.

```sh
exit
```

## Descargar e instalar SonarQube

Instalar la utilidad zip, que será necesaria para descomprimir los archivos de SonarQube.

```sh
sudo apt-get install zip -y Locate the latest download URL from the SonarQube official download page.
```

Descargar los archivos de la distribución de SonarQube

```sh
sudo wget https://binaries.sonarsource.com/Dist...
```

Descomprimir el archivo descargado.

```sh
sudo unzip sonarqube-9.6.1.59531.zip
```

Mover los archivos descomprimidos a la ruta /opt/sonarqube

```sh
sudo mv sonarqube-9.6.1.59531 sonarqube
sudo mv sonarqube /opt/
```

## Agregar al grupo y usuario SonarQube

Crear un grupo y usuario dedicado para SonarQube, que no podrá ejecutarse como usuario root.

Crear el grupo sonar

```sh
sudo groupadd sonar
 Create a sonar user and set /opt/sonarqube as the home directory.
```

```sh
sudo useradd -d /opt/sonarqube -g sonar sonar
 Grant the sonar user access to the /opt/sonarqube directory.
```

```sh
sudo chown sonar:sonar /opt/sonarqube -R
```

## Configurar SonarQube

Editar el archivo de configuración de SonarQube.

```sh
sudo nano /opt/sonarqube/conf/sonar.properties
```

Encontrar las siguientes líneas:

```sh
#sonar.jdbc.username=
#sonar.jdbc.password=
```

Des comentar las líneas, y agregar el nombre de la base de datos y usuarios creados en el paso 2.

```sh
sonar.jdbc.username=sonar
sonar.jdbc.password=my_strong_password
```

Bajo esas dos líneas agregar el sonar.jdbc.url

```sh
sonar.jdbc.url=jdbc:postgresql://localhost:5432/sonarqube
```

Guardar el archivo y salir.

### Editar el archivo sonar

```sh
sudo nano /opt/sonarqube/bin/linux-x86-64/sonar.sh
```

Cerca de 50 líneas abajo, localizar la siguiente línea:

```sh
#RUN_AS_USER=
```

Des comentar la línea y cambiar por:

```sh
RUN_AS_USER=sonar
```

Guardar el archivo y salir.

## Configurar el archivo de servicio SonarQube

Crear el archivo para que el servicio de SonarQube se ejecute al iniciar el sistema.

```sh
sudo nano /etc/systemd/system/sonar.service
```

Pega las siguientes líneas en el archivo

```sh
[Unit]
Description=SonarQube service
After=syslog.target network.target

[Service]
Type=forking

ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop

User=sonar
Group=sonar
Restart=always

LimitNOFILE=65536
LimitNPROC=4096

[Install]
WantedBy=multi-user.target
```

Guardar el archivo y salir.

Habilita el servicio de SonarQube para ejecutarse al iniciar el sistema

```sh
sudo systemctl enable sonar
```

Iniciar el servicio SonarQube

```sh
sudo systemctl start sonar
```

Revisar el estatus del servicio

```sh
sudo systemctl status sonar
```

## Modificar los límites del kernel del sistema

SonarQube utiliza Elasticsearch para almacenar sus índices en un directorio MMap FS. Requiere algunos cambios en los valores predeterminados del sistema.

Editar el archivo de configuración sysctl.

```sh
sudo nano /etc/sysctl.conf
```

Agregar las siguientes líneas

```sh
vm.max_map_count=262144
fs.file-max=65536
ulimit -n 65536
ulimit -u 4096
```

Guardar el archivo y salir.

Reiniciar el sistema para aplicar los cambios.

```sh
sudo reboot
```

## Acceder a la interfaz web de SonarQube

Acceder a SonarQube en el navegador web con la IP del servidor en el puerto 9000, Por ejemplo:

```sh
http://IP:9000
```

Iniciar sesión con el usuario `admin` y la contraseña `admin`. SonarQube te solicitara cambiar tu contraseña.
