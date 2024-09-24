## Trabajo Pr�ctico 6 - Pruebas Unitarias

### 4- Desarrollo:

 

#### 4.1 Creaci�n de una BD SQL Server para nuestra App


A\. Crear una BD Azure SQL Database (Ver Instructivo 5.1)

![Descripci�n de la imagen](imagen1.png)
![Descripci�n de la imagen](imagen2.png)
![Descripci�n de la imagen](imagen3.png)
![Descripci�n de la imagen](imagen4.png)
![Descripci�n de la imagen](imagen5.png)
![Descripci�n de la imagen](imagen6.png)
![Descripci�n de la imagen](imagen7.png)
![Descripci�n de la imagen](imagen8.png)
![Descripci�n de la imagen](imagen9.png)
![Descripci�n de la imagen](imagen10.png)
![Descripci�n de la imagen](imagen11.png)
![Descripci�n de la imagen](imagen12.png)
![Descripci�n de la imagen](imagen13.png)





#### 4.2 Obtener nuestra App
A\. Clonar el repo https://github.com/ingsoft3ucc/Angular_WebAPINetCore8_CRUD_Sample.git

B\. Seguir las instrucciones del README.md del repo clonado prestando atenci�n a la modificaci�n de la cadena de conexi�n en el appSettings.json para que apunte a la BD creada en 4.1 

![Descripci�n de la imagen](imagen14.png)
![Descripci�n de la imagen](imagen15.png)


C\. Navegar a http://localhost:7150/swagger/index.html y probar uno de los controladores para verificar el correcto funcionamiento de la API.

![Descripci�n de la imagen](imagen16.png)


D\. Navegar a http://localhost:4200 y verificar el correcto funcionamiento de nuestro front-end Angular


![Descripci�n de la imagen](imagen17.png)
![Descripci�n de la imagen](imagen18.png)

E\. Una vez verificado el correcto funcionamiento de la Aplicaci�n procederemos a crear un proyecto de pruebas unitarias para nuestra API.

![Descripci�n de la imagen](imagen19.png)
![Descripci�n de la imagen](imagen20.png)
![Descripci�n de la imagen](imagen21.png)


#### 4.3 Crear Pruebas Unitarias para nuestra API
A\. En el directorio raiz de nuestro repo crear un nuevo proyecto de pruebas unitarias para nuestra API 
```bash
dotnet new xunit -n EmployeeCrudApi.Tests
```

![Descripci�n de la imagen](imagen22.png)


B\. Instalar dependencias necesarias

Primero, instala las siguientes bibliotecas mediante NuGet:


```bash
cd EmployeeCrudApi.Tests 
dotnet add package Moq
dotnet add package xunit
dotnet add package Microsoft.EntityFrameworkCore.InMemory

```

![Descripci�n de la imagen](imagen23.png)
![Descripci�n de la imagen](imagen24.png)
![Descripci�n de la imagen](imagen25.png)


C\. Editar archivo UnitTest1.cs reemplazando su contenido por


![Descripci�n de la imagen](imagen26.png)


D\. Renombrar archivo UnitTest1.cs por EmployeeControllerUnitTests.cs
```bash
mv UnitTest1.cs EmployeeControllerUnitTests.cs 
```

![Descripci�n de la imagen](imagen27.png)


E\. Editar el archivo EmployeeCrudApi.Tests/EmployeeCrudApi.Tests.csproj para agregar una referencia a nuestro proyecto de EmployeeCrudApi reemplazando su contenido por


![Descripci�n de la imagen](imagen28.png)



F\. Ejecutar los siguientes comandos para ejecutar nuestras pruebas
G\. Verificar que se hayan ejecutado correctamente las pruebas


```bash
dotnet build
![Descripci�n de la imagen](imagen29.png)

dotnet test
![Descripci�n de la imagen](imagen30.png)

```



H\. Verificar que no estamos usando una dependencia externa como la base de datos.

I\. Modificar la cadena de conexi�n en el archivo appsettings.json para que use un usuario o password incorrecto y recompilar el proyecto EmployeeCrudApi

![Descripci�n de la imagen](imagen31.png)

```bash
dotnet build
dotnet run --urls "http://localhost:7150"
```
J\. Verificar que nuestro proyecto ya no tiene acceso a la BD navegando a http://localhost:7150/swagger/index.html y probando uno de los controladores:


![Descripci�n de la imagen](imagen32.png)

K\. En la carpeta de nuestro proyecto EmployeeCrudApi.Tests volver a correr las pruebas
L\. Verificar que se hayan ejecutado correctamente las pruebas inclusive sin tener acceso a la BD, lo que confirma que es efectivamente un conjunto de pruebas unitarias que no requieren de una dependencia externa para funcionar.

```bash
dotnet build
![Descripci�n de la imagen](imagen33.png)

dotnet test
![Descripci�n de la imagen](imagen34.png)

```


M\. Modificar la cadena de conexi�n en el archivo appsettings.json para que use el usuario y password correcto y recompilar el proyecto EmployeeCrudApi


```bash
dotnet build
dotnet run --urls "http://localhost:7150"
```


![Descripci�n de la imagen](imagen35.png)

N\. Verificar que nuestro proyecto vuelve a tener acceso a la BD navegando a http://localhost:7150/swagger/index.html y probando uno de los controladores:

![Descripci�n de la imagen](imagen36.png)

#### 4.4 Creamos pruebas unitarias para nuestro front de Angular:

A\. Nos posicionamos en nuestro proyecto de front, en el directorio EmployeeCrudAngular/src/app
```bash
pwd
```
![Descripci�n de la imagen](imagen37.png)

B\. Editamos el archivo app.component.spec.ts reemplazando su contenido por:

