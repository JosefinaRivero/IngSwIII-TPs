### 4- Desarrollo:

#### 4.1 Modificar nuestro pipeline para incluir el deploy en QA y PROD de Imagenes Docker en Servicio Azure App Services con Soporte para Contenedores
- Desarrollo del punto 4.1: 
	
  	- ##### 4.1.1 - Agregar a nuestro pipeline una nueva etapa que dependa de nuestra etapa de Construcción y Pruebas y de la etapa de Construcción de Imagenes Docker y subida a ACR realizada en el TP08
  	    
  	  - Agregar tareas para crear un recurso Azure Container Instances que levante un contenedor con nuestra imagen de back utilizando un AppServicePlan en Linux

![Descripción de la imagen](imagen1.png)
![Descripción de la imagen](imagen2.png)
![Descripción de la imagen](imagen3.png)
![Descripción de la imagen](imagen4.png)
![Descripción de la imagen](imagen5.png)
![Descripción de la imagen](imagen6.png)



  	  
  	     
#### 4.2 Desafíos:
- 4.2.1 Agregar tareas para generar Front en Azure App Service con Soporte para Contenedores

![Descripción de la imagen](imagen7.png)
![Descripción de la imagen](imagen8.png)
![Descripción de la imagen](imagen9.png)
![Descripción de la imagen](imagen10.png)

- 4.2.2 Agregar variables necesarias para el funcionamiento de la nueva etapa considerando que debe haber 2 entornos QA y PROD para Back y Front.

![Descripción de la imagen](imagen11.png)

- 4.2.3 Agregar tareas para correr pruebas de integración en el entorno de QA de Back y Front creado en Azure App Services con Soporte para Contenedores. 

![Descripción de la imagen](imagen12.png)
![Descripción de la imagen](imagen13.png)

- 4.2.4 Agregar etapa que dependa de la etapa de Deploy en QA que genere un entorno de PROD.

![Descripción de la imagen](imagen14.png)
![Descripción de la imagen](imagen15.png)
![Descripción de la imagen](imagen16.png)
![Descripción de la imagen](imagen17.png)
![Descripción de la imagen](imagen18.png)

- 4.2.5 Entregar un pipeline que incluya:
  - A) Etapa Construcción y Pruebas Unitarias y Code Coverage Back y Front
  - B) Construcción de Imágenes Docker y subida a ACR
  - C) Deploy Back y Front en QA con pruebas de integración para Azure Web Apps
  - D) Deploy Back y Front en QA con pruebas de integración para ACI
  - E) Deploy Back y Front en QA con pruebas de integración para Azure Web Apps con Soporte para contenedores
  - F) Aprobación manual de QA para los puntos C,D,E
  - G) Deploy Back y Front en PROD para Azure Web Apps
  - H) Deploy Back y Front en PROD para ACI
  - I) Deploy Back y Front en PROD para Azure Web Apps con Soporte para contenedores


![Descripción de la imagen](imagen19.png)
![Descripción de la imagen](imagen20.png)
![Descripción de la imagen](imagen21.png)
![Descripción de la imagen](imagen22.png)











