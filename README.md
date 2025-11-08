´´´
# Define la versión del formato de Docker Compose.
version: '3.8'
# 'services' es la sección donde defines todos tus contenedores.
services:

  # Este es el nombre de tu servicio de WordPress
  # Este nombre es importante para la comunicación interna.
  wp:
    # Equivalentes a tu comando

    # 'image' es la imagen de Docker que se usará.
    image: wordpress:6-apache

    # 'container_name' le da un nombre fijo al contenedor.
    container_name: wp

    # 'ports' mapea los puertos. Formato: "PUERTO_TU_PC:PUERTO_DEL_CONTENEDOR"
    ports:
      - "8082:80"

    # Añadidos para que funcione

    # 'restart: always' asegura que el contenedor se reinicie solo
    # si se cae o si reinicias el PC.
    restart: always

    # 'volumes' hace que los datos sean persistentes (no se borren con el contenedor).
    # 'wp_data' es un "volumen nombrado" que definimos más abajo.
    # Esto guarda tu carpeta /wp-content/ (temas, plugins, archivos subidos).
    volumes:
      - wp_data:/var/www/html

    # 'environment' define las variables de entorno que WordPress necesita
    # para conectarse a la base de datos que también vamos a crear.
    environment:
      # Le dice a WordPress dónde está la base de datos.
      # 'db' es el nombre del servicio de la base de datos (ver abajo).
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_NAME: wordpress_db
      WORDPRESS_DB_USER: wp_user
      WORDPRESS_DB_PASSWORD: password_seguro

    # 'depends_on' le dice a Compose que no inicie el servicio 'wp'
    # hasta que el servicio 'db' esté listo y corriendo.
    depends_on:
      - db

  # Nuevo Servicio: La Base de Datos
  
  # Este es el servicio de la base de datos. Lo llamamos 'db'.
  db:
    # Usamos la imagen oficial de MariaDB.
    image: mariadb:latest
    container_name: wp_db
    restart: always

    # Otro volumen para que los datos de la BD (posts, usuarios, etc.) no se pierdan.
    volumes:
      - db_data:/var/lib/mysql

    # Variables de entorno para configurar la base de datos al crearla.
    environment:
      # Contraseña del superadmin (root) de la base de datos.
      MYSQL_ROOT_PASSWORD: password_root_MUY_seguro
      # Crea esta base de datos al iniciar.
      MYSQL_DATABASE: wordpress_db
      # Crea este usuario.
      MYSQL_USER: wp_user
      # Le da esta contraseña al usuario.
      MYSQL_PASSWORD: password_seguro

# 'volumes' es la sección donde se "declaran" los volúmenes nombrados
# que usamos arriba. Docker gestionará dónde se guardan estos datos en tu PC.
volumes:
  wp_data:
  db_data:
´´´
