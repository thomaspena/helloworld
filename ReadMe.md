## **Configuración de pipeline de CI/CD en Azure DevOps**
El pipeline se divide en 4 etapas: 
## **1. Etapa de Construcción y ejecución de pruebas unitarias**
En esta etapa se utilizan las tareas de `JavaToolInstaller@` y `Maven@4` del Azdure DevOps que permiten realizar la instalación de las dependencias necesarias para la construcción de la aplicación, en este caso es un "HelloWorld" en Maven. Adicionalmente, esta tarea permite ejecutar las pruebas unitarias. Despues se utiliza la tarea de PublishPipelineArtifact@1 para realizar la publicación del resultado de las pruebas unitarias en un artefacto en el workspace del pipeline en formato HTML. (En el repositorio se encuentra el archivo de las pruebas en la carpeta de Archivos Adjuntos)

## **2. Etapa de análisis con SonarQube**
En esta etapa se utilizó SonarQube, ya que es de uso libre, de fácil implementación y tengo experianecia en su manejo. El servidor de SonarQube se desplegó en una App service de Azure llamado **SonarQubeAppServicePruebaTecnica**, ya que **Azure App Service** es una oferta de plataforma como servicio (PaaS) y se puede configurar facilmente la integración y la implementación continuas con Azure DevOps, y no debemos preocuparnos por el SO, también incluye plantillas para una facil implementación. Se instaló la extensión de SonarQube para la organización del Azure DevOps desde el marketplace. Se realizó un service connection en el proyecto de Azure para realizar la autenticación del servidor de SonarQube con la organización del Azure DevOps. Con las tareas de `SonarQubePrepare@6`, `SonarQubeAnalyze@6` y `SonarQubePublish@6` en el pipeline se realiza el análisis, en estas tareas se especifica el proyecto y su respectiva key. Por ultimo se realiza la publicación del resultado del análisis en un artefacto en el workspace del pipeline.
### Resultados Análisis con SonarQube
![Analisis SonarQube](ArchivosAdjuntos/analisisSonarQube.png)
### Ambiente Servidor SonarQube
![Entorno SonarQube](ArchivosAdjuntos/SonarQube.png)
## **3. Etapa de análisis de dependencias con Dependency Chek OWASP**
En esta etapa se utilizó Dependency Check ya que es de uso libre y permite el análisis de las dependencias utilizadas en la aplicación para identificar las vulnerabildades de las mismas. Además al ser de la organización de OWASP, incluye las vulverabilidades del Top 10. Se instaló la extensión de Dependency Check para la organización del Azure DevOps desde el marketplace. Se utilizaron las tareas de `Maven@4`, donde se especificó el uso del plugin establecido en el pom.xml de la aplicación, tambien se utilizó la tarea `PublishTestResults@2` para publicar los resultados en el workspace del pipeline y adicionalmente se usó la tarea de PublishPipelineArtifact@1 para realizar la publicación del resultado del analisis de dependencias en un artefacto en el workspace del pipeline en formato HTML. (En el repositorio se encuentra el archivo de las pruebas en la carpeta de ArchivosAdjuntos)


## **4. Etapa de despliegue en entorno de prueba**
En esta etapa se utilizó un App service llamado EntornoPrueba con SO Linux en Azure para realizar el despliegue de la aplicación, ya que **Azure App Service** es una oferta de plataforma como servicio (PaaS) y se puede configurar facilmente la integración y la implementación continuas con Azure DevOps, y no debemos preocuparnos por el SO, se creo un service connection Azure Resource Manager para autenticarse con el app service en el portal de Azure y con las plantillas se puede hacer una rápida implementación y admite diferewntes lenguajes de programación como el usado en esta apliación. Se utilizó la tarea `ArchiveFiles@2` para empaquetar la aplicación en un archivo .zip, se utilizó la tarea `PublishPipelineArtifact@1` para publicar el archivo empaquetado en el workspace del pipeline en un artefacto y por último, se utilizó la tarea `AzureRmWebAppDeployment@4` para desplegar la aplicación empaquetada en el ambiente de prueba. En esta tarea se especificó el service connection y el nombre del entorno a desplegar.
### Ambiente portal Azure
 ![Entorno Prueba](ArchivosAdjuntos/EntornoPrueba.png)
## **5. Resultados Ejecución del pipeline**
Se muestra la correcta ejecución del pipeline, la generación de los artefactos y la publicación de las pruebas
![Entorno Prueba](ArchivosAdjuntos/Ejecuciónpipeline.png)
![Entorno Prueba](ArchivosAdjuntos/Publicacion.png)
![Entorno Prueba](ArchivosAdjuntos/publicacionsonar.png)
