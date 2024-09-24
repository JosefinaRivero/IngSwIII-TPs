4- Desarrollo:
Prerequisitos:
Node.js
Para MAC OS:
Instalar Brew https://brew.sh/
Correr:
# NOTE:
# Homebrew is not a Node.js package manager.
# Please ensure it is already installed on your system.
# Follow official instructions at https://brew.sh/
# Homebrew only supports installing major Node.js versions and might not support the latest Node.js version from the 20 release line.

# download and install Node.js
brew install node@20

# verifies the right Node.js version is in the environment
node -v # should print `v20.17.0`

# verifies the right npm version is in the environment
npm -v # should print `10.8.2`
Para Windows:
Instalar Chocolatey https://chocolatey.org/
Correr:
# NOTE:
# Chocolatey is not a Node.js package manager.
# Please ensure it is already installed on your system.
# Follow official instructions at https://chocolatey.org/
# Chocolatey is not officially maintained by the Node.js project and might not support the v20.17.0 version of Node.js

# download and install Node.js
choco install nodejs-lts --version="20.17.0"

# verifies the right Node.js version is in the environment
node -v # should print `20`

# verifies the right npm version is in the environment
npm -v # should print `10.8.2`
Angular CLI
 npm install -g @angular/cli
.NET Core 8 SDK https://dotnet.microsoft.com/en-us/download/dotnet/8.0
4.1 Creaci�n de una BD SQL Server para nuestra App
A. Crear una BD Azure SQL Database (Ver Instructivo 5.1) o montar una imagen Docker de SQL Server como se solicit� en el punto 12 del [TP02]. (https://github.com/ingsoft3ucc/TPS_2024/blob/main/trabajos/02-introduccion-docker.md)

B. En caso de optar por la opci�n de montar la imagen de docker, una vez levantada el contenedor, conectarse y ejecutar el siguiente script:

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[Employees](
	[Id] [int] IDENTITY(1,1) NOT NULL,
	[Name] [nvarchar](max) NULL,
	[CreatedDate] [datetime2](7) NOT NULL,
 CONSTRAINT [PK_Employees] PRIMARY KEY CLUSTERED 
(
	[Id] ASC
)WITH (STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
GO
SET IDENTITY_INSERT [dbo].[Employees] ON 
GO
INSERT [dbo].[Employees] ([Id], [Name], [CreatedDate]) VALUES (1, N'Juan Perez', CAST(N'2024-09-05T00:00:00.0000000' AS DateTime2))
GO
INSERT [dbo].[Employees] ([Id], [Name], [CreatedDate]) VALUES (2, N'Carla Ruiz', CAST(N'2024-09-05T22:22:47.4405720' AS DateTime2))
GO
INSERT [dbo].[Employees] ([Id], [Name], [CreatedDate]) VALUES (3, N'Carlos Gomez', CAST(N'2024-09-06T09:16:50.0709430' AS DateTime2))
GO
INSERT [dbo].[Employees] ([Id], [Name], [CreatedDate]) VALUES (4, N'Joaquin Zarate', CAST(N'2024-09-06T09:19:37.8987160' AS DateTime2))
GO
INSERT [dbo].[Employees] ([Id], [Name], [CreatedDate]) VALUES (5, N'Luis Rodriguez', CAST(N'2024-09-06T09:23:31.3244340' AS DateTime2))
GO
SET IDENTITY_INSERT [dbo].[Employees] OFF
GO

4.2 Obtener nuestra App
A. Clonar el repo https://github.com/ingsoft3ucc/Angular_WebAPINetCore8_CRUD_Sample.git

B. Seguir las instrucciones del README.md del repo clonado prestando atenci�n a la modificaci�n de la cadena de conexi�n en el appSettings.json para que apunte a la BD creada en 4.1

C. Navegar a http://localhost:7150/swagger/index.html y probar uno de los controladores para verificar el correcto funcionamiento de la API. image

D. Navegar a http://localhost:4200 y verificar el correcto funcionamiento de nuestro front-end Angular image

E. Una vez verificado el correcto funcionamiento de la Aplicaci�n procederemos a crear un proyecto de pruebas unitarias para nuestra API.

4.3 Crear Pruebas Unitarias para nuestra API
A. En el directorio raiz de nuestro repo crear un nuevo proyecto de pruebas unitarias para nuestra API

dotnet new xunit -n EmployeeCrudApi.Tests
image

B. Instalar dependencias necesarias

Primero, instala las siguientes bibliotecas mediante NuGet:

-Moq

Moq es una popular biblioteca de mocking en .NET que se utiliza en pruebas unitarias para simular el comportamiento de dependencias externas o de objetos complejos. El objetivo de Moq es ayudar a los desarrolladores a escribir pruebas unitarias aisladas sin tener que interactuar con implementaciones reales de dependencias, como bases de datos, servicios externos, APIs o archivos del sistema. De esta manera, se puede probar el c�digo en condiciones controladas y reproducibles. Sirve para:

Simular el comportamiento de dependencias: Moq permite crear versiones simuladas (mocks) de las dependencias externas (por ejemplo, interfaces o clases abstractas) para verificar c�mo interact�a el c�digo con ellas.

Aislamiento de pruebas: Permite aislar el c�digo bajo prueba, garantizando que cualquier error en las dependencias externas no afecte las pruebas.

Verificaci�n de interacciones: Moq puede verificar si los m�todos de las dependencias se llamaron con los argumentos correctos o con qu� frecuencia fueron invocados.

Simulaci�n de resultados: Podemos configurar el comportamiento de los m�todos simulados para que devuelvan valores espec�ficos, excepciones o respuestas as�ncronas para distintos escenarios.

Ejemplos de uso:

Configurar retornos de m�todos: Se puede configurar un mock para que un m�todo retorne un valor espec�fico.
Verificar interacciones: Verificar si un m�todo fue llamado, cu�ntas veces y con qu� par�metros.
Simular excepciones: Probar c�mo responde el c�digo al manejar excepciones lanzadas por dependencias externas.
xUnit

xUnit es un marco de trabajo (framework) para pruebas unitarias en el ecosistema .NET. Es una de las opciones m�s populares para escribir pruebas automatizadas en proyectos de .NET, y es conocido por su simplicidad, flexibilidad y compatibilidad con varias versiones de .NET, incluyendo .NET Core y .NET 5/6/7/8.

Caracter�sticas principales de xUnit:

Atributos de prueba:

[Fact]: Se usa para definir una prueba unitaria b�sica.
[Theory]: Permite ejecutar una prueba varias veces con diferentes par�metros, �til para probar m�ltiples casos de una misma l�gica.
Inyecci�n de dependencias: xUnit soporta inyecci�n de dependencias en los constructores de las clases de prueba, lo que facilita la configuraci�n y limpieza de las pruebas.

Configuraci�n y limpieza de pruebas:

Los m�todos Dispose permiten liberar recursos al final de cada prueba.
Tambi�n se pueden usar los atributos [ClassFixture] y [Collection] para compartir el contexto entre pruebas y gestionar la configuraci�n y limpieza antes o despu�s de varias pruebas.
Integraci�n con herramientas de CI/CD: xUnit se integra perfectamente con sistemas de integraci�n continua y entrega continua (CI/CD) como Azure DevOps, GitHub Actions, y Jenkins, lo que lo convierte en una excelente opci�n para entornos de desarrollo �giles.

Compatibilidad con m�ltiples plataformas: xUnit es compatible con .NET Framework, .NET Core y .NET 5+, lo que lo hace vers�til y adaptable a diferentes proyectos.

Desventajas de NUnit y MSTest corregidas: xUnit surgi� como una evoluci�n de otros marcos como NUnit y MSTest, con el objetivo de corregir ciertas limitaciones. Por ejemplo, no utiliza m�todos como SetUp o TearDown (propios de NUnit), sino que promueve el uso del constructor de clases para inicializar objetos.

Microsoft.EntityFrameworkCore.InMemory

Este paquete es una implementaci�n del proveedor InMemory para Entity Framework Core, y su prop�sito principal es permitir realizar pruebas unitarias y de integraci�n en memoria, sin necesidad de conectarse a una base de datos f�sica. Simula el comportamiento de una base de datos sin necesidad de utilizar un servidor de base de datos real. Esto es �til en las siguientes situaciones:

Pruebas unitarias: Con InMemory, es posible insertar datos en la base de datos simulada, realizar operaciones de consulta, actualizaci�n y eliminaci�n, y verificar que el c�digo funcione correctamente sin tener que preocuparnos por la conexi�n a una base de datos real.
Prototipado r�pido: Si estamos desarrollando una aplicaci�n y necesitamos trabajar con datos sin configurar un entorno de base de datos completo, podemos usar InMemory para almacenar los datos de manera temporal mientras desarrollamos la l�gica de la aplicaci�n.
Aislamiento de pruebas: El proveedor InMemory garantiza que cada ejecuci�n de prueba pueda tener su propia instancia de base de datos aislada, lo que ayuda a asegurar que las pruebas sean consistentes y no dependan de datos residuales de ejecuciones anteriores.
cd EmployeeCrudApi.Tests 
dotnet add package Moq
dotnet add package xunit
dotnet add package Microsoft.EntityFrameworkCore.InMemory
C. Editar archivo UnitTest1.cs reemplazando su contenido por

using EmployeeCrudApi.Controllers;
using EmployeeCrudApi.Data;
using EmployeeCrudApi.Models;
using Microsoft.EntityFrameworkCore;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using Xunit;

namespace EmployeeCrudApi.Tests
{
    public class EmployeeControllerTests
    {
        private ApplicationDbContext GetInMemoryDbContext()
        {
            var options = new DbContextOptionsBuilder<ApplicationDbContext>()
                .UseInMemoryDatabase(databaseName: Guid.NewGuid().ToString()) // Crear una nueva base de datos en memoria para cada prueba
                .Options;

            return new ApplicationDbContext(options);
        }

        [Fact]
        public async Task GetAll_ReturnsListOfEmployees()
        {
            // Arrange
            var context = GetInMemoryDbContext();
            context.Employees.AddRange(
                new Employee { Id = 1, Name = "John Doe" },
                new Employee { Id = 2, Name = "Jane Doe" }
            );
            context.SaveChanges();

            var controller = new EmployeeController(context);

            // Act
            var result = await controller.GetAll();

            // Assert
            Assert.Equal(2, result.Count);
            Assert.Equal("John Doe", result[0].Name);
            Assert.Equal("Jane Doe", result[1].Name);
        }

        [Fact]
        public async Task GetById_ReturnsEmployeeById()
        {
            // Arrange
            var context = GetInMemoryDbContext();
            context.Employees.Add(new Employee { Id = 1, Name = "John Doe" });
            context.SaveChanges();

            var controller = new EmployeeController(context);

            // Act
            var result = await controller.GetById(1);

            // Assert
            Assert.NotNull(result);
            Assert.Equal(1, result.Id);
            Assert.Equal("John Doe", result.Name);
        }

        [Fact]
        public async Task Create_AddsEmployee()
        {
            // Arrange
            var context = GetInMemoryDbContext();
            var controller = new EmployeeController(context);

            var newEmployee = new Employee { Id = 3, Name = "New Employee" };

            // Act
            await controller.Create(newEmployee);

            // Assert
            var employee = await context.Employees.FindAsync(3);
            Assert.NotNull(employee);
            Assert.Equal("New Employee", employee.Name);
        }

        [Fact]
        public async Task Update_UpdatesEmployee()
        {
            // Arrange
            var context = GetInMemoryDbContext();
            var existingEmployee = new Employee { Id = 1, Name = "Old Name" };
            context.Employees.Add(existingEmployee);
            context.SaveChanges();

            var controller = new EmployeeController(context);

            var updatedEmployee = new Employee { Id = 1, Name = "Updated Name" };

            // Act
            await controller.Update(updatedEmployee);

            // Assert
            var employee = await context.Employees.FindAsync(1);
            Assert.NotNull(employee);
            Assert.Equal("Updated Name", employee.Name);
        }

        [Fact]
        public async Task Delete_RemovesEmployee()
        {
            // Arrange
            var context = GetInMemoryDbContext();
            var employeeToDelete = new Employee { Id = 1, Name = "John Doe" };
            context.Employees.Add(employeeToDelete);
            context.SaveChanges();

            var controller = new EmployeeController(context);

            // Act
            await controller.Delete(1);

            // Assert
            var employee = await context.Employees.FindAsync(1);
            Assert.Null(employee); // Verifica que el empleado fue eliminado
        }
    }
}
Explicaci�n:

Este c�digo es una serie de pruebas unitarias para un controlador EmployeeController en C# usando el framework de pruebas XUnit. Las pruebas utilizan una base de datos en memoria (in-memory) proporcionada por Entity Framework Core para evitar dependencias con una base de datos real. A continuaci�n, te explico c�mo funciona el c�digo y d�nde se realiza el mockeo.

An�lisis del c�digo

M�todo GetInMemoryDbContext: Este m�todo crea un contexto de base de datos en memoria utilizando Entity Framework Core. Cada prueba obtiene su propia instancia de base de datos in-memory �nica (por el uso de Guid.NewGuid()).
private ApplicationDbContext GetInMemoryDbContext()
{
    var options = new DbContextOptionsBuilder<ApplicationDbContext>()
        .UseInMemoryDatabase(databaseName: Guid.NewGuid().ToString()) // Crear una base de datos en memoria para cada prueba
        .Options;

    return new ApplicationDbContext(options);
}
InMemoryDatabase: Crea una base de datos temporal en memoria que se usa solo durante la ejecuci�n de las pruebas. Esto es �til para evitar la dependencia de una base de datos f�sica.
Guid.NewGuid(): Cada prueba tiene su propio contexto de base de datos �nico, lo que asegura que las pruebas no interfieran entre s�.
Prueba GetAll_ReturnsListOfEmployees: Esta prueba verifica que el controlador puede recuperar correctamente una lista de empleados.
[Fact]
public async Task GetAll_ReturnsListOfEmployees()
{
    // Arrange
    var context = GetInMemoryDbContext();
    context.Employees.AddRange(
        new Employee { Id = 1, Name = "John Doe" },
        new Employee { Id = 2, Name = "Jane Doe" }
    );
    context.SaveChanges();

    var controller = new EmployeeController(context);

    // Act
    var result = await controller.GetAll();

    // Assert
    Assert.Equal(2, result.Count);
    Assert.Equal("John Doe", result[0].Name);
    Assert.Equal("Jane Doe", result[1].Name);
}
Arrange: Inicializa el contexto en memoria, agrega algunos empleados de prueba y luego crea una instancia del controlador.
Act: Llama al m�todo GetAll() del controlador para recuperar los empleados.
Assert: Verifica que se recuperaron los empleados correctamente y que sus nombres coinciden con los esperados.
Prueba GetById_ReturnsEmployeeById: Verifica que se pueda recuperar un empleado por su ID.
[Fact]
public async Task GetById_ReturnsEmployeeById()
{
    // Arrange
    var context = GetInMemoryDbContext();
    context.Employees.Add(new Employee { Id = 1, Name = "John Doe" });
    context.SaveChanges();

    var controller = new EmployeeController(context);

    // Act
    var result = await controller.GetById(1);

    // Assert
    Assert.NotNull(result);
    Assert.Equal(1, result.Id);
    Assert.Equal("John Doe", result.Name);
}
Similar a la prueba anterior, pero en este caso se verifica que el m�todo GetById() funcione correctamente.
Prueba Create_AddsEmployee: Verifica que se puede crear un nuevo empleado y que se almacene correctamente en la base de datos.
[Fact]
public async Task Create_AddsEmployee()
{
    // Arrange
    var context = GetInMemoryDbContext();
    var controller = new EmployeeController(context);
    var newEmployee = new Employee { Id = 3, Name = "New Employee" };

    // Act
    await controller.Create(newEmployee);

    // Assert
    var employee = await context.Employees.FindAsync(3);
    Assert.NotNull(employee);
    Assert.Equal("New Employee", employee.Name);
}
Act: Llama al m�todo Create del controlador para agregar un nuevo empleado.
Assert: Verifica que el empleado fue agregado a la base de datos in-memory correctamente.
Prueba Update_UpdatesEmployee: Verifica que se pueda actualizar la informaci�n de un empleado existente
[Fact]
public async Task Update_UpdatesEmployee()
{
    // Arrange
    var context = GetInMemoryDbContext();
    var existingEmployee = new Employee { Id = 1, Name = "Old Name" };
    context.Employees.Add(existingEmployee);
    context.SaveChanges();

    var controller = new EmployeeController(context);
    var updatedEmployee = new Employee { Id = 1, Name = "Updated Name" };

    // Act
    await controller.Update(updatedEmployee);

    // Assert
    var employee = await context.Employees.FindAsync(1);
    Assert.NotNull(employee);
    Assert.Equal("Updated Name", employee.Name);
}
Act: Actualiza el empleado llamando a Update.
Assert: Verifica que el nombre del empleado fue actualizado correctamente.
Prueba Delete_RemovesEmployee: Verifica que se puede eliminar un empleado.
[Fact]
public async Task Delete_RemovesEmployee()
{
    // Arrange
    var context = GetInMemoryDbContext();
    var employeeToDelete = new Employee { Id = 1, Name = "John Doe" };
    context.Employees.Add(employeeToDelete);
    context.SaveChanges();

    var controller = new EmployeeController(context);

    // Act
    await controller.Delete(1);

    // Assert
    var employee = await context.Employees.FindAsync(1);
    Assert.Null(employee); // Verifica que el empleado fue eliminado
}
Act: Llama al m�todo Delete del controlador para eliminar un empleado.
Assert: Verifica que el empleado fue eliminado correctamente de la base de datos in-memory.
�D�nde ocurre el mockeo?
In-memory database (mockeo del contexto de la base de datos): En este c�digo, el mockeo se realiza principalmente mediante el uso de la base de datos en memoria. Se utiliza el m�todo UseInMemoryDatabase() para crear una base de datos simulada para las pruebas, lo que permite probar el controlador sin interactuar con una base de datos real.
EmployeeController: El controlador utiliza este contexto simulado para realizar operaciones CRUD durante las pruebas.
Resumen:
El mockeo principal es la base de datos en memoria, que permite realizar operaciones de consulta, creaci�n, actualizaci�n y eliminaci�n sin necesidad de una base de datos real.
Cada prueba crea un nuevo contexto de base de datos en memoria para asegurar que las pruebas no interfieran entre s�.
Se cubren operaciones b�sicas del controlador: obtener todos los empleados, obtener un empleado por ID, crear, actualizar y eliminar empleados.
D. Renombrar archivo UnitTest1.cs por EmployeeControllerUnitTests.cs

mv UnitTest1.cs EmployeeControllerUnitTests.cs 
E. Editar el archivo EmployeeCrudApi.Tests/EmployeeCrudApi.Tests.csproj para agregar una referencia a nuestro proyecto de EmployeeCrudApi reemplazando su contenido por

<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>

    <IsPackable>false</IsPackable>
    <IsTestProject>true</IsTestProject>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="coverlet.collector" Version="6.0.0" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.InMemory" Version="8.0.8" />
    <PackageReference Include="Microsoft.NET.Test.Sdk" Version="17.8.0" />
    <PackageReference Include="Moq" Version="4.20.71" />
    <PackageReference Include="xunit" Version="2.9.0" />
    <PackageReference Include="xunit.runner.visualstudio" Version="2.5.3" />
  </ItemGroup>

  <ItemGroup>
    <ProjectReference Include="../EmployeeCrudApi/EmployeeCrudApi/EmployeeCrudApi.csproj" />
  </ItemGroup>

  <ItemGroup>
    <Using Include="Xunit" />
  </ItemGroup>

</Project>
F. Ejecutar los siguientes comandos para ejecutar nuestras pruebas

dotnet build
dotnet test
G. Verificar que se hayan ejecutado correctamente las pruebas image

H. Verificar que no estamos usando una dependencia externa como la base de datos.

I. Modificar la cadena de conexi�n en el archivo appsettings.json para que use un usuario o password incorrecto y recompilar el proyecto EmployeeCrudApi

dotnet build
dotnet run --urls "http://localhost:7150"
J. Verificar que nuestro proyecto ya no tiene acceso a la BD navegando a http://localhost:7150/swagger/index.html y probando uno de los controladores: image

K. En la carpeta de nuestro proyecto EmployeeCrudApi.Tests volver a correr las pruebas

dotnet build
dotnet test
L. Verificar que se hayan ejecutado correctamente las pruebas inclusive sin tener acceso a la BD, lo que confirma que es efectivamente un conjunto de pruebas unitarias que no requieren de una dependencia externa para funcionar. image

M. Modificar la cadena de conexi�n en el archivo appsettings.json para que use el usuario y password correcto y recompilar el proyecto EmployeeCrudApi

dotnet build
dotnet run --urls "http://localhost:7150"
N. Verificar que nuestro proyecto vuelve a tener acceso a la BD navegando a http://localhost:7150/swagger/index.html y probando uno de los controladores: image

4.4 Creamos pruebas unitarias para nuestro front de Angular:
Intro. Para las pruebas unitarias de nuestro front en Angular utilizaremos Jasmine y Karma, herramientas ampliamente utilizadas para pruebas unitarias en aplicaciones web, especialmente en proyectos de Angular.

Jasmine: Es un framework de pruebas unitarias para JavaScript. Es popular porque no requiere dependencias externas y es f�cil de usar. Jasmine se utiliza para escribir pruebas de manera estructurada y ofrece un conjunto de funciones para facilitar la verificaci�n del comportamiento del c�digo.

Caracter�sticas principales:
Sintaxis descriptiva: Jasmine permite escribir pruebas en un estilo de "BDD" (Desarrollo Orientado a Comportamiento). Usa palabras clave como describe, it, expect para definir pruebas de forma muy legible.
Sin dependencias externas: A diferencia de otros frameworks de prueba, Jasmine no necesita ninguna biblioteca externa para funcionar.
Esp�as (Spies): Jasmine permite observar llamadas a funciones, rastrearlas y verificar si fueron llamadas correctamente.
Karma: Es un test runner (ejecutor de pruebas) que facilita la ejecuci�n de pruebas en varios navegadores. Est� dise�ado para trabajar con frameworks de pruebas como Jasmine, Mocha, entre otros. Karma se integra perfectamente con proyectos de Angular y es parte del Angular CLI.

Caracter�sticas principales:
Soporte de m�ltiples navegadores: Karma permite ejecutar pruebas en diferentes navegadores (Chrome, Firefox, Safari, etc.) de forma autom�tica.
Ejecuci�n en modo de observaci�n: Karma puede detectar cambios en los archivos y ejecutar las pruebas autom�ticamente en cuanto se modifican.
Generaci�n de informes: Karma proporciona informes detallados sobre el resultado de las pruebas.
Integraci�n con CI/CD: Se puede integrar con sistemas de integraci�n continua como Azure Devops, Jenkins, Travis CI o CircleCI para ejecutar las pruebas autom�ticamente en un entorno controlado.
Flujo de trabajo con Karma:
Karma inicia un servidor.
Abre un navegador (o varios) y ejecuta las pruebas en cada uno de ellos.
Karma se queda a la espera de cambios en el c�digo, ejecutando nuevamente las pruebas si detecta modificaciones.
A. Nos posicionamos en nuestro proyecto de front, en el directorio EmployeeCrudAngular/src/app

pwd
image

B. Editamos el archivo app.component.spec.ts reemplazando su contenido por:

import { TestBed } from '@angular/core/testing';
import { AppComponent } from './app.component'; // Ajusta la ruta si es necesario

describe('AppComponent', () => {
  beforeEach(async () => {
    await TestBed.configureTestingModule({
      imports: [AppComponent], // Usa imports en lugar de declarations
    }).compileComponents();
  });

  it('should render title', () => {
    const fixture = TestBed.createComponent(AppComponent);
    fixture.detectChanges();
    const compiled = fixture.nativeElement as HTMLElement;
    expect(compiled.querySelector('h1')?.textContent).toContain('EmployeeCrudAngular');
  });

});
Explicaci�n:

Este c�digo realiza una prueba unitaria para el componente AppComponent en una aplicaci�n de Angular utilizando Jasmine y TestBed para configurar el entorno de pruebas. A continuaci�n se explica c�mo funciona y d�nde se realiza el mockeo:

An�lisis del c�digo:

Configuraci�n del entorno de prueba: El bloque beforeEach se ejecuta antes de cada prueba y configura el entorno de pruebas utilizando TestBed.
beforeEach(async () => {
  await TestBed.configureTestingModule({
    imports: [AppComponent], // Usa imports en lugar de declarations
  }).compileComponents();
});
TestBed: Es la clase principal en Angular que permite configurar y crear un entorno de pruebas para los componentes, servicios y m�dulos.
compileComponents: Este m�todo compila los componentes declarados, asegurando que todo el HTML y el CSS del componente se carguen correctamente antes de ejecutar las pruebas.
Prueba del t�tulo renderizado: La prueba it verifica que el t�tulo del componente AppComponent se renderice correctamente en el DOM.
it('should render title', () => {
  const fixture = TestBed.createComponent(AppComponent); // Crea una instancia del componente
  fixture.detectChanges(); // Dispara el ciclo de detecci�n de cambios de Angular para renderizar el componente
  const compiled = fixture.nativeElement as HTMLElement; // Obtiene el DOM renderizado del componente
  expect(compiled.querySelector('h1')?.textContent).toContain('EmployeeCrudAngular'); // Verifica si el h1 contiene el texto 'EmployeeCrudAngular'
});
Mockeo en este c�digo: Este c�digo en particular no realiza ning�n mockeo expl�cito. Es decir, no est� utilizando servicios externos ni llamadas HTTP que necesiten ser simuladas (mockeadas).
Sin embargo, en una prueba de componentes como esta, el mockeo podr�a ser necesario si:

El componente depende de servicios: Si el AppComponent dependiera de un servicio para obtener datos, este servicio tendr�a que ser simulado (mockeado) para evitar llamadas reales a una API o a otros recursos externos.
El componente tiene dependencias complejas: En esos casos, se podr�an usar objetos o servicios falsos (mocks) para simular el comportamiento de las dependencias.
Resumen:
TestBed se utiliza para configurar el entorno de prueba y crear una instancia del componente.
No se realiza mockeo expl�cito, ya que esta prueba solo verifica la correcta renderizaci�n del t�tulo del componente y no interact�a con servicios o datos externos.
Si el componente fuera m�s complejo, necesitar�as simular las dependencias, pero en este caso no es necesario debido a la simplicidad del componente.

C. Creamos el archivo employee.service.spec.ts reemplazando su contenido por:

import { TestBed } from '@angular/core/testing';
import { HttpClientTestingModule, HttpTestingController } from '@angular/common/http/testing';
import { EmployeeService } from './employee.service';
import { Employee } from './employee.model';
import { DatePipe } from '@angular/common';

describe('EmployeeService', () => {
  let service: EmployeeService;
  let httpMock: HttpTestingController;
  let datePipe: DatePipe;

  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [HttpClientTestingModule],
      providers: [
        EmployeeService,
        DatePipe
      ]
    });

    service = TestBed.inject(EmployeeService);
    httpMock = TestBed.inject(HttpTestingController);
    datePipe = TestBed.inject(DatePipe);
  });

  afterEach(() => {
    httpMock.verify();
  });



  it('should retrieve all employees', () => {
    const today = new Date();
    const expectedDateTime = datePipe.transform(today, 'dd/MM/yyyy HH:mm:ss', undefined) ?? '';  // Consistente con el servicio

    const dummyEmployees: Employee[] = [
      new Employee(1, 'John Doe', expectedDateTime),
      new Employee(2, 'Jane Smith', expectedDateTime)
    ];

    service.getAllEmployee().subscribe(employees => {
      expect(employees.length).toBe(2);
      employees.forEach((employee, index) => {
        // Agrega depuraci�n aqu�
        console.log('Employee createdDate:', datePipe.transform(employee.createdDate, 'dd/MM/yyyy HH:mm:ss', undefined)?? '');  // Imprimir el valor generado por el servicio
        console.log('Dummy employee createdDate:', datePipe.transform(dummyEmployees[index].createdDate, 'MM/dd/yyyy HH:mm:ss', undefined)?? '');   // Imprimir el valor esperado

        expect(datePipe.transform(employee.createdDate, 'dd/MM/yyyy HH:mm:ss', undefined)?? '').toEqual(datePipe.transform(dummyEmployees[index].createdDate, 'MM/dd/yyyy HH:mm:ss', undefined)?? '');  // Compara la fecha completa
      });
    });

    const req = httpMock.expectOne(`${service.apiUrlEmployee}/getall`);
    expect(req.request.method).toBe('GET');
    req.flush(dummyEmployees);
  });


});
Explicaci�n:

