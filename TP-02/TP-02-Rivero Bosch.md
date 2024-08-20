### RIVERO BOSCH JOSEFINA


# 4- Desarrollo:

# 1- Instalar Docker Community Edition
Diferentes opciones para cada sistema operativo
https://docs.docker.com/
Ejecutar el siguiente comando para comprobar versiones de cliente y demonio.
docker versión

# 2- Explorar DockerHub
Registrase en docker hub: https://hub.docker.com/
Familiarizarse con el portal

# 3- Obtener la imagen BusyBox
Ejecutar el siguiente comando, para bajar una imagen de DockerHub
docker pull busybox
![Descripción de la imagen](imagen1.jpg)

Verificar qué versión y tamaño tiene la imagen bajada, obtener una lista de imágenes locales:
docker images
![Descripción de la imagen](imagen2.jpg)

# 4- Ejecutando contenedores
Ejecutar un contenedor utilizando el comando run de docker:
docker run busybox
![Descripción de la imagen](imagen3.jpg)

Explicar porque no se obtuvo ningún resultado

Especificamos algún comando a correr dentro del contenedor, ejecutar por ejemplo:

docker run busybox echo "Hola Mundo"
![Descripción de la imagen](imagen4.jpg)

Ver los contenedores ejecutados utilizando el comando ps:
docker ps
![Descripción de la imagen](imagen5.jpg)

Vemos que no existe nada en ejecución, correr entonces:
docker ps -a
![Descripción de la imagen](imagen6.jpg)

Mostrar el resultado y explicar que se obtuvo como salida del comando anterior.

# 5- Ejecutando en modo interactivo
Ejecutar el siguiente comando
docker run -it busybox sh
![Descripción de la imagen](imagen7.jpg)

Para cada uno de los siguientes comandos dentro de contenedor, mostrar los resultados:
ps
![Descripción de la imagen](imagen8.jpg)

uptime
![Descripción de la imagen](imagen9.jpg)

free
![Descripción de la imagen](imagen10.jpg)

ls -l /
![Descripción de la imagen](imagen11.jpg)

Salimos del contenedor con:
exit

# 6- Borrando contenedores terminados
Obtener la lista de contenedores
docker ps -a
![Descripción de la imagen](imagen12.jpg)

Para borrar podemos utilizar el id o el nombre (autogenerado si no se especifica) de contenedor que se desee, por ejemplo:
docker rm elated_lalande
![Descripción de la imagen](imagen13.jpg)

Para borrar todos los contenedores que no estén corriendo, ejecutar cualquiera de los siguientes comandos:
docker rm $(docker ps -a -q -f status=exited)
docker container prune
![Descripción de la imagen](imagen14.jpg)

# 7- Construir una imagen
Conceptos de DockerFile
Leer https://docs.docker.com/engine/reference/builder/
Describir las instrucciones

FROM: Es la primera instrucción en cualquier Dockerfile y especifica la imagen base a partir de la cual se construirá la nueva imagen. Cada Dockerfile debe comenzar con una instrucción FROM.

RUN: Ejecuta cualquier comando en una capa nueva de la imagen. Es comúnmente usado para instalar paquetes necesarios.

ADD: Copia archivos/directorios desde el host a una imagen Docker. A diferencia de COPY, ADD puede extraer archivos tar automáticamente y también puede funcionar con URLs.

COPY: Similar a ADD, pero solo para copiar archivos o directorios locales al sistema de archivos de la imagen en una ubicación especificada. 

EXPOSE: Informa a Docker que el contenedor escuchará en el puerto especificado en tiempo de ejecución. No abre el puerto, pero es informativo para la configuración del contenedor. 

CMD: Define el comando predeterminado que se ejecutará cuando un contenedor se inicie a partir de la imagen. Solo puede haber una instrucción CMD; si hay varias, solo la última se ejecutará.

ENTRYPOINT: Similar a CMD, pero se usa para definir un comando fijo que siempre se ejecutará cuando se inicie un contenedor. A menudo se combina con CMD para pasar argumentos al comando.


