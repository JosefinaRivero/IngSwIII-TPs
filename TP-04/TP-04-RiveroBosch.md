## Trabajo Pr�ctico 4 - Azure Devops Pipelines

# 1- Objetivos de Aprendizaje
 - Adquirir conocimientos acerca de las herramientas de integraci�n continua presentes en ADO.
 - Configurar este tipo de herramientas.
 - Implementar procesos de construcci�n automatizados simples.
 
# 2- Unidad tem�tica que incluye este trabajo pr�ctico
Este trabajo pr�ctico corresponde a la unidad N�: 3 (Libro Continuous Delivery: Cap 3)


CI (Continuous Integration), Continuous Deployment y Continuous Delivery son tres pr�cticas clave en la ingenier�a de software que se utilizan para mejorar la calidad del software y acelerar el proceso de desarrollo. Aunque est�n relacionadas y a menudo se utilizan juntas, tienen diferencias clave:

**Continuous Integration (CI):**

CI es una pr�ctica que implica la integraci�n constante de cambios de c�digo en un repositorio compartido por parte de varios desarrolladores.
Los desarrolladores env�an regularmente su c�digo al repositorio compartido (a menudo varias veces al d�a).
Cada vez que se env�a c�digo, se activan pruebas autom�ticas para verificar la calidad y la compatibilidad del c�digo.
El objetivo principal de CI es detectar y resolver problemas de integraci�n de c�digo de manera temprana y continua, lo que evita la acumulaci�n de errores.

**Continuous Delivery** 
En Continuous Delivery, despu�s de que el c�digo pasa por CI y se considera estable, se automatiza el proceso de empaquetado y preparaci�n para la entrega.
Sin embargo, la entrega real al entorno de producci�n se realiza manualmente despu�s de una revisi�n y aprobaci�n humanas.

**Continuous Deployment (CD)**
Se refiere a la pr�ctica de automatizar y acelerar el proceso de entrega de software al entorno de producci�n.
En Continuous Development, una vez que el c�digo pasa por el proceso de CI y se considera estable, se automatiza la entrega del software al entorno de producci�n sin intervenci�n manual adicional.
Esto implica la automatizaci�n de tareas como pruebas adicionales, empaquetado, despliegue y configuraci�n en el entorno de producci�n.

En resumen, CI se centra en la integraci�n continua de cambios de c�digo y pruebas autom�ticas, mientras que Continuous Delivery y Continuous Deployment se centran en la automatizaci�n del proceso de entrega de software al entorno de producci�n, con Continuous Delivery deteni�ndose antes de la entrega real y Continuous Deployment automatizando todo el proceso hasta la entrega al entorno de producci�n. 
Estas pr�cticas son esenciales en el desarrollo de software �gil y permiten la entrega r�pida y confiable de software de alta calidad.

# 3- Consignas a desarrollar en el trabajo pr�ctico:

 **Azure DevOps Pipelines**
  - Breve descripci�n de Azure DevOps Pipelines.
  - Tipos de Pipelines: Build y Deploy.
  - Diferencias entre editor cl�sico y YAML.
  - Agentes MS y Self-Hosted


# 4- Pasos del TP
### - 4.1 Verificar acceso a Pipelines concedido
![Descripci�n de la imagen](imagen2.png)
![Descripci�n de la imagen](imagen1.png)

### - 4.2 Agregar en pipeline YAML una tarea de Publish. 
![Descripci�n de la imagen](imagen3.png)
![Descripci�n de la imagen](imagen4.png)
![Descripci�n de la imagen](imagen5.png)
![Descripci�n de la imagen](imagen6.png)

 ### - 4.3 Explicar por qu� es necesario contar con una tarea de Publish en un pipeline que corre en un agente de Microsoft en la nube.

La tarea de Publish es necesaria para que los artefactos generados durante el pipeline sean accesibles y puedan ser utilizados en otros contextos, y para proporcionar una mayor trazabilidad y control sobre los cambios realizados en el c�digo.


 ### - 4.4 Descargar el resultado del pipeline y correr localmente el software compilado.
![Descripci�n de la imagen](imagen7.png)
![Descripci�n de la imagen](imagen9.png)

### - 4.5 Habilitar el editor cl�sico de pipelines. Explicar las diferencias claves entre este tipo de editor y el editor YAML.
![Descripci�n de la imagen](imagen10.png)