Este c�digo es una prueba unitaria para un servicio de Angular llamado EmployeeService, utilizando Jasmine como framework de pruebas y HttpClientTestingModule para simular las peticiones HTTP.

An�lisis del c�digo:

Configuraci�n del entorno de prueba: El bloque beforeEach se ejecuta antes de cada prueba y configura el entorno para la ejecuci�n.
beforeEach(() => {
  TestBed.configureTestingModule({
    imports: [HttpClientTestingModule], // Importa un m�dulo de prueba para simular HttpClient
    providers: [
      EmployeeService, // Proveedor del servicio a probar
      DatePipe         // Proveedor de DatePipe (para formatear fechas)
    ]
  });

  service = TestBed.inject(EmployeeService); // Inyecta el servicio `EmployeeService` que se va a probar
  httpMock = TestBed.inject(HttpTestingController); // Inyecta `HttpTestingController` para simular las peticiones HTTP
  datePipe = TestBed.inject(DatePipe); // Inyecta `DatePipe` para formatear fechas en la prueba
});
HttpClientTestingModule: Se utiliza para evitar realizar peticiones HTTP reales. Este m�dulo permite simular y controlar las peticiones que realiza el servicio durante la prueba.

HttpTestingController: Es el objeto que permite controlar y verificar las peticiones HTTP que realiza el servicio.

