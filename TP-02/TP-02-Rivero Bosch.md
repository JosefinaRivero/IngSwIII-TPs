### RIVERO BOSCH JOSEFINA


# 4- Desarrollo:

# 1- Instalar Docker Community Edition
Diferentes opciones para cada sistema operativo
https://docs.docker.com/
Ejecutar el siguiente comando para comprobar versiones de cliente y demonio.

docker versi髇
![Descripci髇 de la imagen](imagen15.jpg)


# 2- Explorar DockerHub
Registrase en docker hub: https://hub.docker.com/
Familiarizarse con el portal

# 3- Obtener la imagen BusyBox
Ejecutar el siguiente comando, para bajar una imagen de DockerHub

docker pull busybox

![Descripci贸n de la imagen](imagen1.jpg)

Verificar qu茅 versi贸n y tama帽o tiene la imagen bajada, obtener una lista de im谩genes locales:

docker images
![Descripci贸n de la imagen](imagen2.jpg)

# 4- Ejecutando contenedores
Ejecutar un contenedor utilizando el comando run de docker:

docker run busybox

![Descripci贸n de la imagen](imagen3.jpg)


Explicar porque no se obtuvo ning鷑 resultado:

El comando docker run busybox ejecuta un contenedor basado en la imagen de busybox, pero no especifica un comando para ejecutar dentro del contenedor. busybox es una imagen extremadamente ligera que contiene un conjunto b醩ico de utilidades de Unix. Por defecto, cuando se ejecuta sin un comando espec韋ico, el contenedor simplemente inicia y finaliza inmediatamente, ya que no tiene un proceso que mantener en ejecuci髇.

Especificamos alg煤n comando a correr dentro del contenedor, ejecutar por ejemplo:

docker run busybox echo "Hola Mundo"
![Descripci贸n de la imagen](imagen4.jpg)

Ver los contenedores ejecutados utilizando el comando ps:

docker ps
![Descripci贸n de la imagen](imagen5.jpg)

Vemos que no existe nada en ejecuci贸n, correr entonces:

docker ps -a
![Descripci贸n de la imagen](imagen6.jpg)

Mostrar el resultado y explicar que se obtuvo como salida del comando anterior.

Este comando lista todos los contenedores, tanto los que est醤 actualmente en ejecuci髇 como los que se han detenido.

En la salida se puede observar lo siguiente:

CONTAINER ID: 0bf82d4df2da es el identificador del contenedor.

IMAGE: busybox, que es la imagen de Docker utilizada para crear el contenedor.

COMMAND: "echo 'Hola Mundo'", es el comando que se ejecut� dentro del contenedor.

CREATED: 29 seconds ago, indica que el contenedor fue creado hace 29 segundos.

STATUS: Exited (0) 28 seconds ago, indica que el contenedor se ejecut� y luego termin� su ejecuci髇 (con un c骴igo de salida 0, lo que significa que se ejecut� correctamente) hace 28 segundos.

NAMES: affectionate_tesla, es el nombre asignado autom醫icamente al contenedor.



# 5- Ejecutando en modo interactivo

Ejecutar el siguiente comando
docker run -it busybox sh
![Descripci贸n de la imagen](imagen7.jpg)

Para cada uno de los siguientes comandos dentro de contenedor, mostrar los resultados:
ps
![Descripci贸n de la imagen](imagen8.jpg)

uptime
![Descripci贸n de la imagen](imagen9.jpg)

free
![Descripci贸n de la imagen](imagen10.jpg)

ls -l /
![Descripci贸n de la imagen](imagen11.jpg)

Salimos del contenedor con:
exit

# 6- Borrando contenedores terminados
Obtener la lista de contenedores
docker ps -a
![Descripci贸n de la imagen](imagen12.jpg)

Para borrar podemos utilizar el id o el nombre (autogenerado si no se especifica) de contenedor que se desee, por ejemplo:
docker rm elated_lalande
![Descripci贸n de la imagen](imagen13.jpg)

Para borrar todos los contenedores que no est茅n corriendo, ejecutar cualquiera de los siguientes comandos:
docker rm $(docker ps -a -q -f status=exited)
docker container prune
![Descripci贸n de la imagen](imagen14.jpg)

