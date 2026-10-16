# BIND MOUNT
En un bind mount mapeamos (montar) un directorio o archivo específico del sistema de archivos del host con una parte del sistema de ficheros del contenedor.

```
docker run -d --name <nombre contenedor> -v <ruta carpeta host>:<ruta carpeta contenedor> <imagen> 
```
ó
```
docker run -d --name <nombre contenedor> --mount type=bind,source=<ruta carpeta host>,target=<ruta carpeta contenedor> <imagen>
```
- destination, dst, target: La ruta donde se monta el archivo o directorio en el contenedor.
- source, src: El origen del montaje.
  
### En tu computador crear una carpeta llamada nginx y dentro de esta carpeta crea otra llamada html. Como se aprecia en la figura.
![Volúmenes](directorio.PNG)

### Crear un contenedor con la imagen nginx:alpine, mapear todos por puertos, para la ruta carpeta host colocar el directorio en donde se encuentra la carpeta html en tu computador y para la ruta carpeta contenedor: /usr/share/nginx/html (esta ruta se obtiene al revisar la documentación de la imagen)
![Volúmenes](volumen-host.PNG)
# COMPLETAR CON EL COMANDO
docker run -d --name contenedor-nginx -p 8080:80 --mount type=bind,source=/Users/P3E010-D/Documents/nginx/html,target=/usr/share/nginx/html nginx:alpine

### ¿Qué sucede al ingresar al servidor de nginx?
Al ingresar a http://localhost:8080 en mi navegador, se muestra un error 403 Forbidden. Esto sucede porque mi carpeta local html está completamente vacía y, al realizar el bind mount, se ocultó el archivo de bienvenida por defecto de Nginx, por lo que el servidor no encuentra ningún archivo para mostrar.

### ¿Qué pasa con el archivo index.html del contenedor?
El archivo index.html original que venía dentro del contenedor fue sobrescrito u ocultado por el montaje de mi carpeta local. Como mi directorio del host está vacío, el contenedor pasa a leer un directorio vacío en esa ruta.

### Ir a https://html5up.net/ y descargar un template gratuito, descomprirlo dentro de tu computador en la carpeta html
### ¿Qué sucede al ingresar al servidor de nginx?
Al ingresar de nuevo a http://localhost:8080, se visualiza correctamente la página web de la plantilla que descargué. El servidor web refleja los archivos de inmediato sin necesidad de reiniciar el contenedor, demostrando que lee directamente mi carpeta local.

### Eliminar el contenedor
docker rm -f contenedor-nginx

### ¿Qué sucede al crear nuevamente un contenedor montado al directorio definidos anteriormente?
Al ejecutar otra vez el comando de creación del contenedor montado a la misma carpeta, el servidor de Nginx vuelve a mostrar de inmediato la página web de la plantilla de HTML5 UP.
Esto sucede porque los datos están almacenados de forma persistente en mi computadora (host). Aunque eliminé el contenedor anterior, los archivos sobrevivieron de manera independiente, y el nuevo contenedor puede acceder a ellos instantáneamente al montarse en la misma ruta.


