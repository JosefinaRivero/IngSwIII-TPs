### 4- Desarrollo:
#### Prerequisitos:

#### 4.1 Agregar Code Coverage a nuestras pruebas unitarias de backend y front-end e integrarlas junto con sus resultados en nuestro pipeline de build.
- Desarrollo del punto 4.1: 
	- ##### 4.1.1 En el directorio raiz de nuestro proyecto Angular instalar el siguiente paquete:
  	```bash
	npm install karma-coverage --save-dev
   	```
   	 - ##### 4.1.2 Editar nuestro archivo karma.conf.js para que incluya reporte de cobertura
  	```javascript
	module.exports = function (config) {
	  config.set({
	    frameworks: ['jasmine', '@angular-devkit/build-angular'],
	    plugins: [
	      require('karma-jasmine'),
	      require('karma-chrome-launcher'),
	      require('karma-junit-reporter'),
	      require('karma-coverage'),
	      require('@angular-devkit/build-angular/plugins/karma')
	    ],
	    reporters: ['progress', 'junit', 'coverage'],
	    junitReporter: {
	      outputDir: 'test-results',
	      outputFile: 'test-results.xml',
	      useBrowserName: false
	    },
	    coverageReporter: {
	      type: 'lcov',
	      dir: require('path').join(__dirname, './coverage'),
	      subdir: '.',
	      file: 'lcov.info'
	    },
	    preprocessors: {
	      // A�ade los archivos que deseas instrumentar para la cobertura
	      'src/**/*.ts': ['coverage'], // Aseg�rate de instrumentar los archivos de tu aplicaci�n
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

	```
   	- ##### 4.1.3 En el dir raiz del proyecto EmployeeCrudApi.Tests ejecutar:
   	  ```bash
   	  dotnet add package coverlet.collector
   	  ```
   	- ##### 4.1.4 Agregar a nuestro pipeline ANTES del Build de Back la tarea de test con los argumentos especificados y la de publicaci�n de resultados de cobertura:
	```yaml
    - task: DotNetCoreCLI@2
      displayName: 'Ejecutar pruebas de la API'
      inputs:
        command: 'test'
        projects: '**/*.Tests.csproj'  # Aseg�rate de que el patr�n apunte a tu proyecto de pruebas
        arguments: '--collect:"XPlat Code Coverage"'

    - task: PublishCodeCoverageResults@2
      inputs:
        summaryFileLocation: '$(Agent.TempDirectory)/**/*.cobertura.xml'
        failIfCoverageEmpty: false
      displayName: 'Publicar resultados de code coverage del back-end'
	```
 	- ##### 4.1.5 Agregar a nuestro pipeline ANTES del Build de front la tarea de test y la de publicaci�n de los resultados.
	   ```yaml
	    - script: npx ng test --karma-config=karma.conf.js --watch=false --browsers ChromeHeadless --code-coverage
	      displayName: 'Ejecutar pruebas del front'
	      workingDirectory: $(projectPath)
	      continueOnError: true  # Para que el pipeline contin�e aunque falle
	    
	    - task: PublishCodeCoverageResults@2
	      inputs:
	        summaryFileLocation: '$(projectPath)/coverage/lcov.info'
	        failIfCoverageEmpty: false
	      condition: always()  # Esto asegura que se ejecute siempre
	      displayName: 'Publicar resultados de code coverage del front'  
	    
	    - task: PublishTestResults@2
	      inputs:
	        testResultsFormat: 'JUnit'
	        testResultsFiles: '$(projectPath)/test-results/test-results.xml'
	        failTaskOnFailedTests: true
	      condition: always()  # Esto asegura que se ejecute siempre
	      displayName: 'Publicar resultados de pruebas unitarias del front'
    	```
	
	- ##### 4.1.6 Ejecutar el pipeline y analizar el resultado de las pruebas unitarias y la cobertura de c�digo.

 	![image](https://github.com/user-attachments/assets/2798520e-a33e-40bf-9337-c822dfb7e805)

  	![image](https://github.com/user-attachments/assets/e489405d-85a3-4cc9-ade3-19f5b98a3737)


#### 4.2 Agregar An�lisis Est�tico de C�digo con SonarCloud:

SonarCloud https://www.sonarsource.com/products/sonarcloud/ es una plataforma de an�lisis de calidad de c�digo basada en la nube que ayuda a los equipos de desarrollo a detectar errores, vulnerabilidades de seguridad, malas pr�cticas y problemas de mantenibilidad en el c�digo. Proporciona an�lisis est�tico de c�digo para m�ltiples lenguajes, como Java, .NET, JavaScript, TypeScript, Python, y m�s, ofreciendo m�tricas detalladas sobre la cobertura de pruebas, duplicaci�n de c�digo, complejidad ciclomatica, y deuda t�cnica. Adem�s, SonarCloud se integra f�cilmente con sistemas de CI/CD y plataformas de control de versiones como GitHub, Azure DevOps y Bitbucket, lo que permite un an�lisis continuo del c�digo en cada commit o despliegue.

La plataforma destaca por ofrecer un tablero interactivo donde se pueden visualizar y priorizar los problemas detectados, facilitando la refactorizaci�n y la mejora de la calidad del c�digo. SonarCloud permite a los equipos identificar vulnerabilidades y puntos cr�ticos de seguridad, adem�s de generar informes de cobertura de pruebas automatizadas, ayudando a mantener el c�digo limpio, seguro y f�cil de mantener a lo largo del ciclo de vida del desarrollo.

- ### Resumen de M�tricas de Calidad de C�digo en SonarCloud

	1. **Coverage (Cobertura de C�digo)**: Mide el porcentaje de c�digo cubierto por pruebas automatizadas.
	2. **Bugs (Errores)**: Identifica problemas que podr�an causar fallos en el c�digo en tiempo de ejecuci�n.
	3. **Vulnerabilities (Vulnerabilidades)**: Detecta posibles puntos de riesgo que podr�an comprometer la seguridad del c�digo.
	4. **Code Smells**: Indica malas pr�cticas o patrones de c�digo que, aunque no causan fallos, afectan la calidad y mantenibilidad.
	5. **Duplications (Duplicaci�n de C�digo)**: Mide el porcentaje de c�digo duplicado en el proyecto, lo que afecta la mantenibilidad.
	6. **Maintainability (Mantenibilidad)**: Eval�a la facilidad con la que el c�digo puede ser modificado o extendido sin introducir nuevos errores.
	7. **Technical Debt (Deuda T�cnica)**: Estima el tiempo necesario para corregir todos los problemas de calidad del c�digo.
	8. **Reliability (Fiabilidad)**: Mide la probabilidad de que el c�digo funcione sin errores.
	9. **Security Hotspots (Puntos Cr�ticos de Seguridad)**: Indica �reas del c�digo que deben ser revisadas manualmente por posibles problemas de seguridad.
	10. **Cyclomatic Complexity (Complejidad Ciclom�tica)**: Mide el n�mero de caminos l�gicos independientes en el c�digo, indicando su complejidad estructural.
	11. **Cognitive Complexity (Complejidad Cognitiva)**: Eval�a la dificultad de entender el c�digo, considerando aspectos de legibilidad y comprensi�n.
	12. **Maintainability Rating**: Clasifica el proyecto en una escala de A a E seg�n la deuda t�cnica y la facilidad de mantenimiento.
	13. **Branch Coverage**: Mide la cobertura de las ramas de decisi�n en el c�digo (por ejemplo, en estructuras `if`, `else` o `switch`).
	14. **Complexity per Function**: Mide la complejidad promedio de las funciones dentro del proyecto.
	15. **Complexity per Class**: Mide la complejidad promedio por clase, ayudando a evaluar la arquitectura y el dise�o del sistema.
	16. **Lines of Code (LoC)**: Cuenta el n�mero total de l�neas de c�digo en el proyecto, excluyendo comentarios y espacios en blanco.
	17. **Comment Density (Densidad de Comentarios)**: Mide el porcentaje de l�neas de c�digo que son comentarios, �til para evaluar la documentaci�n interna.
	18. **Issues (Problemas)**: Cuenta el n�mero total de problemas detectados en el c�digo, que pueden clasificarse como bugs, vulnerabilidades o code smells.
	19. **Security Rating**: Proporciona una clasificaci�n general de la seguridad del c�digo, en una escala de A (mejor) a E (peor).
	20. **Reliability Rating**: Clasifica la confiabilidad del c�digo en una escala de A a E, basada en la cantidad y gravedad de los errores detectados.
	21. **Security Remediation Effort**: Estima el tiempo necesario para resolver todos los problemas de seguridad identificados en el c�digo.
	22. **Uncovered Conditions**: Mide el n�mero de condiciones l�gicas que no est�n cubiertas por las pruebas automatizadas.
	23. **Duplicated Blocks**: Cuenta los bloques de c�digo que se repiten en el proyecto.
	24. **Unresolved Issues**: Indica el n�mero de problemas detectados que a�n no han sido resueltos.
	
- Desarrollo del punto 4.2: Demostraci�n de c�mo integrar SonarCloud en un pipeline de CI/CD y c�mo leer los reportes de an�lisis est�tico.
	- ##### 4.2.1 Integraremos SonarCloud para analizar el c�digo fuente. Configurar SonarCloud en nuestro pipeline siguiendo instructivo 5.1
	  - Antes de nuestra tarea de Build del Back:
		```yaml
		    
	 	    - task: SonarCloudPrepare@2
		      inputs:
		        SonarCloud: 'SonarCloud' #Nombre de nuestra Service Connection a SonarCloud
		        organization: 'ingsoft3ucc'  #Nombre de nuestra organizacion SonarCloud
		        scannerMode: 'MSBuild'
		        projectKey: 'Angular_WebAPINetCore8_CRUD_Sample'  #Key de nuestro proyecto en SonarCloud
		        projectName: 'Angular_WebAPINetCore8_CRUD_Sample' #Nombre de nuestro proyecto en SonarCloud
		      displayName: 'Prepare SonarCloud'
	        
		 ```

  	  - Despues de nuestra tarea de Build del Back:
		   ```yaml
		     - task: SonarCloudAnalyze@2
		       inputs:
		         jdkversion: 'JAVA_HOME_17_X64'
		       displayName: 'Analyze SonarCloud'
		      
		    - task: SonarCloudPublish@2
		      displayName: 'Publish SonarCloud'
		      inputs:
		        pollingTimeoutSec: '300'
	    ```
  	- ##### 4.2.2 Vemos el resultado de nuestro pipeline, en extensions tenemos un link al an�lisis realizado por SonarCloud

  	![image](https://github.com/user-attachments/assets/312a3c9f-659e-4249-b204-aa1abd312cc2)
  
	- ##### 4.2.3 Ir al link y analizar toda la informaci�n obtenida. Detallar en la entrega del TP los puntos m�s relevantes del informe, qu� significan y para qu� sirven.

  	![image](https://github.com/user-attachments/assets/6e1ce439-5f57-41b9-8624-1491d57120bd)


#### 4.3 Pruebas de Integraci�n con Cypress:

**Cypress** https://www.cypress.io/ es una herramienta de pruebas tanto de integraci�n como unitarias dise�ada para probar aplicaciones web. Facilita la escritura, ejecuci�n y depuraci�n de pruebas autom�ticas en tiempo real dentro del navegador. A diferencia de otras herramientas de pruebas, **Cypress** se ejecuta directamente en el navegador, lo que le permite interactuar con la p�gina web de manera m�s eficiente, proporcionando feedback r�pido y detallado sobre las pruebas.

##### **Caracter�sticas clave de Cypress**:
- **Pruebas en tiempo real**: Ver los resultados de las pruebas en el navegador mientras se ejecutan.
- **Automatizaci�n completa del navegador**: Cypress controla el navegador para simular la interacci�n del usuario.
- **R�pida retroalimentaci�n**: Ideal para desarrollo �gil con integraci�n continua.
- **Depuraci�n sencilla**: Acceso a capturas de pantalla y videos de las pruebas fallidas.

### **Comandos m�s comunes de Cypress**

| **Comando**                  | **Descripci�n**                                              | **Ejemplo**                                        |
|------------------------------|--------------------------------------------------------------|----------------------------------------------------|
| `cy.visit(url)`               | Carga la p�gina web especificada.                            | `cy.visit('https://example.com')`                  |
| `cy.get(selector)`            | Selecciona un elemento del DOM usando un selector CSS.       | `cy.get('button')`                                 |
| `cy.click()`                  | Simula un clic en el elemento seleccionado.                  | `cy.get('button').click()`                         |
| `cy.type(text)`               | Escribe el texto en el campo seleccionado.                   | `cy.get('input[name="username"]').type('user123')`  |
| `cy.should(condition)`        | Verifica que un elemento cumple con una condici�n.           | `cy.get('h1').should('contain', 'Welcome')`        |
| `cy.wait(time)`               | Espera una cantidad de milisegundos antes de continuar.      | `cy.wait(2000)`                                    |
| `cy.intercept(method, url)`   | Intercepta una solicitud de red para pruebas.                | `cy.intercept('GET', '/api/data').as('getData')`   |
| `cy.reload()`                 | Recarga la p�gina actual.                                    | `cy.reload()`                                      |
| `cy.go(direction)`            | Navega hacia atr�s o adelante en el historial del navegador. | `cy.go('back')`                                    |
| `npx cypress run`             | Ejecuta las pruebas en modo "headless".                      | `npx cypress run`                                  |
| `assert.equal(value1, value2)` | Verifica que dos valores sean iguales.                      | `assert.equal(4, 4)`                               |
| `assert.deepEqual(obj1, obj2)` | Verifica que dos objetos sean iguales.                      | `assert.deepEqual({a:1}, {a:1})`                   |
| `assert.isTrue(value)`         | Verifica que el valor sea verdadero.                        | `assert.isTrue(true)`                              |
| `assert.exists(value)`         | Verifica que un valor exista (no sea `null` o `undefined`). | `assert.exists('text')`                            |
| `cy.should('be.visible')`      | Verifica que un elemento sea visible.                       | `cy.get('button').should('be.visible')`            |
| `cy.should('not.be.visible')`  | Verifica que un elemento no sea visible.                    | `cy.get('.modal').should('not.be.visible')`        |
| `cy.should('exist')`           | Verifica que un elemento existe en el DOM.                  | `cy.get('#menu').should('exist')`                  |
| `cy.should('not.exist')`       | Verifica que un elemento no existe en el DOM.               | `cy.get('#loading').should('not.exist')`           |

- Desarrollo del punto 4.4: 
	- ##### 4.3.1 En el directorio raiz de nuestro proyecto Angular instalar el siguiente paquete:
  	```bash
	npm install cypress --save-dev
   	```
   	- ##### 4.3.2 Abrir Cypress:
  	```bash
   	npx cypress open
   	```
	- ##### 4.3.3 Inicializar Cypress en nuestro proyecto como se indica en el instructivo 5.2
   	Esto crear� autom�ticamente una estructura de carpetas dentro de tu proyecto. 
	![image](https://github.com/user-attachments/assets/b0d3de46-91b2-4d19-bff0-7998a78c1521)

	 - cypress/e2e: Aqu� es donde se almacenan tus archivos de prueba.
	 - cypress/fixtures: Aqu� se almacenan los datos de prueba que puedes usar en tus tests.
	 - cypress/support: Contiene archivos de configuraci�n y comandos personalizados.

  	- ##### 4.3.4 Crear nuestra primera prueba navegando a nuestro front.
 	En la carpeta cypress/e2e, crear un archivo con el nombre primer_test.js y agregar el siguiente c�digo para probar la p�gina de inicio de nuestro front:
  	```js
	  describe('Mi primera prueba', () => {
	  it('Carga correctamente la p�gina de ejemplo', () => {
	    cy.visit('https://as-crud-web-api-qa.azurewebsites.net/') // Colocar la url local o de Azure de nuestro front
	    cy.get('h1').should('contain', 'EmployeeCrudAngular') // Verifica que el t�tulo contenga "EmployeeCrudAngular"
	  })
	})
   	```
	- ##### 4.3.5 Correr nuestra primera prueba
 	Si est� abierta la interfaz gr�fica de Cypress, aparecer� el archivo primer_test.cy.js en la lista de pruebas. Clic en el archivo para ejecutar la prueba.
  	<img width="1154" alt="image" src="https://github.com/user-attachments/assets/f34a9af2-1692-441a-b1f0-37055ae2d7a0">
   	<img width="1436" alt="image" src="https://github.com/user-attachments/assets/8758c000-2f40-4a05-8cb0-c5fa56d583f9">
	
	Tambi�n es posible ejecutar Cypress en modo "headless" (sin interfaz gr�fica) utilizando el siguiente comando:
	```bash
 	npx cypress run
 	```
 	![image](https://github.com/user-attachments/assets/78ecba2e-9e40-4eb0-a286-56908723dee1)


	- ##### 4.3.6 Modificar nuestra prueba para que falle.
	  - Editamos el archivo primer_test.cy.js y hacemos que espere otra cosa en el t�tulo
	  - Ejecutamos cypress en modo headless
	 ![image](https://github.com/user-attachments/assets/b431354b-0e4f-492e-8c40-2993991ac76d)

	Cypress captura autom�ticamente pantallas cuando una prueba falla. Las capturas de pantalla se guardan en la carpeta `cypress/screenshots`.
	<img width="1136" alt="image" src="https://github.com/user-attachments/assets/e6f26695-def4-4596-b5c6-152f5b95aa59">

 	- ##### 4.3.6 Grabar nuestras pruebas para que Cypress genere c�digo autom�tico y genere reportes:
    	 - Cerramos Cypress
	 - Editamos el archivo cypress.config.ts incluyendo la propiedad **experimentalStudio** en true y la configuraci�n de reporter�a.
	      ```typescript
		import { defineConfig } from "cypress";

		export default defineConfig({
		  e2e: {
		    setupNodeEvents(on, config) {
		      // implement node event listeners here
		    },
		    reporter: 'junit',  // Configura el reporter a JUnit
		    reporterOptions: {
		      mochaFile: 'cypress/results/results-[hash].xml',  // Directorio y nombre de los archivos de resultados
		      toConsole: true,  // Opcional: imprime los resultados en la consola
		      },
		  },
		  experimentalStudio: true,
		});

		```
       - Corremos nuevamente Cypress con npx cypress open, una vez que se ejecute nuestra prueba tendremos la opci�n de "Add Commands to Test". Esto permitir� interactuar con la aplicaci�n y generar autom�ticamente comandos de prueba basados en las interacciones con la p�gina:
         
         <img width="748" alt="image" src="https://github.com/user-attachments/assets/73c287fc-de00-477d-abbf-d905eb11cdb3">

       - Por ejemplo, si agregamos un nuevo empleado y luego verificamos que est� en la lista, Cypress nos generar� un c�digo como este:
 	```typescript
  	escribe('Mi primera prueba', () => {
	  it('Carga correctamente la p�gina de ejemplo', () => {
        cy.visit('https://as-crud-web-api-qa.azurewebsites.net/') // Colocar la url local o de Azure de nuestro front
        cy.get('h1').should('contain', 'EmployeeCrudAngular') // Verifica que el t�tulo contenga "EmployeeCrudAngular"
        /* ==== Generated with Cypress Studio ==== */
        cy.get('.btn').click();
        cy.get('.form-control').clear('O');
        cy.get('.form-control').type('Oso Pratto');
        cy.get('.btn').click();
        cy.get(':nth-child(15) > :nth-child(2)').click();
        cy.get(':nth-child(15) > :nth-child(2)').should('have.text', ' Oso Pratto ');
        /* ==== End Cypress Studio ==== */
      })
	})
 	```
  	- Por supuesto que habr� que hacerle ajustes, como por ejemplo que se fije siempre en la �ltima fila de la grilla y no en la posici�n 15 como lo grab�, es ah� cuando consultando la documentaci�n de Cypress debemos ver c�mo modificar el c�digo, en nuestro caso de ejemplo ser�a as�:
  	  ```typescript
  	  cy.get('tr:last-child > :nth-child(2)').should('have.text', ' Oso Pratto ');
  	  ```
  	- ##### 4.3.7 Hacemos prueba de editar un empleado
    	 - Creamos en cypress/e2e/ un archivo editEmployee_test.cy.js con el siguiente contenido, guardamos y aparecer� en Cypress:
  	       
		```js
  		describe('editEmployeeTest', () => {
			  it('Carga correctamente la p�gina de ejemplo', () => {
		        cy.visit('https://as-crud-web-api-qa.azurewebsites.net/') // Colocar la url local o de Azure de nuestro front
		      })
			})
  	  	```
  	
 	<img width="845" alt="image" src="https://github.com/user-attachments/assets/d37524a6-fcfc-431c-9a69-594bf4dbbc62">
	- Hacemos "Add command to the test" y empezamos a interactuar con la p�gina
 	<img width="679" alt="image" src="https://github.com/user-attachments/assets/e7ad9389-3f68-42fc-95f2-68a7a79a029a">
  
  	- Hacemos algunos ajustes al c�digo generado:
   
  	```js
	describe('editEmployeeTest', () => {
	  it('Edita correctamente un empleado', () => {
	    cy.visit('https://as-crud-web-api-qa.azurewebsites.net/') // URL del front
	
	    // Haz clic en el bot�n de editar empleado
	    cy.get(':nth-child(7) > :nth-child(4) > a > .fa').click();
	
	    // Aseg�rate de que el campo de texto est� visible y habilitado
	    cy.get('.form-control')
	      .should('be.visible')
	      .should('not.be.disabled')
	
	    cy.wait(500);
	
	    cy.get('.form-control').type('{selectall}{backspace}')  // Selecciona todo el texto y lo elimina
	
	    cy.wait(500);
	
	    cy.get('.form-control').should('have.value', '');  // Verifica que el campo est� vac�o
	
	    // Escribe el nuevo nombre 
	    cy.get('.form-control').type('Emilia Schwindt Modified');
	
	    // Env�a el formulario
	    cy.get('.btn').click();
	
	    // Verifica que el nombre fue actualizado correctamente
	    cy.get(':nth-child(7) > :nth-child(2)').should('have.text', ' Emilia Schwindt Modified ');
	  });
	});

  	```

#### 4.4 Desaf�os:
- Integrar en el pipeline SonarCloud para nuestro proyecto Angular, mostrar el resultado obtenido en SonarCloud
- Implementar en Cypress pruebas de integraci�n que incluya los casos desarrollados como pruebas unitarias del front en el TP06.
- Incorporar al pipeline de Deploy la ejecuci�n de las pruebas de integraci�n y la visualizaci�n de sus resultados.
- **Resultado esperado**:
    - Un Pipeline en YAML que incluya a) Build de QA y Front con ejecuci�n y resultado de pruebas de code coverage, pruebas unitarias y an�lisis de Sonar Cloud y b) Deploy a WebApp(s) de QA y Front que incluya ejecuci�n y resultado de pruebas de integraci�n

    <img width="1318" alt="image" src="https://github.com/user-attachments/assets/76324d85-3706-495d-ad56-6054371a3019">

    - Dos Stages: Una para Build, Test Unitarios, Code Coverage y SonarCloud y otra para el Deploy a QA con Tests de Integraci�n

	<img width="296" alt="image" src="https://github.com/user-attachments/assets/03a69736-a1c1-4ce2-a648-4237da015150">

    - En la pesta�a Test, poder visualizar los Test Unitarios de Front y Back y los Test de Integracion:

	<img width="1262" alt="image" src="https://github.com/user-attachments/assets/83fe8613-631d-4c4d-9042-37455701c0e3">

    - En la pesta�a Code Coverage, visualizar la cobertura de las pruebas unitarias de Back y de Front:

	<img width="1199" alt="image" src="https://github.com/user-attachments/assets/e3d48d54-945c-4b99-afbe-6a72b5f95c3f">

    - En la pesta�a Extensions, ver el an�lisis de SonarCloud en verde

       <img width="720" alt="image" src="https://github.com/user-attachments/assets/c5511fb6-3d39-4b9c-ad9a-e688bdaa3016">
    
    - Un documento de una carilla explicando qu� informaci�n pudieron sacar del an�lisis de Sonar Cloud y de las pruebas de cobertura.


