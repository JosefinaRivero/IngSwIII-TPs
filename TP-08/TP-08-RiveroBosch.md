
### 4- Desarrollo:
#### Prerequisitos:
 - Azure CLI instalado 

![Descripci�n de la imagen](imagen1.png)


#### 4.1 Modificar nuestro pipeline para construir im�genes Docker de back y front y subirlas a ACR
- Desarrollo del punto 4.1:
 
	- ##### 4.1.1 Crear archivos DockerFile para nuestros proyectos de Back y Front
   	  - En la raiz de nuestro repo crear una carpeta docker con dos subcarpetas api y front, dentro de cada una de ellas colocar los dockerfiles correspondientes para la creaci�n de im�genes docker en funci�n de la salida de nuestra etapa de Build y Test

![Descripci�n de la imagen](imagen2.png)

          - DockerFile Back:

			![Descripci�n de la imagen](imagen3.png)

          - DockerFile Front:

			![Descripci�n de la imagen](imagen4.png)

   	- ##### 4.1.2 Crear un recurso ACR en Azure Portal siguiendo el instructivo 5.1

![Descripci�n de la imagen](imagen5.png)

  	- ##### 4.1.3 Modificar nuestro pipeline en la etapa de Build y Test
   	  - Luego de la tarea de publicaci�n de los artefactos de Back agregar la tarea de publicaci�n de nuestro dockerfile de back para que est� disponible en etapas posteriores:
   	     
![Descripci�n de la imagen](imagen6.png)

   	  - Luego de la tarea de publicaci�n de los artefactos de Front agregar la tarea de publicaci�n de nuestro dockerfile de front para que est� disponible en etapas posteriores:
   	    
![Descripci�n de la imagen](imagen7.png)
![Descripci�n de la imagen](imagen8.png)


  	- ##### 4.1.4 En caso de no contar en nuestro proyecto con una ServiceConnection a Azure Portal para el manejo de recursos, agregar una service connection a Azure Resource Manager como se indica en instructivo 5.2 

![Descripci�n de la imagen](imagen9.png)


  	- ##### 4.1.5 Agregar a nuestro pipeline variables 
		

![Descripci�n de la imagen](imagen10.png)
![Descripci�n de la imagen](imagen11.png)

  	- ##### 4.1.6 Agregar a nuestro pipeline una nueva etapa que dependa de nuestra etapa de Build y Test
  	  - Agregar tareas para generar imagen Docker de Back
   	  
   	        
![Descripci�n de la imagen](imagen12.png)

  	
  	- ##### 4.1.7 - Ejecutar el pipeline y en Azure Portal acceder a la opci�n Repositorios de nuestro recurso Azure Container Registry. Verificar que exista una imagen con el nombre especificado en la variable backImageName asignada en nuestro pipeline
  	 
![Descripci�n de la imagen](imagen13.png)
![Descripci�n de la imagen](imagen14.png)

	- ##### 4.1.8 - Agregar tareas para generar imagen Docker de Front (DESAFIO)
  	  - A la etapa creada en 4.1.6 Agregar tareas para generar imagen Docker de Front

![Descripci�n de la imagen](imagen15.png)
![Descripci�n de la imagen](imagen16.png)
![Descripci�n de la imagen](imagen17.png)
![Descripci�n de la imagen](imagen18.png)

  	- ##### 4.1.9 - Agregar a nuestro pipeline una nueva etapa que dependa de nuestra etapa de Construcci�n de Imagenes Docker y subida a ACR
	  - Agregar variables a nuestro pipeline:
  	  

![Descripci�n de la imagen](imagen19.png)

  	  - Agregar variable secreta cnn-string-qa desde la GUI de ADO que apunte a nuestra BD de SQL Server de QA como se indica en el instructivo 5.3

![Descripci�n de la imagen](imagen20.png)

  	    
  	  - Agregar tareas para crear un recurso Azure Container Instances que levante un contenedor con nuestra imagen de back
  	  
![Descripci�n de la imagen](imagen21.png)
![Descripci�n de la imagen](imagen22.png)
![Descripci�n de la imagen](imagen23.png)

  	  - ##### 4.1.10 - Ejecutar el pipeline y en Azure Portal acceder al recurso de Azure Container Instances creado. Copiar la url del contenedor y navegarlo desde browser. Verificar que traiga datos.

![Descripci�n de la imagen](imagen24.png)
![Descripci�n de la imagen](imagen25.png)
![Descripci�n de la imagen](imagen26.png)
![Descripci�n de la imagen](imagen27.png)

  	  - ##### 4.1.11 - Agregar tareas para generar un recurso Azure Container Instances que levante un contenedor con nuestra imagen de front (DESAFIO)
  	  	- A la etapa creada en 4.1.9 Agregar tareas para generar contenedor en ACI con nuestra imagen de Front
  	        - Tener en cuenta que el contenedor debe recibir como variable de entorno API_URL el valor de una variable container-url-api-qa definida en nuestro pipeline.
  	        - Para que el punto anterior funcione el c�digo fuente del front debe ser modificado para que la url de la API pueda ser cambiada luego de haber sido constru�da la imagen. Se deja un ejemplo de las modificaciones a realizar en el repo https://github.com/ingsoft3ucc/CrudAngularConEnvironment.git

![Descripci�n de la imagen](imagen28.png)
![Descripci�n de la imagen](imagen29.png)
![Descripci�n de la imagen](imagen30.png)
![Descripci�n de la imagen](imagen31.png)
![Descripci�n de la imagen](imagen32.png)
![Descripci�n de la imagen](imagen33.png)
![Descripci�n de la imagen](imagen34.png)
![Descripci�n de la imagen](imagen35.png)
![Descripci�n de la imagen](imagen36.png)


  	  - ##### 4.1.12 - Agregar tareas para correr pruebas de integraci�n en el entorno de QA de Back y Front creado en ACI.
  	     
![Descripci�n de la imagen](imagen37.png)
![Descripci�n de la imagen](imagen38.png)
![Descripci�n de la imagen](imagen39.png)
![Descripci�n de la imagen](imagen40.png)

#### 4.2 Desaf�os:
- 4.2.1 Agregar tareas para generar imagen Docker de Front. (Punto 4.1.8)
- 4.2.2 Agregar tareas para generar en Azure Container Instances un contenedor de imagen Docker de Front. (Punto 4.1.11)
- 4.2.3 Agregar tareas para correr pruebas de integraci�n en el entorno de QA de Back y Front creado en ACI. (Punto 4.1.12)
- 4.2.4 Agregar etapa que dependa de la etapa de Deploy en ACI QA y genere contenedores en ACI para entorno de PROD.

![Descripci�n de la imagen](imagen41.png)
![Descripci�n de la imagen](imagen42.png)
![Descripci�n de la imagen](imagen43.png)
![Descripci�n de la imagen](imagen44.png)
![Descripci�n de la imagen](imagen45.png)
![Descripci�n de la imagen](imagen46.png)
![Descripci�n de la imagen](imagen47.png)
![Descripci�n de la imagen](imagen48.png)