Mockeado y verificaci�n de peticiones HTTP: En la prueba (it), se verifica que el servicio getAllEmployee haga la petici�n HTTP correctamente y que los datos recibidos coincidan con los esperados.
it('should retrieve all employees', () => {
  const today = new Date();
  const expectedDateTime = datePipe.transform(today, 'dd/MM/yyyy HH:mm:ss', undefined) ?? ''; // Formatea la fecha actual

  const dummyEmployees: Employee[] = [
    new Employee(1, 'John Doe', expectedDateTime),
    new Employee(2, 'Jane Smith', expectedDateTime)
  ];

  // Se simula la llamada al servicio
  service.getAllEmployee().subscribe(employees => {
    expect(employees.length).toBe(2); // Verifica que se reciban dos empleados
    employees.forEach((employee, index) => {
      // Compara la fecha formateada del servicio con la de los datos mockeados
      expect(datePipe.transform(employee.createdDate, 'dd/MM/yyyy HH:mm:ss', undefined) ?? '').toEqual(
        datePipe.transform(dummyEmployees[index].createdDate, 'MM/dd/yyyy HH:mm:ss', undefined) ?? ''
      );
    });
  });

  // Se espera que se haya hecho una petici�n GET
  const req = httpMock.expectOne(`${service.apiUrlEmployee}/getall`);
  expect(req.request.method).toBe('GET');

  // Simula la respuesta de la API con los datos de `dummyEmployees`
  req.flush(dummyEmployees);
});
D�nde se realiza el mockeo?

