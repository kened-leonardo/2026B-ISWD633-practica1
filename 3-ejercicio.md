## Esquema para el ejercicio
![Imagen](esquema-ejercicio3.PNG)

### Crear red net-wp
docker network create net-wp

### Para que persista la información es necesario conocer en dónde mysql almacena la información.
# COMPLETAR LA SIGUIENTE ORACIÓN. REVISAR LA DOCUMENTACIÓN DE LA IMAGEN EN https://hub.docker.com/
En el esquema del ejercicio carpeta del contenedor (a) es /var/lib/mysql

Ruta carpeta host: C:\Users\P3E010-D\Documents/ejercicio3/db

### ¿Qué contiene la carpeta db del host?
Inicialmente, la carpeta db del host está completamente vacía. No contiene ningún archivo ni directorio porque todavía no se ha montado ningún contenedor en ella.

### Crear un contenedor con la imagen mysql:8  en la red net-wp, configurar las variables de entorno: MYSQL_ROOT_PASSWORD, MYSQL_DATABASE, MYSQL_USER y MYSQL_PASSWORD
docker run -d --name contenedor-db --network net-wp -v C:\Users\P3E010-D\Documents\ejercicio3\db:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=rootpassword -e MYSQL_DATABASE=wordpress_db -e MYSQL_USER=wp_user -e MYSQL_PASSWORD=wppassword mysql:8

### ¿Qué observa en la carpeta db que se encontraba inicialmente vacía?
Al iniciar el contenedor, la carpeta db del host se llena automáticamente con archivos y carpetas internas del sistema de MySQL (como ibdata1, archivos de configuración y directorios de bases de datos). Esto confirma que MySQL inicializó y guardó los datos directamente en mi disco local.

### Para que persista la información es necesario conocer en dónde wordpress almacena la información.
# COMPLETAR LA SIGUIENTE ORACIÓN. REVISAR LA DOCUMENTACIÓN DE LA IMAGEN EN https://hub.docker.com/
En el esquema del ejercicio la carpeta del contenedor (b) es /var/www/html.

Ruta carpeta host: C:\Users\P3E010-D\Documents/ejercicio3/www

### Crear un contenedor con la imagen wordpress en la red net-wp, configurar las variables de entorno WORDPRESS_DB_HOST, WORDPRESS_DB_USER, WORDPRESS_DB_PASSWORD y WORDPRESS_DB_NAME (los valores de estas variables corresponden a los del contenedor creado previamente)
docker run -d --name contenedor-wp --network net-wp -p 9500:80 -v C:\Users\P3E010-D\Documents\ejercicio3\www:/var/www/html -e WORDPRESS_DB_HOST=contenedor-db -e WORDPRESS_DB_USER=wp_user -e WORDPRESS_DB_PASSWORD=wppassword -e WORDPRESS_DB_NAME=wordpress_db wordpress

### Personalizar la apariencia de wordpress y agregar una entrada
<img width="3840" height="1908" alt="image" src="https://github.com/user-attachments/assets/cb39c5c3-c3b7-42f6-8948-bb5b88d7c6d2" />


### Eliminar el contenedor y crearlo nuevamente, ¿qué ha sucedido?

Al eliminar el contenedor web y volverlo a levantar apuntando al mismo directorio y red, noto que la página web no se ha perdido y mantiene exactamente el tema configurado y la entrada que había creado. No me volvió a pedir la pantalla de instalación. Esto sucede porque toda la información de la web quedó almacenada permanentemente en las carpetas locales de mi computadora (www y db), logrando que los datos sobrevivan al ciclo de vida del contenedor de forma exitosa.

