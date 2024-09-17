## Trabajo Práctico 5 - Despliegue de aplicaciones con Azure Devops Release Pipelines

### 1- Objetivos de Aprendizaje
 - Adquirir conocimientos acerca de las herramientas de despliegue y releases de aplicaciones.
 - Configurar este tipo de herramientas.
 - Comprender el concepto de recurso en Azure
 - Comprender los conceptos básicos de Release Pipelines en Azure DevOps.
 - Configurar un Release Pipeline para automatizar despliegues en diferentes entornos-

### 2- Unidad temática que incluye este trabajo práctico
Este trabajo práctico corresponde a la unidad Nº: 3 (Libro Continuous Delivery: Cap 10)

### 3- Consignas a desarrollar en el trabajo práctico:
 - Los despliegues (deployments) de aplicaciones se pueden realizar en diferentes tipos de entornos
   - On-Premise (internos) es decir en servidores propios.
   - Nubes Públicas, ejemplo AWS, Azure, Gcloud, etc.
   - Plataformas como servicios (PaaS), ejemplo Heroku, Google App Engine, AWS, Azure WebApp, etc
 - En este práctico haremos despliegue a Plataforma como Servicio utilizando Azure Web Apps

### 4- Desarrollo:
4.1\. Crear una cuenta en Azure
![Descripción de la imagen](imagen1.png)

4.2\. Crear un recurso Web App en Azure Portal y navegar a la url provista
![Descripción de la imagen](imagen2.png)
![Descripción de la imagen](imagen3.png)
![Descripción de la imagen](imagen4.png)
![Descripción de la imagen](imagen5.png)

4.3\. Actualizar Pipeline de Build para que use tareas de DotNetCoreCLI@2 como en el pipeline clásico, luego crear un Pipeline de Release en Azure DevOps con CD habilitada

Actualizamos el Pipeline de Build:

![Descripción de la imagen](imagen6.png)

Luego creamos un Pipeline de Release:

![Descripción de la imagen](imagen7.png)
![Descripción de la imagen](imagen8.png)
![Descripción de la imagen](imagen9.png)
![Descripción de la imagen](imagen10.png)
![Descripción de la imagen](imagen11.png)

4.4\. Optimizar Pipeline de Build

![Descripción de la imagen](imagen12.png)
![Descripción de la imagen](imagen13.png)

4.5\. Verificar el deploy en la url de la WebApp /weatherforecast
![Descripción de la imagen](imagen14.png)

4.6\. Realizar un cambio al código del controlador para que devuelva 7 pronósticos, realizar commit, evaluar ejecución de pipelines de build y release, navegar a la url de la webapp/weatherforecast y corroborar cambio

Primero realizamos el cambio para que devuelva 7 pronosticos, yrealizamos el commit :

![Descripción de la imagen](imagen15.png)

Vemos que se ejecuta el pipeline de build y release:

![Descripción de la imagen](imagen16.png)
![Descripción de la imagen](imagen17.png)

Corroboramos los cambios:

![Descripción de la imagen](imagen18.png)

4.7\. Clonar la Web App de QA para que contar con una WebApp de PROD a partir de un Template Deployment en Azure Portal y navegar a la url provista para la WebApp de PROD.


![Descripción de la imagen](imagen19.png)
![Descripción de la imagen](imagen20.png)
![Descripción de la imagen](imagen21.png)
![Descripción de la imagen](imagen22.png)
![Descripción de la imagen](imagen23.png)
![Descripción de la imagen](imagen24.png)

Verificamos que se haya clonado:

![Descripción de la imagen](imagen25.png)

4.8\. Agregar una etapa de Deploy a Prod en Azure Release Pipelines 

![Descripción de la imagen](imagen26.png)
![Descripción de la imagen](imagen27.png)
![Descripción de la imagen](imagen28.png)

4.9\.  Realizar un cambio al código del controlador para que devuelva 10 pronósticos, realizar commit, evaluar ejecución de pipelines de build y release, navegar a la url de la webapp/weatherforecast y corroborar cambio, verificar que en la url de la webapp_prod/weatherforecast se muestra lo mismo.

Realizamos cambio para que devuelva 10 pronosticos:

![Descripción de la imagen](imagen29.png)

Realizamos el commit:

![Descripción de la imagen](imagen30.png)

Evaluamos ejecución de pipeline de build y de reléase:

![Descripción de la imagen](imagen31.png)
![Descripción de la imagen](imagen32.png)

Corroboramos cambios:

![Descripción de la imagen](imagen33.png)


4.10\. Modificar pipeline de release para colocar una aprobación manual para el paso a Producción.

![Descripción de la imagen](imagen34.png)
![Descripción de la imagen](imagen35.png)
![Descripción de la imagen](imagen36.png)

4.11\. Realizar un cambio al código del controlador para que devuelva 5 pronósticos, realizar commit, evaluar ejecución de pipelines de build y release, navegar a la url de la webapp/weatherforecast y corroborar cambio, verificar que en la url de la webapp_prod/weatherforecast aun se muestra la versión anterior.

Realizamos cambio para que devuelva 5 pronosticos:

![Descripción de la imagen](imagen37.png)

Realizamos commit:

![Descripción de la imagen](imagen38.png)

Evaluamos ejecución de pipelines de build y release:

![Descripción de la imagen](imagen39.png)

Corroboramos que se realizo el cambio
![Descripción de la imagen](imagen40.png)

Verificamos que en la WebApp-PROD no se realizo el cambio:
![Descripción de la imagen](imagen41.png)

4.12\. Aprobar el pase ya sea desde el release o desde el mail recibido. 

![Descripción de la imagen](imagen42.png)

4.12.1\. Notar que se puede dar la aprobación pero posponer su aplicación hasta una determinada fecha

4.13\. Esperar a la finalización de la etapa de Pase a Prod y luego corroborar que en la url de la webapp_prod/weatherforecast se muestra la nueva versión coinicidente con la de QA.

![Descripción de la imagen](imagen43.png)

Corroboramos que se realizo el cambio:

![Descripción de la imagen](imagen44.png)


4.14\. Realizar un pipeline (no release) que incluya el deploy a QA y a PROD con una aprobación manual. El pipeline debe estar construido en YAML sin utilizar el editor clásico de pipelines ni el editor clásico de pipelines de release.

![Descripción de la imagen](imagen45.png)
![Descripción de la imagen](imagen46.png)
![Descripción de la imagen](imagen47.png)
![Descripción de la imagen](imagen48.png)
![Descripción de la imagen](imagen49.png)
![Descripción de la imagen](imagen50.png)

 