Mockeo de peticiones HTTP:
HttpClientTestingModule y HttpTestingController permiten simular las peticiones HTTP realizadas por el servicio.
El c�digo httpMock.expectOne() se asegura de que se realice una solicitud a la URL correcta.
El m�todo req.flush() simula la respuesta de la API devolviendo los datos mockeados (dummyEmployees).
Datos mockeados:
dummyEmployees es un arreglo de empleados ficticios que simula la respuesta de la API. Se compara con los datos reales que el servicio devuelve para asegurarse de que los empleados se han recibido y formateado correctamente.
3- Uso de DatePipe: El DatePipe se utiliza tanto en el servicio como en la prueba para garantizar que las fechas se formateen correctamente. La prueba compara las fechas formateadas de los datos recibidos con las fechas formateadas de los datos mockeados.

4- Resumen: El c�digo mockea:

La respuesta de la API (con dummyEmployees).
Las peticiones HTTP usando HttpClientTestingModule y HttpTestingController. Esto permite que la prueba se ejecute sin realizar llamadas reales a una API.
D. Editamos el archivo employee.component.spec.ts ubicado en la carpeta employee reemplazando su contenido por:

import { TestBed } from '@angular/core/testing';
import { EmployeeComponent } from './employee.component';
import { HttpClientTestingModule } from '@angular/common/http/testing';
import { DatePipe } from '@angular/common';