# 7- Construir una imagen
Conceptos de DockerFile
Leer https://docs.docker.com/engine/reference/builder/
Describir las instrucciones

FROM: Especifica la imagen base a partir de la cual se construir� la nueva imagen. Es la primera instrucci髇 en un Dockerfile y establece el entorno sobre el cual se realizar醤 las modificaciones.

RUN: Ejecuta un comando en el contenedor durante la construcci髇 de la imagen. Es com鷑 usar RUN para instalar paquetes o realizar configuraciones.

ADD: Copia archivos y directorios desde la m醧uina host al contenedor. Adem醩 de copiar, ADD puede descomprimir archivos tar autom醫icamente y descargar archivos desde URLs.

COPY: Similar a ADD, pero solo copia archivos y directorios desde la m醧uina host al contenedor sin realizar ninguna acci髇 adicional como descomprimir. Se usa generalmente para copiar archivos de la aplicaci髇 al contenedor.

EXPOSE: Indica el puerto en el que el contenedor escuchar� las conexiones en tiempo de ejecuci髇. Esto no expone el puerto fuera del contenedor, sino que es una forma de documentar que el contenedor utiliza ese puerto.

CMD: Define el comando que se ejecutar� por defecto cuando se inicie un contenedor a partir de la imagen. Puede ser sobrescrito en el momento de ejecutar el contenedor. Se puede usar una cadena ejecutable o una lista de argumentos.

ENTRYPOINT: Establece el comando que se ejecutar� cuando se inicie el contenedor. A diferencia de CMD, no se puede sobrescribir en el momento de ejecutar el contenedor. ENTRYPOINT define el proceso principal del contenedor.


A partir del c骴igo https://github.com/ingsoft3ucc/SimpleWebAPI crearemos una imagen.

Clonar repo
![Descripci髇 de la imagen](imagen16.jpg)

Crear imagen etiquet醤dola con un nombre. El punto final le indica a Docker que use el dir actual
docker build -t mywebapi .
![Descripci髇 de la imagen](imagen17.jpg)

Revisar Dockerfile y explicar cada l韓ea

#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.:  proporciona un enlace para entender c髆o Visual Studio utiliza el Dockerfile para construir im醙enes de manera m醩 eficiente durante la depuraci髇.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base :  Establece la imagen base para la aplicaci髇, que en este caso es mcr.microsoft.com/dotnet/aspnet:7.0, una imagen oficial de .NET ASP.NET Core Runtime. Esta imagen est� optimizada para ejecutar aplicaciones ASP.NET Core.

WORKDIR /app : Establece el directorio de trabajo dentro del contenedor en /app. Cualquier comando subsiguiente se ejecutar� desde este directorio.

EXPOSE 80
EXPOSE 443
EXPOSE 5254 :Documenta que la aplicaci髇 escucha en los puertos 80 (HTTP), 443 (HTTPS) y 5254. Esto no expone los puertos fuera del contenedor; solo documenta que estos puertos est醤 en uso dentro del contenedor. Puedes mapear estos puertos al host cuando ejecutes el contenedor.

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build : Cambia a una imagen diferente, mcr.microsoft.com/dotnet/sdk:7.0, que incluye el SDK de .NET 7.0. Esta imagen contiene todas las herramientas necesarias para construir y compilar aplicaciones .NET.

WORKDIR /src : Cambia el directorio de trabajo a /src, donde se llevar� a cabo el proceso de construcci髇 de la aplicaci髇

COPY ["SimpleWebAPI/SimpleWebAPI.csproj", "SimpleWebAPI/"] : Copia el archivo de proyecto SimpleWebAPI.csproj desde tu m醧uina local al contenedor, ubic醤dolo en el directorio /src/SimpleWebAPI. Esto se hace para que las dependencias se restauren bas醤dose en este archivo.

RUN dotnet restore "SimpleWebAPI/SimpleWebAPI.csproj" : Ejecuta el comando dotnet restore, que restaura todas las dependencias y paquetes NuGet definidos en SimpleWebAPI.csproj

COPY . . : Copia todos los archivos y carpetas desde el contexto de construcci髇 local al directorio de trabajo actual del contenedor (/src). Esto incluye todo el c骴igo fuente de la aplicaci髇

