# Caso práctico: Subir imágenes a Docker Hub

## Objetivo

El objetivo de este caso práctico es subir una imagen a Docker Hub desde nuestro equipo local.

## Requisitos

- Tener instalado Docker en nuestro equipo local.
- Tener una cuenta en Docker Hub.
- Tener una imagen creada en nuestro equipo local.


## Desarrollo

En este caso práctico vamos a realizar varios pasos:

1. Login en Docker Hub.
2. Crear una imagen de PHP-FPM con XDebug incluido, y nombralá siguiendo la convención de nombres de Docker Hub.
3. Subir la imagen a Docker Hub.
   

### 1. Iniciar sesión en Docker Hub

1. Ingresar a [Docker Hub](https://hub.docker.com/) y crear una cuenta si no la tienes.
2. Ingresar a la consola de Docker en tu equipo local.
3. Ingresar el siguiente comando para iniciar sesión en Docker Hub:

   ```bash
   docker login
   ```

   Nos pedirá nuestro usuario y contraseña de Docker Hub.

4. Ingresar el siguiente comando para subir nuestra imagen a Docker Hub:

   ```bash
    docker push <nombre_de_usuario>/<nombre_de_imagen>:<tag>
    ```

    Donde:
    - `<nombre_de_usuario>` es el nombre de usuario de Docker Hub.
    - `<nombre_de_imagen>` es el nombre de la imagen que queremos subir.
    - `<tag>` es la versión de la imagen que queremos subir.
    - Ejemplo: `docker push johndoe/nginx:1.0`
    - Si no especificamos el tag, se subirá la imagen con el tag `latest`.
    - Ejemplo: `docker push johndoe/nginx`
    - Si no especificamos el nombre de usuario, se subirá la imagen con el nombre de usuario `library`.

5. Ingresar a [Docker Hub](https://hub.docker.com/) y verificar que nuestra imagen se haya subido correctamente.

</br>


### 2. Crear la imagen de PHP-FPM con XDebug incluido

En el caso práctico anterior, creamos una imagen de PHP-FPM con XDebug incluido. Ahora vamos a subir esta imagen a Docker Hub.

Para nombrar a nuestra imagen es importante seguir la conveción de nombres de Docker Hub, que es la siguiente:

  - Nombre de usuario: `johndoe`
  - Nombre de la imagen: `php
  - Tag: `8.3.0-fpm-xdebug` dentro del tag, 
    - el primer dato es la versión de PHP
    - el segundo dato indica que corresponde a PHP-FPM
    - el tercer dato indica que tiene XDebug incluido (se llama variante)


En el caso práctico anterior, en el `dockerfile` de php-fpm, mapeamos los ficheros de configuración de XDebug. Eso era lo adecuado para ese proyecto ya que la configuración, pero si queremos tener una imagen con XDebug incluido, y con una configuración por defecto, lo mejor es copiar los ficheros de configuración de XDebug directamente en la imagen, ya así poder reutilizarla en otros proyectos.

Pero ahora en el dockerfile, en lugar de mapear los ficheros de configuración de XDebug, vamos a copiarlos directamente en la imagen. Para ello, vamos a modificar el fichero `Dockerfile` de la siguiente manera:

```Dockerfile
FROM php:8.0-fpm

# Install xdebug
# --> PECL is a repository for PHP Extensions and as PECL is not installed by default, we need to install it first
# --> xdebug is a PHP extension which provides debugging and profiling capabilities
# --> Activate el módulo xdebug, a través de la función docker-php-ext-enable
RUN pecl install xdebug-3.3.0 \
    && docker-php-ext-enable xdebug

# Copiar ficheros de configuración de XDebug
COPY ./docker/php/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini
COPY ./docker/php/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

EXPOSE 9000

```

Y ahora vamos a construir la imagen con el siguiente comando:

```bash
docker build -t {mi-usuario}}/php:8.3-fpm-xdebug
```

### 3. Subir imagen PHP-FPM com XDebug incluido a Docker Hub

Una vez que hemos creado la imagen, vamos a subirla a Docker Hub con el siguiente comando:

```bash
docker push {mi-usuario}/php:8.3-fpm-xdebug
```

Y una vez finalizado este paso, ya tenemos nuestra imagen subida a Docker Hub, y podemos reutilizar en otros proyectos.


## Conclusión

Hemos subido una imagen a Docker Hub desde nuestro equipo local. Esto nos permite reutilizar la imagen en otros proyectos, y compartir la imagen con otros desarrolladores.
