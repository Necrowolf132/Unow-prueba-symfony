# Proyecto Fullstack con Symfony, React y Python Microservice (Unow test tecnico symfony)

Este proyecto combina un API REST con Symfony, un frontend con React, y un microservicio en Python, todos conectados mediante Docker Compose. Se incluye un servidor MySQL para la base de datos y un servidor Nginx con HTTPS (certificados autofirmados) para el proxy inverso.

---

## 📋 Requisitos

Antes de comenzar, asegúrate de tener instalados:

- [Docker](https://www.docker.com/products/docker-desktop)
- [Docker Compose](https://docs.docker.com/compose/)
- [mkcert](https://github.com/FiloSottile/mkcert)

---

## ⚙️ Configuración Inicial

1.  Clona este repositorio:

    ```bash
    git clone https://github.com/tu_usuario/proyecto-fullstack.git
    cd proyecto-fullstack
    ```

2.  Correr el comando mkcert con el nombre de dominio deseado Ej. "mckcert \*.develop.jeelidev.online" y luego "mkcert -install" (estas son las que ya existen por defecto en la carpeta certs)

    NOTA IMPORTANTE:

    En caso de cambiar los nombres de dominio autofirmados, y generar nuevos certifitifiacados se tiene en remplazar en la carpeta certs y modificar el archivo default.conf con los nombres correspondientes en la lineas a continuacion (tambien puede simplemente usar los que ya estan)

    ```bash
        ssl_certificate /etc/nginx/certs/_wildcard.develop.jeelidev.online.pem;
        ssl_certificate_key /etc/nginx/certs/_wildcard.develop.jeelidev.online-key.pem;
    ```

    ```bash

    server {
        listen 443 ssl;
        server_name unow-symfony-prueba-api.develop.jeelidev.online;


    ```

3.  Modificar tu archivo /etc/host para que el dominio creado apunte a la direccion ip 127.0.0.1
4.  Corrar solo el contenedor de la base de datos con el comando

    ```bash
     docker-compose up mysql --build
    ```

    y mientras esta ejecutando correr el script de restauracion de base de datos en la raiz del proyecto de la siguinte manera

    ```bash
    ./mysql_backup_restore.sh --host localhost --port 3306 --user root --password sgkxOSxSKVXFDcbpMOYvamAFtPCSESsg --database  symfony_db --restore ./backups/backup.sql
    ```

5.  Crea un archivo .env en la raíz del proyecto (o ajusta el existente)

    ```bash
    APP_ENV=dev
    APP_SECRET=your_secret
    ```

6.  Ejecutar el comando

    ```bash
    docker-compose down mysql
    ```

    Para detener el contenedor de mysql

## 🚀 Despliegue Local

1. Construye y levanta los contenedores con el siguiente comando en la raiz del proyecto:

   ```bash
   docker-compose up --build
   ```

2. Accede a los servicios (recuerde cambiar localhost por los dominios autofirmados que genero en caso de que los generara):

```bash
   API (Symfony): https://unow-symfony-prueba-api.develop.jeelidev.online/api/
   Frontend (React): https://unow-symfony-prueba-api.develop.jeelidev.online/
   Microservicio (Python): https://unow-symfony-prueba-api.develop.jeelidev.online/microservice/
```

## 🗃️ Base de Datos

El servicio MySQL está configurado con las siguientes credenciales (definidas en docker-compose.yml):

    Host: mysql (o localhost)
    Usuario: root
    Contraseña: sgkxOSxSKVXFDcbpMOYvamAFtPCSESsg
    Base de datos: symfony_db

Para conectarte a MySQL desde tu máquina local puede usar esas credenciales con cualquier manejador de base de datos o usar la temrinal con le cliente mysql:

    mysql -h 127.0.0.1 -P 3306 -u root -p

## 📦 Instalación Adicional

Recuerda que siempre puede por medio de docker conectarte a cualquier de los contenedores de docker que se encuentren ejecutandose para correr comandos de docker o propios de npm o de la tecnologia que estes usando en cuestion

### Backend (Symfony)

Accede al contenedor del backend:

```bash
docker exec -it symfony-api /bin/bash
```

Ejecuta las migraciones para crear las tablas:

```bash
php bin/console doctrine:migrations:migrate
```

### Frontend (React)###

Si es necesario, instala dependencias extras o levanta el servicio en diferido entrando directamente al contenedor:

```bash
docker exec -it react-frontend /bin/bash
```

## 🛠 Solución de Problemas

Es posible que suceda que aparezcan Certificado SSL no confiable
El navegador puede marcar el certificado autofirmado como no seguro. Para evitar esto es muy importante se genero las credenciales nuevamente usar el comando "mkcert -install" esto deberia pasar las cedenciales directamente a las carpetas de sistema donde estan los validadores de certificacion de su navegador.
otra alternativa es:

Descarga el archivo nginx/certs/\_wildcard.develop.jeelidev.online.pem (o nombre nuevo que creaste)
y Instálalo manualmente en tu sistema como un certificado confiable.