![Descripci�n de la imagen](imagen38.png)


C\. Creamos el archivo employee.service.spec.ts reemplazando su contenido por:

![Descripci�n de la imagen](imagen39.png)
![Descripci�n de la imagen](imagen40.png)


D\. Editamos el archivo employee.component.spec.ts ubicado en la carpeta **employee** reemplazando su contenido por:

![Descripci�n de la imagen](imagen41.png)



E\. Editamos el archivo addemployee.component.spec.ts ubicado en la carpeta **addemployee** reemplazando su contenido por:

![Descripci�n de la imagen](imagen42.png)


F\. En el directorio raiz de nuestro proyecto EmployeeCrudAngular ejecutamos el comando 
```bash
ng test
```
En proyectos de Angular, Jasmine se usa para escribir las pruebas, y Karma se encarga de ejecutarlas. Cuando ejecutamos el comando ng test, Karma se inicia, carga las pruebas escritas en Jasmine, y las ejecuta en un navegador.

G\. Vemos que se abre una ventana de Karma con Jasmine en la que nos indica que los tests se ejecutaron correctamente

![Descripci�n de la imagen](imagen43.png)


H\. Vemos que los tests se ejecutaron correctamente:

![Descripci�n de la imagen](imagen44.png)


I\. Verificamos que no est� corriendo nuestra API navegando a http://localhost:7150/swagger/index.html y recibiendo esta salida:


![Descripci�n de la imagen](imagen45.png)


J\. Los puntos G y H nos indican que se han ejecutado correctamente las pruebas inclusive sin tener acceso a la API, lo que confirma que es efectivamente un conjunto de pruebas unitarias que no requieres de una dependencia externa para funcionar.

#### 4.5 Agregamos generaci�n de reporte XML de nuestras pruebas de front.
Para cuando integremos nuestras pruebas en un pipeline de Build, vamos a necesitar el resultado devuelto por nuestras pruebas para reportarlas junto a las pruebas de back que se reportan automaticamente. 

![image](https://github.com/user-attachments/assets/12c430fd-13e7-4370-8c2a-a2f2756bd33f)


Haremos los siguientes pasos para prepararnos:

A\. Instalamos dependencia karma-junit-reporter
```bash
npm install karma-junit-reporter --save-dev
```

![Descripci�n de la imagen](imagen46.png)

B\. En el directorio raiz de nuestro proyecto (al mismo nivel que el archivo angular.json) creamos un archivo karma.conf.js con el siguiente contenido

![Descripci�n de la imagen](imagen47.png)
![Descripci�n de la imagen](imagen48.png)



C\. Ejecutamos nuestros test de la siguiente manera:
```bash
ng test --karma-config=karma.conf.js --watch=false --browsers ChromeHeadless
```

![Descripci�n de la imagen](imagen49.png)

D\. Verificamos que se creo un archivo test-result.xml en el directorio test-results que est� al mismo nivel que el directorio src

![Descripci�n de la imagen](imagen50.png)



#### 4.6 Modificamos el c�digo de nuestra API y creamos nuevas pruebas unitarias:

A\. Realizar al menos 5 de las siguientes modificaciones sugeridas al c�digo de la API:
  - Al agregar y al editar un empleado, controlar que el nombre del empleado no est� repetido.
  - La longitud m�xima del nombre y apellido del empleado debe ser de 100 caracteres.
  - Almacenar el nombre en la BD siempre con la primera letra de los nombres en Mayuscula y todo el apellido en Mayusculas. Ejemplo, si recibo juan carlos chamizo, se debe almacenar como Juan Carlos CHAMIZO.
  - Asegurar que el nombre del empleado no contenga caracteres especiales o n�meros, a menos que sea necesario (por ejemplo, caracteres especiales en apellidos como "O'Connor" o "Garc�a").
  - Validar que el nombre tenga un n�mero m�nimo de caracteres, por ejemplo, al menos dos caracteres para evitar entradas inv�lidas como "A".
  - Verificar que el nombre no contenga n�meros, ya que no es com�n en los nombres de empleados.
  - Asegurar que cada parte del nombre (separada por espacios) tenga al menos un car�cter o m�s, por ejemplo, para evitar "A B".
  - Verificar que no haya palabras vac�as o que el nombre no est� compuesto solo de espacios.
  - Prohibir el uso de nombres triviales o gen�ricos como "Empleado", "N/A", "Nombre", etc.
  - Evitar que se ingresen caracteres repetidos de forma excesiva, como "Juuuuaannnn".
  - Implementar un filtro para evitar el uso de palabras inapropiadas, ofensivas o que puedan violar pol�ticas internas.
En todos los casos donde no se cumplan las condiciones, la API debe devolver un error de HTTP 400 Bad Request y un Json indicando el error, por ejemplo:
```json
{
  "status": 400,
  "error": "Bad Request",
  "message": "El nombre del empleado ya existe."
}
```

B\. Crear las pruebas unitarias necesarias para validar las modificaciones realizadas en el c�digo

#### 4.7 Modificamos el c�digo de nuestro Front y creamos nuevas pruebas unitarias:

A\. Realizar en el c�digo del front las mismas modificaciones hechas a la API. 

B\. Las validaciones deben ser realizadas en el front sin llegar a la API, y deben ser mostradas en un toast como por ejemplo https://stackblitz.com/edit/angular12-toastr?file=src%2Fapp%2Fapp.component.ts o https://stackblitz.com/edit/angular-error-toast?file=src%2Fapp%2Fcore%2Frxjsops.ts

C\. Crear las pruebas unitarias necesarias en el front para validar las modificaciones realizadas en el c�digo del front.