A partir del código https://github.com/ingsoft3ucc/SimpleWebAPI crearemos una imagen.
Clonar repo
Crear imagen etiquetándola con un nombre. El punto final le indica a Docker que use el dir actual
docker build -t mywebapi .
Revisar Dockerfile y explicar cada línea
Ver imágenes disponibles
Ejecutar un contenedor con nuestra imagen
Subir imagen a nuestra cuenta de dockerhub
7.1 Inicia sesión en Docker Hub
Primero, asegúrate de estar autenticado en Docker Hub desde tu terminal:
docker login
7.2 Etiquetar la imagen a subir con tu nombre de usuario de Docker Hub y el nombre de la imagen. Por ejemplo:
docker tag <nombre_imagen_local> <tu_usuario_dockerhub>/<nombre_imagen>:<tag>
7.3 Subir la Imagen
Para subir la imagen etiquetada a Docker Hub, utiliza el comando docker push:
docker push <tu_usuario_dockerhub>/<nombre_imagen>:<tag>
7.4 Verificar la Subida
docker pull <tu_usuario_dockerhub>/<nombre_imagen>:<tag>
# 8- Publicando puertos
En el caso de aplicaciones web o base de datos donde se interactúa con estas aplicaciones a través de un puerto al cual hay que acceder, estos puertos están visibles solo dentro del contenedor. Si queremos acceder desde el exterior deberemos exponerlos.

Ejecutar la siguiente imagen, en este caso utilizamos la bandera -d (detach) para que nos devuelva el control de la consola:
docker run --name myapi -d mywebapi
Ejecutamos un comando ps:

Vemos que el contendor expone 3 puertos el 80, el 5254 y el 443, pero si intentamos en un navegador acceder a http://localhost/WeatherForecast no sucede nada.

Procedemos entonces a parar y remover este contenedor:

docker kill myapi
docker rm myapi
Vamos a volver a correrlo otra vez, pero publicando el puerto 80
docker run --name myapi -d -p 80:80 mywebapi
Accedamos nuevamente a http://localhost/WeatherForecast y vemos que nos devuelve datos.
# 9- Modificar Dockerfile para soportar bash
Modificamos dockerfile para que entre en bash sin ejecutar automaticamente la app
#ENTRYPOINT ["dotnet", "SimpleWebAPI.dll"]
CMD ["/bin/bash"]
Rehacemos la imagen
docker build -t mywebapi .
Corremos contenedor en modo interactivo exponiendo puerto
docker run -it --rm -p 80:80 mywebapi
Navegamos a http://localhost/weatherforecast
Vemos que no se ejecuta automaticamente
Ejecutamos app:
dotnet SimpleWebAPI.dll
-Volvemos a navegar a http://localhost/weatherforecast

Salimos del contenedor
# 10- Montando volúmenes
Hasta este punto los contenedores ejecutados no tenían contacto con el exterior, ellos corrían en su propio entorno hasta que terminaran su ejecución. Ahora veremos cómo montar un volumen dentro del contenedor para visualizar por ejemplo archivos del sistema huésped:

Ejecutar el siguiente comando, cambiar myusuario por el usuario que corresponda. En Mac puede utilizarse /Users/miusuario/temp):
docker run -it --rm -p 80:80 -v /Users/miuser/temp:/var/temp  mywebapi
Dentro del contenedor correr
ls -l /var/temp
touch /var/temp/hola.txt
Verificar que el Archivo se ha creado en el directorio del guest y del host.
# 11- Utilizando una base de datos
Levantar una base de datos PostgreSQL
mkdir $HOME/.postgres

docker run --name my-postgres -e POSTGRES_PASSWORD=mysecretpassword -v $HOME/.postgres:/var/lib/postgresql/data -p 5432:5432 -d postgres:9.4
Ejecutar sentencias utilizando esta instancia
docker exec -it my-postgres /bin/bash

psql -h localhost -U postgres

#Estos comandos se corren una vez conectados a la base

\l
create database test;
\connect test
create table tabla_a (mensaje varchar(50));
insert into tabla_a (mensaje) values('Hola mundo!');
select * from tabla_a;

\q

exit
Conectarse a la base utilizando alguna IDE (Dbeaver - https://dbeaver.io/, Azure DataStudio -https://azure.microsoft.com/es-es/products/data-studio, etc). Interactuar con los objectos objectos creados.

Explicar que se logro con el comando docker run y docker exec ejecutados en este ejercicio.

# 12- Hacer el punto 11 con Microsoft SQL Server
Armar un contenedor con SQL Server
Crear BD, Tablas y ejecutar SELECT
# 13- Presentación del trabajo práctico.
Subir un archivo md (puede ser en una carpeta) trabajo-practico-02 con las salidas de los comandos utilizados. Si es necesario incluir también capturas de pantalla.
