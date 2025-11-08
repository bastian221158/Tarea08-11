# 🚀 Proyecto WordPress con Docker Compose

Este repositorio contiene un archivo `docker-compose.yml` listo para desplegar un sitio de WordPress (con Apache) junto a una base de datos MariaDB.

---

## 🛠️ Archivo `docker-compose.yml`

El siguiente es el contenido completo del archivo `docker-compose.yml` necesario para levantar los servicios. El código está documentado línea por línea para explicar qué hace cada parte.

Simplemente copia este código en un archivo llamado `docker-compose.yml` y ejecútalo.

```yml
# Define la versión del formato de Docker Compose.
version: '3.8'

# 'services' es la sección donde defines todos tus contenedores.
services:

  # Este es el nombre de tu servicio de WordPress
  # Este nombre es importante para la comunicación interna.
  wp:

    # 'image' es la imagen de Docker que se usará.
    image: wordpress:6-apache

    # 'container_name' le da un nombre fijo al contenedor.
    container_name: wp

    # 'ports' mapea los puertos. Formato: "PUERTO_TU_PC:PUERTO_DEL_CONTENEDOR"
    ports:
      - "8082:80"

    # --- Añadidos para que funcione ---

    # 'restart: always' asegura que el contenedor se reinicie solo
    # si se cae o si reinicias el PC.
    restart: always

    # 'volumes' hace que los datos sean persistentes.
    # 'wp_data' es un "volumen nombrado" que definimos más abajo.
    volumes:
      - wp_data:/var/www/html

    # 'environment' define las variables de entorno que WordPress necesita
    # para conectarse a la base de datos que también vamos a crear.
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_NAME: wordpress_db
      WORDPRESS_DB_USER: wp_user
      WORDPRESS_DB_PASSWORD: password_seguro

    # 'depends_on' le dice a Compose que no inicie el servicio 'wp'
    # hasta que el servicio 'db' esté listo y corriendo.
    depends_on:
      - db

  # --- Nuevo Servicio: La Base de Datos ---
  
  # Este es el servicio de la base de datos. Lo llamamos 'db'.
  db:
    # Usamos la imagen oficial de MariaDB.
    image: mariadb:latest
    container_name: wp_db
    restart: always
    volumes:
      - db_data:/var/lib/mysql

    # Variables de entorno para configurar la base de datos al crearla.
    environment:
      MYSQL_ROOT_PASSWORD: password_root_MUY_seguro
      MYSQL_DATABASE: wordpress_db
      MYSQL_USER: wp_user
      MYSQL_PASSWORD: password_seguro

# 'volumes' es la sección donde se "declaran" los volúmenes nombrados
volumes:
  wp_data:
  db_data:
```

---

## 🚀 Cómo Arrancar

1.  Crea una carpeta para tu proyecto.
2.  Crea un archivo llamado `docker-compose.yml` y pega el código de arriba.
3.  Cambia las contraseñas `password_seguro` y `password_root_MUY_seguro`.
4.  Abre una terminal en esa carpeta y ejecuta:
    ```bash
    docker-compose up -d
    ```
5.  Accede a tu sitio en **`http://localhost:8082`**.

---

## 🛑 Comandos Útiles

* **Para detener los contenedores:**
    ```bash
    docker-compose down
    ```
* **Para detener Y BORRAR los datos:**
    ```bash
    docker-compose down -v
    ```