WORKDIR "/src/SimpleWebAPI" : Cambia el directorio de trabajo al de la aplicaci髇 dentro del contenedor (/src/SimpleWebAPI), donde se encuentra el c骴igo de la aplicaci髇.

RUN dotnet build "SimpleWebAPI.csproj" -c Release -o /app/build : Compila la aplicaci髇 en modo de lanzamiento (Release) y coloca los archivos compilados en el directorio /app/build.


FROM build AS publish: Utiliza la imagen de la etapa build como base para la etapa publish, donde se publica la aplicaci髇.

RUN dotnet publish "SimpleWebAPI.csproj" -c Release -o /app/publish /p:UseAppHost=false: Publica la aplicaci髇 compilada, preparando los archivos para su despliegue. Coloca los archivos resultantes en el directorio /app/publish. El par醡etro /p:UseAppHost=false indica que no se debe generar un ejecutable nativo espec韋ico de la plataforma, lo cual es 鷗il para reducir el tama駉 de la imagen final.

FROM base AS final : Cambia a la imagen base definida al principio (base), que es la imagen de ASP.NET Core Runtime. Esto es para minimizar el tama駉 de la imagen final, ya que solo contiene lo necesario para ejecutar la aplicaci髇, no las herramientas de desarrollo

WORKDIR /app: Establece nuevamente el directorio de trabajo en /app.

COPY --from=publish /app/publish . : Copia los archivos publicados desde la etapa publish a la imagen final, coloc醤dolos en el directorio /app

ENTRYPOINT ["dotnet", "SimpleWebAPI.dll"] : Define el comando que se ejecutar� cuando se inicie el contenedor. En este caso, se ejecuta dotnet SimpleWebAPI.dll, lo que lanza la aplicaci髇 ASP.NET Core.

#CMD ["/bin/bash"] :  Esta l韓ea est� comentada. Si estuviera activa, sobrescribir韆 el ENTRYPOINT anterior con el comando /bin/bash, que abrir韆 una sesi髇 de terminal bash en lugar de ejecutar la aplicaci髇. Esto es 鷗il para depuraci髇, pero generalmente no se deja activo en una imagen de producci髇.





Ver im醙enes disponibles
![Descripci髇 de la imagen](imagen18.jpg)

Ejecutar un contenedor con nuestra imagen
Subir imagen a nuestra cuenta de dockerhub
7.1 Inicia sesi贸n en Docker Hub
Primero, aseg煤rate de estar autenticado en Docker Hub desde tu terminal:
docker login
7.2 Etiquetar la imagen a subir con tu nombre de usuario de Docker Hub y el nombre de la imagen. Por ejemplo:
docker tag <nombre_imagen_local> <tu_usuario_dockerhub>/<nombre_imagen>:<tag>

![Descripci髇 de la imagen](imagen19.jpg)


7.3 Subir la Imagen
Para subir la imagen etiquetada a Docker Hub, utiliza el comando docker push:
docker push <tu_usuario_dockerhub>/<nombre_imagen>:<tag>

![Descripci髇 de la imagen](imagen20.jpg)

7.4 Verificar la Subida
docker pull <tu_usuario_dockerhub>/<nombre_imagen>:<tag>

![Descripci髇 de la imagen](imagen21.jpg)


# 8- Publicando puertos
En el caso de aplicaciones web o base de datos donde se interact煤a con estas aplicaciones a trav茅s de un puerto al cual hay que acceder, estos puertos est谩n visibles solo dentro del contenedor. Si queremos acceder desde el exterior deberemos exponerlos.

Ejecutar la siguiente imagen, en este caso utilizamos la bandera -d (detach) para que nos devuelva el control de la consola:
docker run --name myapi -d mywebapi

![Descripci髇 de la imagen](imagen22.jpg)


Ejecutamos un comando ps:

Vemos que el contendor expone 3 puertos el 80, el 5254 y el 443, pero si intentamos en un navegador acceder a http://localhost/WeatherForecast no sucede nada.
![Descripci髇 de la imagen](imagen23.jpg)


Procedemos entonces a parar y remover este contenedor:

docker kill myapi
docker rm myapi