describe('EmployeeComponent', () => {
  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [EmployeeComponent, HttpClientTestingModule],
      providers: [DatePipe] // A�ade DatePipe a los proveedores
    });
  });

  it('should create', () => {
    const fixture = TestBed.createComponent(EmployeeComponent);
    const component = fixture.componentInstance;
    expect(component).toBeTruthy();
  });
});
Explicaci�n: Este c�digo realiza una prueba unitaria del componente EmployeeComponent utilizando Jasmine y TestBed. A continuaci�n, se explica c�mo funciona y d�nde se lleva a cabo el mockeo (si lo hay).

An�lisis del c�digo:

Configuraci�n del entorno de prueba: El bloque beforeEach configura el entorno de pruebas para el EmployeeComponent.
beforeEach(() => {
  TestBed.configureTestingModule({
    imports: [EmployeeComponent, HttpClientTestingModule], // Importa el componente y el m�dulo de pruebas HTTP
    providers: [DatePipe] // Proporciona el DatePipe para ser usado en las pruebas
  });
});
TestBed: Es el entorno de pruebas para Angular que permite configurar m�dulos, componentes y servicios para pruebas.
imports: En esta prueba, se importa el propio EmployeeComponent y el HttpClientTestingModule, que es un m�dulo proporcionado por Angular para realizar pruebas de HTTP sin hacer llamadas reales a una API.
providers: Aqu� se est� proporcionando el DatePipe, que se puede usar para realizar transformaciones de fechas dentro del componente. Si EmployeeComponent lo necesita, estar� disponible para inyecci�n de dependencias.
Prueba de creaci�n del componente: Esta prueba simple verifica que el EmployeeComponent se pueda crear correctamente.
it('should create', () => {
  const fixture = TestBed.createComponent(EmployeeComponent); // Crea una instancia de `EmployeeComponent`
  const component = fixture.componentInstance; // Obtiene la instancia del componente
  expect(component).toBeTruthy(); // Verifica que la instancia del componente sea v�lida
});
fixture: Es el contenedor para una instancia del componente en el contexto de prueba. A trav�s del fixture se puede interactuar con el componente y el DOM asociado.
component: La instancia real del EmployeeComponent que se crea en la prueba.
expect: La prueba verifica que el componente se haya creado correctamente, comprobando que el componente no sea null o undefined.
Mockeo en este c�digo: El mockeo expl�cito ocurre a trav�s del uso de HttpClientTestingModule.
HttpClientTestingModule: Este m�dulo de pruebas reemplaza el HttpClient real con un cliente HTTP simulado, evitando que se realicen solicitudes HTTP reales durante la prueba. As� se "mockea" cualquier solicitud HTTP que podr�a haber sido realizada por el EmployeeComponent al backend.
DatePipe: Aunque se proporciona el DatePipe para ser utilizado en las pruebas, no se est� realizando ning�n mockeo sobre este en el c�digo. Se est� utilizando la implementaci�n real del DatePipe.
�D�nde ocurre el mockeo?
El mockeo principal ocurre en el uso de HttpClientTestingModule. Al incluir este m�dulo, cualquier llamada HTTP realizada por EmployeeComponent ser� interceptada por un servicio HTTP simulado, evitando solicitudes reales a un backend.
En esta prueba en particular, el enfoque est� en verificar que el componente se crea correctamente, por lo que no se est�n simulando interacciones con el backend. Si se realizaran solicitudes HTTP dentro del componente, este mockeo ser�a gestionado por el HttpTestingController para interceptar las llamadas.