```yaml
		trigger:
  - main

pool:
  vmImage: 'windows-latest'


variables:
  solution: '**/*.sln'
  buildPlatform: 'Any CPU'
  buildConfiguration: 'Release'
  frontPath: './EmployeeCrudAngular'

  # AZURE VARIABLES
  ConnectedServiceName: 'ServiceConnectionARM'
  acrLoginServer: 'jringsoft3uccacr.azurecr.io'
  backImageName: 'employee-crud-api'
  frontImageName: 'employee-crud-front'
  
  acrName: 'jringsoft3uccacr'
  ResourceGroupName: 'TPS-IngSoft3UCC2024'
  backContainerInstanceNameQA: 'jr-crud-api-qa'
  backImageTag: 'latest'
  container-cpu-api-qa: 1
  container-memory-api-qa: 1.5

  #CONTAINER INSTANCES

  frontContainerInstanceNameQA: 'jr-crud-front-qa'
  frontImageTag: 'latest' 
  container-cpu-front-qa: 1
  container-memory-front-qa: 1.5

  backContainerInstanceNameprod: 'jr-crud-api-prod'
  backprodImageTag: 'latest'

  container-cpu-api-prod: 1
  container-memory-api-prod: 1.5

  frontContainerInstanceNameprod: 'jr-crud-front-prod'
  frontprodImageTag: 'latest' 
  container-cpu-front-prod: 1
  container-memory-front-prod: 1.5
  
  baseUrl: 'http://jr-crud-front-qa.eastus.azurecontainer.io'

  #APP SERVICES
  AppServicePlanLinux: 'MiAppPlanJR'
  AppServicesApiNameContainersQA: 'jr-back-web'
  AppServicesFrontNameContainersQA: 'jr-front-web'

  AppServicesApiNameContainersProd: 'jr-back-web-prod'
  AppServicesFrontNameContainersProd: 'jr-front-web-prod'

 # WEB APPS
  WebAppApiNameQA: 'JR-Backend-QA'
  api_url_wa_qa: 'https://$(WebAppApiNameQA).azurewebsites.net/api/Employee'

  WebAppFrontNameQA: 'JR-Frontend-QA'
  front_url_wa_qa: 'https://$(WebAppFrontNameQA).azurewebsites.net'

  WebAppApiNameProd: 'JR-Backend-Prod'
  api_url_wa_prod: 'https://$(WebAppApiNameProd).azurewebsites.net/api/Employee'

  WebAppFrontNameProd: 'JR-Frontend-Prod'
  front_url_wa_prod: 'https://$(WebAppFrontNameProd).azurewebsites.net'




stages:
  #----------------------------------------------------------
  # ### STAGE DE BUILD Y TEST DEL BACKEND Y FRONTEND
  #----------------------------------------------------------
  - stage: BuildAndTestBackAndFront
    displayName: "Build and Test API and Front"
    jobs:
      # Job para compilar y probar la API .NET
      - job: BuildDotnet
        displayName: "Build and Test API"
        pool:
          vmImage: 'windows-latest'
        steps:
          - checkout: self
            fetchDepth: 0

          - task: NuGetToolInstaller@1
            inputs:
              versionSpec: '>=5.8'

          - task: NuGetCommand@2
            displayName: 'Restaurar paquetes NuGet'
            inputs:
              restoreSolution: '$(solution)'


          - task: DotNetCoreCLI@2
            displayName: 'Compilar la API'
            inputs:
              command: build
              projects: '$(solution)'
              arguments: '--configuration $(buildConfiguration)'

          - task: DotNetCoreCLI@2
            displayName: 'Ejecutar pruebas de la API'
            inputs:
              command: test
              projects: '**/*.Tests.csproj'
              arguments: '--collect:"XPlat Code Coverage"'

          - task: PublishCodeCoverageResults@2
            displayName: 'Publicar resultados de code coverage del back-end'
            inputs:
              summaryFileLocation: '$(Agent.TempDirectory)/**/*.cobertura.xml'
              failIfCoverageEmpty: false

          - task: DotNetCoreCLI@2
            displayName: 'Publicar aplicación'
            inputs:
              command: publish
              publishWebProjects: True
              arguments: '--configuration $(buildConfiguration) --no-restore --output $(Build.ArtifactStagingDirectory)/drop-back'
              zipAfterPublish: false

          - task: PublishBuildArtifacts@1
            displayName: 'Publicar artefactos de compilación'
            inputs:
              PathtoPublish: '$(Build.ArtifactStagingDirectory)/drop-back'
              ArtifactName: 'drop-back'
              publishLocation: 'Container'

          - task: PublishPipelineArtifact@1
            displayName: 'Publicar Dockerfile de Back'
            inputs:
              targetPath: '$(Build.SourcesDirectory)/docker/api/dockerfile'
              artifact: 'dockerfile-back'

      # Job para compilar y probar el frontend Angular
      - job: BuildAngular
        displayName: "Build and Test Angular"
        pool:
          vmImage: 'ubuntu-latest'
        steps:
          - task: NodeTool@0
            displayName: 'Instalar Node.js'
            inputs:
              versionSpec: '22.x'

          - script: npm install
            displayName: 'Instalar dependencias'
            workingDirectory: $(frontPath)

          - script: npx ng test --karma-config=karma.conf.js --watch=false --browsers ChromeHeadless --code-coverage
            displayName: 'Ejecutar pruebas del front'
            workingDirectory: $(frontPath)
            continueOnError: true

          - task: PublishCodeCoverageResults@2
            displayName: 'Publicar resultados de code coverage del front'
            inputs:
              summaryFileLocation: '$(frontPath)/coverage/lcov.info'
              failIfCoverageEmpty: false
            condition: always()

          - task: PublishTestResults@2
            displayName: 'Publicar resultados de pruebas unitarias del front'
            inputs:
              testResultsFormat: 'JUnit'
              testResultsFiles: '$(frontPath)/test-results/test-results.xml'
              failTaskOnFailedTests: true
            condition: always()

          - script: npm run build
            displayName: 'Compilar el proyecto Angular'
            workingDirectory: $(frontPath)

          - task: PublishBuildArtifacts@1
            displayName: 'Publicar artefactos Angular'
            inputs:
              PathtoPublish: '$(frontPath)/dist'
              ArtifactName: 'drop-front'

          - task: PublishPipelineArtifact@1
            displayName: 'Publicar Dockerfile de Front'
            inputs:
              targetPath: '$(Build.SourcesDirectory)/docker/front/dockerfile'
              artifact: 'dockerfile-front'

  #----------------------------------------------------------
  # ### STAGE BUILD DOCKER IMAGES Y PUSH A AZURE CONTAINER REGISTRY
  #----------------------------------------------------------
  - stage: DockerBuildAndPush
    displayName: 'Construir y Subir Imágenes Docker a ACR'
    dependsOn: BuildAndTestBackAndFront
    jobs:
      - job: docker_build_and_push
        displayName: 'Construir y Subir Imágenes Docker a ACR'
        pool:
          vmImage: 'ubuntu-latest'
          
        steps:
          - checkout: self

          - task: DownloadPipelineArtifact@2
            displayName: 'Descargar Artefactos de Back'
            inputs:
              buildType: 'current'
              artifactName: 'drop-back'
              targetPath: '$(Pipeline.Workspace)/drop-back'
          
          - task: DownloadPipelineArtifact@2
            displayName: 'Descargar Dockerfile de Back'
            inputs:
              buildType: 'current'
              artifactName: 'dockerfile-back'
              targetPath: '$(Pipeline.Workspace)/dockerfile-back'

          - task: AzureCLI@2
            displayName: 'Iniciar Sesión en Azure Container Registry (ACR)'
            inputs:
              azureSubscription: '$(ConnectedServiceName)'
              scriptType: bash
              scriptLocation: inlineScript
              inlineScript: |
                az acr login --name $(acrLoginServer)
      
          - task: Docker@2
            displayName: 'Construir Imagen Docker para Back'
            inputs:
              command: build
              repository: $(acrLoginServer)/$(backImageName)
              dockerfile: $(Pipeline.Workspace)/dockerfile-back/dockerfile
              buildContext: $(Pipeline.Workspace)/drop-back
              tags: 'latest'

          - task: Docker@2
            displayName: 'Subir Imagen Docker de Back a ACR'
            inputs:
              command: push
              repository: $(acrLoginServer)/$(backImageName)
              tags: 'latest'

      - job: docker_build_and_push_front
        displayName: 'Construir y Subir Imagen Docker de Front a ACR'
        pool:
          vmImage: 'ubuntu-latest'
          
        steps:
          - checkout: self

          - task: DownloadPipelineArtifact@2
            displayName: 'Descargar Artefactos de Front'
            inputs:
              buildType: 'current'
              artifactName: 'drop-front'
              targetPath: '$(Pipeline.Workspace)/drop-front'
          
          - task: DownloadPipelineArtifact@2
            displayName: 'Descargar Dockerfile de Front'
            inputs:
              buildType: 'current'
              artifactName: 'dockerfile-front'
              targetPath: '$(Pipeline.Workspace)/dockerfile-front'

          - task: AzureCLI@2
            displayName: 'Iniciar Sesión en Azure Container Registry (ACR)'
            inputs:
              azureSubscription: '$(ConnectedServiceName)'
              scriptType: bash
              scriptLocation: inlineScript
              inlineScript: |
                az acr login --name $(acrLoginServer)
      
          - task: Docker@2
            displayName: 'Construir Imagen Docker para Front'
            inputs:
              command: build
              repository: $(acrLoginServer)/$(frontImageName)
              dockerfile: $(Pipeline.Workspace)/dockerfile-front/dockerfile
              buildContext: $(Pipeline.Workspace)/drop-front
              tags: 'latest'

          - task: Docker@2
            displayName: 'Subir Imagen Docker de Front a ACR'
            inputs:
              command: push
              repository: $(acrLoginServer)/$(frontImageName)
              tags: 'latest'

  # -------------------------------------------------------------------------------
  # |       STAGE DEPLOY FRONTEND AND BACKEND TO AZURE WEB APPS QA                 |
  # -------------------------------------------------------------------------------
  - stage: DeployToWebAppQA
    displayName: 'Deploy to Azure Web APPs (QA)'
    dependsOn: BuildAndTestBackAndFront
    condition: succeeded()
    pool:
      vmImage: 'windows-latest'
    
    jobs:
      # -------------------------------------------------------------------------------
      # |               DEPLOY API TO AZURE WEB APP                                    |
      # -------------------------------------------------------------------------------
      - job: DeployBackQA
        displayName: 'Deploy Backend to Azure Web APP (QA)'
        steps:
          - task: DownloadBuildArtifacts@1
            displayName: 'Download API artifacts'
            inputs:
              buildType: 'current'
              downloadType: 'single'
              artifactName: 'drop-back'  # Ensure 'drop-back' matches the published artifact name
              downloadPath: '$(System.ArtifactsDirectory)'
            
          - task: AzureCLI@2
            displayName: 'Crear App Service si no existe'
            inputs:
              azureSubscription: '$(ConnectedServiceName)'
              scriptType: bash
              scriptLocation: inlineScript
              inlineScript: |
                # Verifica si la App Service existe
                appExists=$(az webapp show --name $(WebAppApiNameQA) --resource-group $(ResourceGroupName) --query "name" --output tsv)
                if [ -z "$appExists" ]; then
                  echo "La App Service no existe. Creándola..."
                  az webapp create --resource-group $(ResourceGroupName) --plan $(AppServicePlanLinux) --name $(WebAppApiNameQA) --runtime "DOTNETCORE|6.0"
                else
                  echo "La App Service ya existe."
                fi



          - task: AzureRmWebAppDeployment@4
            displayName: 'Deploy API To Azure Web App'
            inputs:
              ConnectionType: 'AzureRM'
              azureSubscription: '$(ConnectedServiceName)'  # Make sure this variable is correctly set
              appType: 'webApp'
              WebAppName: $(WebAppApiNameQA)  # Variable for the API web app name
              packageForLinux: '$(System.ArtifactsDirectory)/drop-back/EmployeeCrudApi'
              AppSettings: '-DBCONNECTIONSTRING "$(cnn_string_qa)"'

      # -------------------------------------------------------------------------------
      # |               DEPLOY FRONT TO AZURE WEB APP                                 |
      # -------------------------------------------------------------------------------
      - job: DeployFrontQA
        displayName: 'Deploy Frontend to Azure Web APP (QA)'
        steps:
          - task: DownloadBuildArtifacts@1
            displayName: 'Download Frontend artifacts'
            inputs:
              buildType: 'current'
              downloadType: 'single'
              artifactName: 'drop-front'  # Ensure 'drop-front' matches the published artifact name
              downloadPath: '$(System.ArtifactsDirectory)'
          
          - task: AzureCLI@2
            displayName: 'Crear App Service si no existe'
            inputs:
              azureSubscription: '$(ConnectedServiceName)'
              scriptType: bash
              scriptLocation: inlineScript
              inlineScript: |
                # Verifica si la App Service existe
                appExists=$(az webapp show --name $(WebAppFrontNameQA) --resource-group $(ResourceGroupName) --query "name" --output tsv)
                if [ -z "$appExists" ]; then
                  echo "La App Service no existe. Creándola..."
                  az webapp create --resource-group $(ResourceGroupName) --plan $(AppServicePlanLinux) --name $(WebAppFrontNameQA) --runtime "DOTNETCORE|6.0"
                else
                  echo "La App Service ya existe."
                fi
          - script: |
              echo window['env'] = { apiUrl: '$(API_URL)' }; > $(System.ArtifactsDirectory)/drop-front/employee-crud-angular/browser/assets/env.js
            displayName: 'Set env.js content'
          
          - task: AzureRmWebAppDeployment@4
            displayName: 'Deploy Frontend To Azure Web App'
            inputs:
              ConnectionType: 'AzureRM'
              azureSubscription: '$(ConnectedServiceName)'  # Make sure this variable is correctly set
              appType: 'webApp'
              WebAppName: $(WebAppFrontNameQA)  # Variable for the frontend web app name
              packageForLinux: '$(System.ArtifactsDirectory)/drop-front/employee-crud-angular/browser'

 # -------------------------------------------------------------------------------
  # |              RUN INTEGRATION TESTS ON AZURE WEB APPS                         |
  # -------------------------------------------------------------------------------
      - job: RunCypressTests
        displayName: 'Run Cypress Tests on Web Apps'
        dependsOn: [DeployFrontQA, DeployBackQA]
             
        variables:
            baseUrl: 'http://JR-Frontend-QA.azurewebsites.net'

        condition: succeeded()
        steps:
            - script: npm install ts-node typescript --save-dev
              displayName: 'Install typescript'
              workingDirectory: ./EmployeeCrudAngular

            - script: npx cypress run --config-file cypress.config.ts  --env baseUrl=$(baseUrl)
              workingDirectory: ./EmployeeCrudAngular
              displayName: 'Run integration tests'
              continueOnError: true
            - task: PublishTestResults@2
              inputs:
                testResultsFormat: 'JUnit'
                testResultsFiles: '*.xml'
                searchFolder: '$(frontPath)/cypress/results'
                testRunTitle: 'Cypress Integration Tests'

  # -------------------------------------------------------------------------------
  # |       STAGE DEPLOY FRONTEND AND BACKEND TO AZURE WEB APPS PROD               |
  # -------------------------------------------------------------------------------
  - stage: DeployToWebAppProd
    displayName: 'Deploy to Azure Web APPs (Prod)'
    dependsOn: DeployToWebAppQA
    condition: succeeded()
    pool:
      vmImage: 'windows-latest'
      
    jobs:
      # -------------------------------------------------------------------------------
      # |               DEPLOY API TO AZURE WEB APP                                    |
      # -------------------------------------------------------------------------------
      - deployment: DeployBack
        displayName: 'Deploy Backend to Azure Web APP (Prod)'
        environment: 'PROD'
        strategy:
          runOnce:
            deploy:
              steps:
                - task: DownloadBuildArtifacts@1
                  displayName: 'Download API artifacts'
                  inputs:
                    buildType: 'current'
                    downloadType: 'single'
                    artifactName: 'drop-back'  # Ensure 'drop-back' matches the published artifact name
                    downloadPath: '$(System.ArtifactsDirectory)'

                - task: AzureCLI@2
                  displayName: 'Crear App Service si no existe'
                  inputs:
                            azureSubscription: '$(ConnectedServiceName)'
                            scriptType: bash
                            scriptLocation: inlineScript
                            inlineScript: |
                              # Verifica si la App Service existe
                              appExists=$(az webapp show --name $(WebAppApiNameProd) --resource-group $(ResourceGroupName) --query "name" --output tsv)
                              if [ -z "$appExists" ]; then
                                echo "La App Service no existe. Creándola..."
                                az webapp create --resource-group $(ResourceGroupName) --plan $(AppServicePlanLinux) --name $(WebAppApiNameProd) --runtime "DOTNETCORE|6.0"
                              else
                                echo "La App Service ya existe."
                              fi


                - task: AzureRmWebAppDeployment@4
                  displayName: 'Deploy API To Azure Web App'
                  inputs:
                    ConnectionType: 'AzureRM'
                    azureSubscription: '$(ConnectedServiceName)'  # Make sure this variable is correctly set
                    appType: 'webApp'
                    WebAppName: $(WebAppApiNameProd)
                    packageForLinux: '$(System.ArtifactsDirectory)/drop-back/EmployeeCrudApi'
                    AppSettings: '-DBCONNECTIONSTRING "$(cnn_string_qa)"'

      # -------------------------------------------------------------------------------
      # |               DEPLOY FRONT TO AZURE WEB APP                                 |
      # -------------------------------------------------------------------------------
      - deployment: DeployFront
        displayName: 'Deploy Frontend to Azure Web APP (PROD)'
        environment: 'PROD'
        strategy:
          runOnce:
            deploy:
              steps:
                - task: DownloadBuildArtifacts@1
                  displayName: 'Download Frontend artifacts'
                  inputs:
                    buildType: 'current'
                    downloadType: 'single'
                    artifactName: 'drop-front'  # Ensure 'drop-front' matches the published artifact name
                    downloadPath: '$(System.ArtifactsDirectory)'
                
                - task: AzureCLI@2
                  displayName: 'Crear App Service si no existe'
                  inputs:
                            azureSubscription: '$(ConnectedServiceName)'
                            scriptType: bash
                            scriptLocation: inlineScript
                            inlineScript: |
                              # Verifica si la App Service existe
                              appExists=$(az webapp show --name $(WebAppFrontNameProd) --resource-group $(ResourceGroupName) --query "name" --output tsv)
                              if [ -z "$appExists" ]; then
                                echo "La App Service no existe. Creándola..."
                                az webapp create --resource-group $(ResourceGroupName) --plan $(AppServicePlanLinux) --name $(WebAppFrontNameProd) --runtime "DOTNETCORE|6.0"
                              else
                                echo "La App Service ya existe."
                              fi
               
                
                - script: |
                    echo window['env'] = { apiUrl: '$(api_url_wa_prod)' }; > $(System.ArtifactsDirectory)/drop-front/employee-crud-angular/browser/assets/env.js
                  displayName: 'Set env.js content'
                
                - task: AzureRmWebAppDeployment@4
                  displayName: 'Deploy Frontend To Azure Web App'
                  inputs:
                    ConnectionType: 'AzureRM'
                    azureSubscription: '$(ConnectedServiceName)'  # Make sure this variable is correctly set
                    appType: 'webApp'
                    WebAppName: $(WebAppFrontNameProd)
                    packageForLinux: '$(System.ArtifactsDirectory)/drop-front/employee-crud-angular/browser'


    #----------------------------------------------------------
  # ### STAGE DEPLOY TO ACI QA
  #----------------------------------------------------------
  - stage: DeployToACIQA
    displayName: 'Desplegar en Azure Container Instances (ACI) QA'
    dependsOn: DockerBuildAndPush 
    jobs:
      - job: deploy_to_aci_qa
        displayName: 'Desplegar en Azure Container Instances (ACI) QA'
        pool:
          vmImage: 'ubuntu-latest'
        steps:
      

        - task: AzureCLI@2
          displayName: 'Desplegar Imagen Docker de Back en ACI QA'
          inputs:
            azureSubscription: '$(ConnectedServiceName)'
            scriptType: bash
            scriptLocation: inlineScript
            inlineScript: |
              echo "Resource Group: $(ResourceGroupName)"
              echo "Container Instance Name: $(backContainerInstanceNameQA)"
              echo "ACR Login Server: $(acrLoginServer)"
              echo "Image Name: $(backImageName)"
              echo "Image Tag: $(backImageTag)"
              echo "Connection String: $(cnn_string_qa)"
          
              az container delete --resource-group $(ResourceGroupName) --name $(backContainerInstanceNameQA) --yes

              az container create --resource-group $(ResourceGroupName) \
                --name $(backContainerInstanceNameQA) \
                --image $(acrLoginServer)/$(backImageName):$(backImageTag) \
                --registry-login-server $(acrLoginServer) \
                --registry-username $(acrName) \
                --registry-password $(az acr credential show --name $(acrName) --query "passwords[0].value" -o tsv) \
                --dns-name-label $(backContainerInstanceNameQA) \
                --ports 80 \
                --environment-variables ConnectionStrings__DefaultConnection="$(cnn_string_qa)" \
                --restart-policy Always \
                --cpu $(container-cpu-api-qa) \
                --memory $(container-memory-api-qa)

      - job: deploy_front_to_aci_qa
        displayName: 'Desplegar Frontend en Azure Container Instances (ACI) QA'
        pool:
         vmImage: 'ubuntu-latest'
        
        steps:

        - task: AzureCLI@2
          displayName: 'Desplegar Imagen Docker de Front en ACI QA'
          inputs:
            azureSubscription: '$(ConnectedServiceName)'
            scriptType: bash
            scriptLocation: inlineScript
            inlineScript: |
              echo "Resource Group: $(ResourceGroupName)"
              echo "Container Instance Name: $(frontContainerInstanceNameQA)"
              echo "ACR Login Server: $(acrLoginServer)"
              echo "Image Name: $(frontImageName)"
              echo "Image Tag: $(frontImageTag)"
          
              az container delete --resource-group $(ResourceGroupName) --name $(frontContainerInstanceNameQA) --yes

              az container create --resource-group $(ResourceGroupName) \
                --name $(frontContainerInstanceNameQA) \
                --image $(acrLoginServer)/$(frontImageName):$(frontImageTag) \
                --registry-login-server $(acrLoginServer) \
                --registry-username $(acrName) \
                --registry-password $(az acr credential show --name $(acrName) --query "passwords[0].value" -o tsv) \
                --dns-name-label $(frontContainerInstanceNameQA) \
                --ports 80 \
                --environment-variables API_URL="$(API_URL)" \
                --restart-policy Always \
                --cpu $(container-cpu-front-qa) \
                --memory $(container-memory-front-qa)
          
        # job para los Test de Integración

      - job: RunCypressTests
        displayName: 'Run Cypress Tests'
        dependsOn: [deploy_front_to_aci_qa, deploy_to_aci_qa]
        condition: succeeded()
        
        steps:
        - script: npm install typescript ts-node
          workingDirectory: ./EmployeeCrudAngular
          displayName: 'Instalar TypeScript'
        - script: npx cypress run --config-file cypress.config.ts --env baseUrl=$(baseUrl)
          workingDirectory: ./EmployeeCrudAngular
          displayName: 'Correr Tests en Cypress E2E'

        - task: PublishTestResults@2
          displayName: 'Publicar Resultados de Cypress'
          inputs:
            testResultsFiles: '$(Build.SourcesDirectory)/EmployeeCrudAngular/cypress/results/*.xml'
            testRunTitle: 'Cypress E2E Tests - QA'
            failTaskOnFailedTests: true

  #----------------------------------------------------------
    # ### STAGE DEPLOY TO ACI PROD
    #----------------------------------------------------------
  - stage: DeployToACIPROD
    displayName: 'Desplegar en Azure Container Instances (ACI) prod'
    dependsOn: DeployToACIQA 
    jobs:
      - deployment: deploy_to_aci_prod  # Cambia 'job' a 'deployment'
        displayName: 'Desplegar en Azure Container Instances (ACI) prod'
        environment: 'PROD'  # Colocar 'environment' aquí
        strategy: 
          runOnce:
            deploy:
              steps:  # Los pasos de despliegue van aquí
                - task: AzureCLI@2
                  displayName: 'Desplegar Imagen Docker de Back en ACI prod'
                  inputs:
                    azureSubscription: '$(ConnectedServiceName)'
                    scriptType: bash
                    scriptLocation: inlineScript
                    inlineScript: |
                      echo "Resource Group: $(ResourceGroupName)"
                      echo "Container Instance Name: $(backContainerInstanceNameprod)"
                      echo "ACR Login Server: $(acrLoginServer)"
                      echo "Image Name: $(backImageName)"
                      echo "Image Tag: $(backprodImageTag)"
                      echo "Connection String: $(cnn_string_qa)"
                  
                      az container delete --resource-group $(ResourceGroupName) --name $(backContainerInstanceNameprod) --yes

                      az container create --resource-group $(ResourceGroupName) \
                        --name $(backContainerInstanceNameprod) \
                        --image $(acrLoginServer)/$(backImageName):$(backprodImageTag) \
                        --registry-login-server $(acrLoginServer) \
                        --registry-username $(acrName) \
                        --registry-password $(az acr credential show --name $(acrName) --query "passwords[0].value" -o tsv) \
                        --dns-name-label $(backContainerInstanceNameprod) \
                        --ports 80 \
                        --environment-variables ConnectionStrings__DefaultConnection="$(cnn_string_qa)" \
                        --restart-policy Always \
                        --cpu $(container-cpu-api-prod) \
                        --memory $(container-memory-api-prod)

      - deployment: deploy_front_to_aci_prod  # Cambia 'job' a 'deployment'
        displayName: 'Desplegar Frontend en Azure Container Instances (ACI) prod'
        environment: 'PROD'  # Colocar 'environment' aquí
        strategy: 
          runOnce:
            deploy:
              steps:  # Los pasos de despliegue van aquí
                - task: AzureCLI@2
                  displayName: 'Desplegar Imagen Docker de Front en ACI prod'
                  inputs:
                    azureSubscription: '$(ConnectedServiceName)'
                    scriptType: bash
                    scriptLocation: inlineScript
                    inlineScript: |
                      echo "Resource Group: $(ResourceGroupName)"
                      echo "Container Instance Name: $(frontContainerInstanceNameprod)"
                      echo "ACR Login Server: $(acrLoginServer)"
                      echo "Image Name: $(frontImageName)"
                      echo "Image Tag: $(frontprodImageTag)"
                  
                      az container delete --resource-group $(ResourceGroupName) --name $(frontContainerInstanceNameprod) --yes

                      az container create --resource-group $(ResourceGroupName) \
                        --name $(frontContainerInstanceNameprod) \
                        --image $(acrLoginServer)/$(frontImageName):$(frontprodImageTag) \
                        --registry-login-server $(acrLoginServer) \
                        --registry-username $(acrName) \
                        --registry-password $(az acr credential show --name $(acrName) --query "passwords[0].value" -o tsv) \
                        --dns-name-label $(frontContainerInstanceNameprod) \
                        --ports 80 \
                        --environment-variables API_URL="$(API_URL_PROD)" \
                        --restart-policy Always \
                        --cpu $(container-cpu-front-prod) \
                        --memory $(container-memory-front-prod)

    #---------------------------------------
    ### STAGE DEPLOY TO AZURE APP SERVICE QA
    #---------------------------------------
  - stage: DeployImagesToAppServiceQA
    displayName: 'Deploy Docker Images to Azure App Service (QA)'
    dependsOn: 
    - BuildAndTestBackAndFront
    - DockerBuildAndPush
    condition: succeeded()
    jobs:
    # -------------------------------------------------------------------------------
    # |               DEPLOY API TO AZURE APP SERVICE                               |
    # -------------------------------------------------------------------------------
        - job: DeployAPIImageToAppServiceQA
          displayName: 'Deploy API to Azure App Service (QA)'
          pool:
            vmImage: 'ubuntu-latest'
          steps:
            - task: AzureCLI@2
              displayName: 'Create API Azure App Service resource and configure image'
              inputs:
                azureSubscription: '$(ConnectedServiceName)'
                scriptType: 'bash'
                scriptLocation: 'inlineScript'
                inlineScript: |
                  # Check if API App Service already exists
                  if ! az webapp list --query "[?name=='$(AppServicesApiNameContainersQA)' && resourceGroup=='$(ResourceGroupName)'] | length(@)" -o tsv | grep -q '^1$'; then
                    echo "API App Service doesn't exist. Creating new..."
                    # Create App Service without specifying container image
                    az webapp create --resource-group $(ResourceGroupName) --plan $(AppServicePlanLinux) --name $(AppServicesApiNameContainersQA) --deployment-container-image-name "nginx"  # Especifica una imagen temporal para permitir la creación
                  else
                    echo "API App Service already exists. Updating image..."
                  fi

                  # Configure App Service to use Azure Container Registry (ACR)
                  az webapp config container set --name $(AppServicesApiNameContainersQA) --resource-group $(ResourceGroupName) \
                    --container-image-name $(acrLoginServer)/$(backImageName):$(backImageTag) \
                    --container-registry-url https://$(acrLoginServer) \
                    --container-registry-user $(acrName) \
                    --container-registry-password $(az acr credential show --name $(acrName) --query "passwords[0].value" -o tsv)
                  # Set environment variables
                  az webapp config appsettings set --name $(AppServicesApiNameContainersQA) --resource-group $(ResourceGroupName) \
                    --settings DBCONNECTIONSTRING="$(cnn_string_qa)" \
          # -------------------------------------------------------------------------------
    # |               DEPLOY FRONTEND TO AZURE APP SERVICE                           |
    # -------------------------------------------------------------------------------
        - job: DeployFrontImageToAppServiceQA
          displayName: 'Deploy Front to Azure App Service (QA)'
          pool:
            vmImage: 'ubuntu-latest'
          steps:
            - task: AzureCLI@2
              displayName: 'Create Front Azure App Service resource and configure image'
              inputs:
                azureSubscription: '$(ConnectedServiceName)'
                scriptType: 'bash'
                scriptLocation: 'inlineScript'
                inlineScript: |
                  # Check if Front App Service already exists
                  if ! az webapp list --query "[?name=='$(AppServicesFrontNameContainersQA)' && resourceGroup=='$(ResourceGroupName)'] | length(@)" -o tsv | grep -q '^1$'; then
                    echo "Front App Service doesn't exist. Creating new..."
                    # Create App Service without specifying container image
                    az webapp create --resource-group $(ResourceGroupName) --plan $(AppServicePlanLinux) --name $(AppServicesFrontNameContainersQA) --deployment-container-image-name "nginx"  # Especifica una imagen temporal para permitir la creación
                  else
                    echo "Front App Service already exists. Updating image..."
                  fi

                  # Configure App Service to use Azure Container Registry (ACR)
                  az webapp config container set --name $(AppServicesFrontNameContainersQA) --resource-group $(ResourceGroupName) \
                    --container-image-name $(acrLoginServer)/$(frontImageName):$(frontImageTag) \
                    --container-registry-url https://$(acrLoginServer) \
                    --container-registry-user $(acrName) \
                    --container-registry-password $(az acr credential show --name $(acrName) --query "passwords[0].value" -o tsv)
                  # Set environment variables
                  az webapp config appsettings set --name $(AppServicesFrontNameContainersQA) --resource-group $(ResourceGroupName) \
                    --settings API_URL="$(API_WEB_URL)" \

    # -------------------------------------------------------------------------------
    # |           RUN INTEGRATION TESTS ON AZURE APP SERVICES                       |
    # -------------------------------------------------------------------------------
        - job: RunCypressTests
          displayName: 'Run Cypress Tests on App Services'
          dependsOn: [DeployFrontImageToAppServiceQA, DeployAPIImageToAppServiceQA]
          variables:
              baseUrl: 'http://jr-front-web.azurewebsites.net'
          condition: succeeded()
          steps:
            - script: npm install ts-node typescript --save-dev
              displayName: 'Install typescript'
              workingDirectory: ./EmployeeCrudAngular

            - script: npx cypress run --config-file cypress.config.ts  --env baseUrl=$(baseUrl)
              workingDirectory: ./EmployeeCrudAngular
              displayName: 'Run integration tests'
              continueOnError: true
            - task: PublishTestResults@2
              inputs:
                testResultsFormat: 'JUnit'
                testResultsFiles: '*.xml'
                searchFolder: '$(frontPath)/cypress/results'
                testRunTitle: 'Cypress Integration Tests'
    

    # -------------------------------------------------------------------------------
    # |     STAGE DEPLOY FRONTEND AND BACKEND TO AZURE APP SERVICES PROD             |
    # -------------------------------------------------------------------------------
  - stage: DeployImagesToAppServiceProd
    displayName: 'Deploy Docker Images to Azure App Service (Prod)'
    dependsOn: DeployImagesToAppServiceQA
    condition: succeeded()
    jobs:
    # -------------------------------------------------------------------------------
    # |               DEPLOY API TO AZURE APP SERVICE                               |
    # -------------------------------------------------------------------------------
        - deployment: DeployAPIImageToAppServiceProd
          displayName: 'Deploy API to Azure App Service (Prod)'
          pool:
            vmImage: 'ubuntu-latest'
          environment: 'PROD'
          strategy:
            runOnce:
              deploy:
                steps:
                  - task: AzureCLI@2
                    displayName: 'Create API Azure App Service resource and configure image'
                    inputs:
                      azureSubscription: '$(ConnectedServiceName)'
                      scriptType: 'bash'
                      scriptLocation: 'inlineScript'
                      inlineScript: |
                        # Check if API App Service already exists
                        if ! az webapp list --query "[?name=='$(AppServicesApiNameContainersProd)' && resourceGroup=='$(ResourceGroupName)'] | length(@)" -o tsv | grep -q '^1$'; then
                          echo "API App Service doesn't exist. Creating new..."
                          # Create App Service without specifying container image
                          az webapp create --resource-group $(ResourceGroupName) --plan $(AppServicePlanLinux) --name $(AppServicesApiNameContainersProd) --deployment-container-image-name "nginx"  # Especifica una imagen temporal para permitir la creación
                        else
                          echo "API App Service already exists. Updating image..."
                        fi

                        # Configure App Service to use Azure Container Registry (ACR)
                        az webapp config container set --name $(AppServicesApiNameContainersProd) --resource-group $(ResourceGroupName) \
                          --container-image-name $(acrLoginServer)/$(backImageName):$(backImageTag) \
                          --container-registry-url https://$(acrLoginServer) \
                          --container-registry-user $(acrName) \
                          --container-registry-password $(az acr credential show --name $(acrName) --query "passwords[0].value" -o tsv)
                        # Set environment variables
                        az webapp config appsettings set --name $(AppServicesApiNameContainersProd) --resource-group $(ResourceGroupName) \
                          --settings DBCONNECTIONSTRING="$(cnn_string_qa)" \

    # -------------------------------------------------------------------------------
    # |               DEPLOY FRONTEND TO AZURE APP SERVICE                           |
    # -------------------------------------------------------------------------------
        - deployment: DeployFrontImageToAppServiceProd
          displayName: 'Deploy Front to Azure App Service (Prod)'
          pool:
            vmImage: 'ubuntu-latest'
          environment: 'PROD'
          strategy:
            runOnce:
              deploy:
                steps:
                  - task: AzureCLI@2
                    displayName: 'Create Front Azure App Service resource and configure image'
                    inputs:
                      azureSubscription: '$(ConnectedServiceName)'
                      scriptType: 'bash'
                      scriptLocation: 'inlineScript'
                      inlineScript: |
                        # Check if Front App Service already exists
                        if ! az webapp list --query "[?name=='$(AppServicesFrontNameContainersProd)' && resourceGroup=='$(ResourceGroupName)'] | length(@)" -o tsv | grep -q '^1$'; then
                          echo "Front App Service doesn't exist. Creating new..."
                          # Create App Service without specifying container image
                          az webapp create --resource-group $(ResourceGroupName) --plan $(AppServicePlanLinux) --name $(AppServicesFrontNameContainersProd) --deployment-container-image-name "nginx"  # Especifica una imagen temporal para permitir la creación
                        else
                          echo "Front App Service already exists. Updating image..."
                        fi

                        # Configure App Service to use Azure Container Registry (ACR)
                        az webapp config container set --name $(AppServicesFrontNameContainersProd) --resource-group $(ResourceGroupName) \
                          --container-image-name $(acrLoginServer)/$(frontImageName):$(frontImageTag) \
                          --container-registry-url https://$(acrLoginServer) \
                          --container-registry-user $(acrName) \
                          --container-registry-password $(az acr credential show --name $(acrName) --query "passwords[0].value" -o tsv)
                        # Set environment variables
                        az webapp config appsettings set --name $(AppServicesFrontNameContainersProd) --resource-group $(ResourceGroupName) \
                          --settings API_URL="$(API_WEB_URL_PROD)" \
	
  	  ```
### 6-  Presentación del trabajo práctico.
- Subir un doc al repo de GitHub con las capturas de pantalla de los pasos realizados. Debe ser un documento (md, word, o pdf), no videos. Y el documento debe seguir los pasos indicados en el Desarrollo del TP.
- Acceso al repo de Azure Devops para revisar el trabajo realizado.

### 7-  Criterio de Calificación
El paso 4.1 representa un 20% de la nota total, el paso 4.2 representa el 80% restante.