![Descripci髇 de la imagen](imagen24.jpg)

Vamos a volver a correrlo otra vez, pero publicando el puerto 80
docker run --name myapi -d -p 80:80 mywebapi

![Descripci髇 de la imagen](imagen25.jpg)

Accedamos nuevamente a http://localhost/WeatherForecast y vemos que nos devuelve datos.

![Descripci髇 de la imagen](imagen26.jpg)

# 9- Modificar Dockerfile para soportar bash
Modificamos dockerfile para que entre en bash sin ejecutar automaticamente la app
#ENTRYPOINT ["dotnet", "SimpleWebAPI.dll"]
CMD ["/bin/bash"]
Rehacemos la imagen
docker build -t mywebapi .

![Descripci髇 de la imagen](imagen27.jpg)

Corremos contenedor en modo interactivo exponiendo puerto
docker run -it --rm -p 80:80 mywebapi



Navegamos a http://localhost/weatherforecast
Vemos que no se ejecuta automaticamente
Ejecutamos app:
dotnet SimpleWebAPI.dll

![Descripci髇 de la imagen](imagen28.jpg)

-Volvemos a navegar a http://localhost/weatherforecast

![Descripci髇 de la imagen](imagen29.jpg)

Salimos del contenedor



# 10- Montando vol鷐enes
Hasta este punto los contenedores ejecutados no ten韆n contacto con el exterior, ellos corr韆n en su propio entorno hasta que terminaran su ejecuci髇. Ahora veremos c髆o montar un volumen dentro del contenedor para visualizar por ejemplo archivos del sistema hu閟ped:

Ejecutar el siguiente comando, cambiar myusuario por el usuario que corresponda. En Mac puede utilizarse /Users/miusuario/temp):
docker run -it --rm -p 80:80 -v /Users/miuser/temp:/var/temp  mywebapi
Dentro del contenedor correr
ls -l /var/temp

![Descripci髇 de la imagen](imagen30.jpg)

touch /var/temp/hola.txt

![Descripci髇 de la imagen](imagen31.jpg)


Verificar que el Archivo se ha creado en el directorio del guest y del host.

![Descripci髇 de la imagen](imagen32.jpg)

# 11- Utilizando una base de datos
Levantar una base de datos PostgreSQL
mkdir $HOME/.postgres


![Descripci髇 de la imagen](imagen33.jpg)

docker run --name my-postgres -e POSTGRES_PASSWORD=mysecretpassword -v $HOME/.postgres:/var/lib/postgresql/data -p 5432:5432 -d postgres:9.4

![Descripci髇 de la imagen](imagen34.jpg)

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

![Descripci髇 de la imagen](imagen35.jpg)
![Descripci髇 de la imagen](imagen36.jpg)



exit
Conectarse a la base utilizando alguna IDE (Dbeaver - https://dbeaver.io/, Azure DataStudio -https://azure.microsoft.com/es-es/products/data-studio, etc). Interactuar con los objectos objectos creados.

![Descripci髇 de la imagen](imagen37.jpg)



![Descripci髇 de la imagen](imagen38.jpg)


Explicar que se logro con el comando docker run y docker exec ejecutados en este ejercicio.

Docker run: Este comando se usa para crear y ejecutar un contenedor basado en la imagen postgres:9.4. Configura la base de datos PostgreSQL con una contrase馻, monta un volumen para persistir datos y expone el puerto necesario para acceder a la base de datos desde fuera del contenedor.


Docker exec: Permite ejecutar comandos dentro de un contenedor que ya est� en ejecuci髇. En este caso, accede a una shell dentro del contenedor para ejecutar comandos de PostgreSQL.

# 12- Hacer el punto 11 con Microsoft SQL Server
Armar un contenedor con SQL Server
Crear BD, Tablas y ejecutar SELECT

![Descripci髇 de la imagen](imagen42.jpg)
![Descripci髇 de la imagen](imagen44.jpg)
![Descripci髇 de la imagen](imagen43.jpg)

# 13- Presentaci髇 del trabajo pr醕tico.
Subir un archivo md (puede ser en una carpeta) trabajo-practico-02 con las salidas de los comandos utilizados. Si es necesario incluir tambi閚 capturas de pantalla.