Resumen:
TestBed configura el entorno de prueba para el EmployeeComponent.
HttpClientTestingModule mockea cualquier solicitud HTTP, asegurando que no se realicen llamadas reales a la API.
DatePipe se inyecta como proveedor, pero no se realiza ning�n mockeo sobre �l.
La prueba verifica que el componente se cree correctamente.
E. Editamos el archivo addemployee.component.spec.ts ubicado en la carpeta addemployee reemplazando su contenido por:

import { TestBed } from '@angular/core/testing';
import { AddemployeeComponent } from './addemployee.component';
import { HttpClientTestingModule } from '@angular/common/http/testing';
import { ActivatedRoute } from '@angular/router';
import { of } from 'rxjs'; // para simular observables
import { DatePipe } from '@angular/common';

describe('AddemployeeComponent', () => {
  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [AddemployeeComponent, HttpClientTestingModule],
      providers: [
        DatePipe,
        {
          provide: ActivatedRoute, // Simula ActivatedRoute
          useValue: {
            params: of({ id: 1 }) // simula el par�metro id en la URL
          }
        }
      ]
    });
  });

  it('should create', () => {
    const fixture = TestBed.createComponent(AddemployeeComponent);
    const component = fixture.componentInstance;
    expect(component).toBeTruthy();
  });
});
Explicaci�n: Este c�digo realiza una prueba unitaria del componente AddemployeeComponent en Angular utilizando Jasmine y TestBed. A continuaci�n, se explica c�mo funciona y d�nde se realiza el mockeo.