### - 4.6 Crear un nuevo pipeline con el editor cl�sico. Descargar el resultado del pipeline y correr localmente el software compilado.
![Descripci�n de la imagen](imagen11.png)
![Descripci�n de la imagen](imagen12.png)
![Descripci�n de la imagen](imagen14.png)
![Descripci�n de la imagen](imagen15.png)
![Descripci�n de la imagen](imagen16.png)
![Descripci�n de la imagen](imagen17.png)


### - 4.7 Configurar CI en ambos pipelines (YAML y Classic Editor). Mostrar resultados de la ejecuci�n autom�tica de ambos pipelines al hacer un commit en la rama main.

Classic editor:
![Descripci�n de la imagen](imagen18.png)
![Descripci�n de la imagen](imagen19.png)

yaml:
![Descripci�n de la imagen](imagen20.png)
![Descripci�n de la imagen](imagen21.png)
![Descripci�n de la imagen](imagen22.png)
![Descripci�n de la imagen](imagen23.png)

### - 4.8 Explicar la diferencia entre un agente MS y un agente Self-Hosted. Qu� ventajas y desventajas hay entre ambos? Cu�ndo es conveniente y/o necesario usar un Self-Hosted Agent?

Diferencias entre Agente MS y Agente Self-Hosted:

Agente MS: proporcionado por Microsoft, f�cil de configurar, no requiere mantenimiento, se actualiza autom�ticamente.

Ventajas: f�cil de usar, no requiere mantenimiento.

Desventajas: no se puede personalizar, no se puede acceder a recursos locales.


Agente Self-Hosted: se ejecuta en una m�quina local o virtual, requiere configuraci�n y mantenimiento por el usuario.

Ventajas: se puede personalizar, se puede acceder a recursos locales.

Desventajas: requiere configuraci�n y mantenimiento.



Cu�ndo usar un Self-Hosted Agent?

Se requiere acceso a recursos locales o a la red local.
Se necesitan recursos espec�ficos o personalizados.
Se requiere una mayor flexibilidad y control sobre la configuraci�n y el mantenimiento del agente.

### - 4.8 Crear un Pool de Agentes y un Agente Self-Hosted
![Descripci�n de la imagen](imagen24.png)
![Descripci�n de la imagen](imagen25.png)

### - 4.9 Instalar y correr un agente en nuestra m�quina local.
![Descripci�n de la imagen](imagen26.png)
![Descripci�n de la imagen](imagen29.png)
![Descripci�n de la imagen](imagen28.png)
![Descripci�n de la imagen](imagen30.png)


### - 4.10 Crear un pipeline que use el agente Self-Hosted alojado en nuestra m�quina local.
![Descripci�n de la imagen](imagen31.png)
![Descripci�n de la imagen](imagen32.png)

### - 4.11 Buscar el resultado del pipeline y correr localmente el software compilado.
![Descripci�n de la imagen](imagen33.png)
![Descripci�n de la imagen](imagen34.png)

### - 4.12 Crear un nuevo proyecto en ADO clonado desde un repo que contenga una aplicaci�n en Angular como por ejemplo 
https://github.com/ingsoft3ucc/angular-demo-project.git
![Descripci�n de la imagen](imagen35.png)
![Descripci�n de la imagen](imagen36.png)

### - 4.13 Configurar un pipeline de build para un proyecto de tipo Angular como el clonado.
![Descripci�n de la imagen](imagen37.png)
![Descripci�n de la imagen](imagen38.png)
![Descripci�n de la imagen](imagen39.png)

### - 4.14 Habilitar CI para el pipeline.
![Descripci�n de la imagen](imagen40.png)
![Descripci�n de la imagen](imagen41.png)

### - 4.15 Hacer un cambio a un archivo del proyecto (alg�n cambio en el HTML que se renderiza por ejemplo) y verificar que se ejecute autom�ticamente el pipeline.
![Descripci�n de la imagen](imagen42.png)

### - 4.16 Descargar el resultado del pipeline y correr en un servidor web local el sitio construido.
![Descripci�n de la imagen](imagen43.png)

### - 4.17 Mostrar el antes y el despu�s del cambio.
![Descripci�n de la imagen](imagen45.png)
![Descripci�n de la imagen](imagen44.png)


#### 5- Presentaci�n del trabajo pr�ctico.
Subir un doc al repo con las capturas de pantalla de los pasos realizados y colocar en el excel de repos (https://docs.google.com/spreadsheets/d/1mZKJ8FH390QHjwkABokh3Ys6kMOFZGzZJ3-kg5ziELc/edit?gid=0#gid=0) la url del proyecto de AzureDevops.

#### 6- Criterio de Calificaci�n
Los pasos 4.1 al 4.11 representan un 60% de la nota total, los pasos 4.12 al 4.17 representan el 40% restante