An�lisis del c�digo:

Configuraci�n del entorno de prueba: En el bloque beforeEach, se configura el entorno de pruebas para el componente AddemployeeComponent.
beforeEach(() => {
  TestBed.configureTestingModule({
    imports: [AddemployeeComponent, HttpClientTestingModule], // Importa el componente y m�dulo de pruebas HTTP
    providers: [
      DatePipe, // Inyecta DatePipe para el formateo de fechas en el componente
      {
        provide: ActivatedRoute, // Simula ActivatedRoute
        useValue: {
          params: of({ id: 1 }) // Simula el par�metro de la ruta (id) como observable
        }
      }
    ]
  });
});
TestBed: Configura el entorno de pruebas para que puedas probar tu componente dentro del mismo.
imports: Aqu� se importa el propio componente AddemployeeComponent y el m�dulo HttpClientTestingModule, que es un mock del HttpClient para pruebas.
providers: Define inyecciones de dependencias para el componente.
DatePipe: Se inyecta el DatePipe, �til si el componente necesita realizar operaciones de formateo de fechas.
ActivatedRoute: Se provee un mock de ActivatedRoute usando useValue. Esto permite simular un par�metro de la URL (en este caso, { id: 1 }) sin tener que usar el enrutador real.
Prueba de creaci�n del componente: Este test verifica que el componente AddemployeeComponent se crea correctamente.
it('should create', () => {
  const fixture = TestBed.createComponent(AddemployeeComponent); // Crea una instancia de AddemployeeComponent
  const component = fixture.componentInstance; // Obtiene la instancia del componente
  expect(component).toBeTruthy(); // Verifica que el componente ha sido creado correctamente
});
fixture: Es el contenedor que proporciona acceso a una instancia del componente en el contexto de prueba.
component: Es la instancia real del componente AddemployeeComponent.
expect: Verifica que el componente se haya creado correctamente.
Mockeo en este c�digo:
En este c�digo, se realizan varios mockeos importantes:

HttpClientTestingModule: Este m�dulo reemplaza el HttpClient real con uno simulado para evitar realizar llamadas HTTP reales en el contexto de pruebas.
ActivatedRoute: Se provee un mock de ActivatedRoute para simular par�metros de ruta. En este caso, params: of({ id: 1 }) simula que la ruta actual tiene un par�metro id con valor 1. Esto es �til si el componente depende de los par�metros de la ruta para cargar datos o realizar operaciones espec�ficas.
�D�nde ocurre el mockeo?
HttpClientTestingModule: Mockea las llamadas HTTP realizadas por el AddemployeeComponent. Si el componente hace solicitudes HTTP, estas ser�n interceptadas por el mock.
ActivatedRoute: Se mockea usando useValue para simular los par�metros de la URL. En este caso, se est� simulando que el par�metro id en la URL es 1, sin depender del enrutador real.
�Por qu� es importante?
El mockeo del ActivatedRoute permite probar el comportamiento del componente sin la necesidad de configurar un enrutador real ni rutas.
HttpClientTestingModule evita que las pruebas hagan llamadas reales a una API, lo que hace que las pruebas sean m�s r�pidas, confiables y controladas.
Resumen:
TestBed configura el entorno de prueba para el componente AddemployeeComponent.
HttpClientTestingModule mockea las llamadas HTTP para evitar solicitudes reales.
ActivatedRoute est� mockeado para simular los par�metros de la ruta, lo que permite probar el comportamiento dependiente de la URL.
La prueba verifica que el componente se crea correctamente, pero no valida ning�n comportamiento m�s all� de la creaci�n del componente.
F. En el directorio raiz de nuestro proyecto EmployeeCrudAngular ejecutamos el comando

ng test
En proyectos de Angular, Jasmine se usa para escribir las pruebas, y Karma se encarga de ejecutarlas. Cuando ejecutamos el comando ng test, Karma se inicia, carga las pruebas escritas en Jasmine, y las ejecuta en un navegador.

G. Vemos que se abre una ventana de Karma con Jasmine en la que nos indica que los tests se ejecutaron correctamente image

H. Vemos que los tests se ejecutaron correctamente: image

I. Verificamos que no est� corriendo nuestra API navegando a http://localhost:7150/swagger/index.html y recibiendo esta salida: image

J. Los puntos G y H nos indican que se han ejecutado correctamente las pruebas inclusive sin tener acceso a la API, lo que confirma que es efectivamente un conjunto de pruebas unitarias que no requieres de una dependencia externa para funcionar.

4.5 Agregamos generaci�n de reporte XML de nuestras pruebas de front.
Para cuando integremos nuestras pruebas en un pipeline de Build, vamos a necesitar el resultado devuelto por nuestras pruebas para reportarlas junto a las pruebas de back que se reportan automaticamente.

image

Haremos los siguientes pasos para prepararnos:

A. Instalamos dependencia karma-junit-reporter

npm install karma-junit-reporter --save-dev
B. En el directorio raiz de nuestro proyecto (al mismo nivel que el archivo angular.json) creamos un archivo karma.conf.js con el siguiente contenido

module.exports = function (config) {
  config.set({
    frameworks: ['jasmine', '@angular-devkit/build-angular'],
    plugins: [
      require('karma-jasmine'),
      require('karma-chrome-launcher'),
      require('karma-junit-reporter'),
      require('@angular-devkit/build-angular/plugins/karma')
    ],
    reporters: ['progress', 'junit'],
    junitReporter: {
      outputDir: 'test-results',
      outputFile: 'test-results.xml',
      useBrowserName: false
    },
    port: 9876,
    colors: true,
    logLevel: config.LOG_INFO,
    autoWatch: true,
    browsers: ['ChromeHeadless'],
    singleRun: true,
    restartOnFileChange: true
  });
};
C. Ejecutamos nuestros test de la siguiente manera:

ng test --karma-config=karma.conf.js --watch=false --browsers ChromeHeadless
D. Verificamos que se creo un archivo test-result.xml en el directorio test-results que est� al mismo nivel que el directorio src

4.6 Modificamos el c�digo de nuestra API y creamos nuevas pruebas unitarias:
A. Realizar al menos 5 de las siguientes modificaciones sugeridas al c�digo de la API:

Al agregar y al editar un empleado, controlar que el nombre del empleado no est� repetido.
La longitud m�xima del nombre y apellido del empleado debe ser de 100 caracteres.
Almacenar el nombre en la BD siempre con la primera letra de los nombres en Mayuscula y todo el apellido en Mayusculas. Ejemplo, si recibo juan carlos chamizo, se debe almacenar como Juan Carlos CHAMIZO.
Asegurar que el nombre del empleado no contenga caracteres especiales o n�meros, a menos que sea necesario (por ejemplo, caracteres especiales en apellidos como "O'Connor" o "Garc�a").
Validar que el nombre tenga un n�mero m�nimo de caracteres, por ejemplo, al menos dos caracteres para evitar entradas inv�lidas como "A".
Verificar que el nombre no contenga n�meros, ya que no es com�n en los nombres de empleados.
Asegurar que cada parte del nombre (separada por espacios) tenga al menos un car�cter o m�s, por ejemplo, para evitar "A B".
Verificar que no haya palabras vac�as o que el nombre no est� compuesto solo de espacios.
Prohibir el uso de nombres triviales o gen�ricos como "Empleado", "N/A", "Nombre", etc.
Evitar que se ingresen caracteres repetidos de forma excesiva, como "Juuuuaannnn".
Implementar un filtro para evitar el uso de palabras inapropiadas, ofensivas o que puedan violar pol�ticas internas. En todos los casos donde no se cumplan las condiciones, la API debe devolver un error de HTTP 400 Bad Request y un Json indicando el error, por ejemplo:
{
  "status": 400,
  "error": "Bad Request",
  "message": "El nombre del empleado ya existe."
}
B. Crear las pruebas unitarias necesarias para validar las modificaciones realizadas en el c�digo

4.7 Modificamos el c�digo de nuestro Front y creamos nuevas pruebas unitarias:
A. Realizar en el c�digo del front las mismas modificaciones hechas a la API.

B. Las validaciones deben ser realizadas en el front sin llegar a la API, y deben ser mostradas en un toast como por ejemplo https://stackblitz.com/edit/angular12-toastr?file=src%2Fapp%2Fapp.component.ts o https://stackblitz.com/edit/angular-error-toast?file=src%2Fapp%2Fcore%2Frxjsops.ts

C. Crear las pruebas unitarias necesarias en el front para validar las modificaciones realizadas en el c�digo del front.