## Configuración de pipeline de CI/CD en Azure DevOps
El pipeline se divide en 4 etapas: 
## 1. Etapa de Construcción y ejecución de pruebas unitarias.
En esta etapa se utilizan las tareas de JavaToolInstaller@ y Maven@4 del Azdure DevOps que permiten realizar la instalación de las dependencias necesarias para la construcción de la aplicación, en este caso es un "HelloWorld" en Maven. Adicionalmente, esta tarea permite ejecutar las pruebas unitarias. Despues se utiliza la tarea de PublishPipelineArtifact@1 para realizar la publicación del resultado de las pruebas unitarias en un artefacto en el workspace del pipeline en formato HTML.

## 2. Etapa de análisis con SonarQube
En esta etapa se utilizó SonarQube, ya que es de uso libre y es de fácil implementación. El servidor de SonarQube se desplegó en una Aplicación Web de Azure llamado SonarQubeAppServicePruebaTecnica. Se instaló la extensión de SonarQube para la organización del Azure DevOps desde el marketplace. Se realizó un service connection en el proyecto de Azure para realizar la autenticación del servidor de SonarQube con la organización del Azure DevOps. Con las tareas de SonarQubePrepare@6, SonarQubeAnalyze@6 y SonarQubePublish@6 en el pipeline se realiza el análisis, en estas tareas se especifica el proyecto y su respectiva key. Por ultimo se realiza la publicación del resultado del análisis en un artefacto en el workspace del pipeline.

## 3. Etapa de análisis de dependencias con Dependency Chek
En esta etapa se utilizó Dependency Check ya que es de uso libre y permite el análisis de las dependencias utilizadas en la aplicación para identificar las vulnerabildades de las mismas. Se instaló la extensión de Dependency Check para la organización del Azure DevOps desde el marketplace. Se utilizaron las tareas de Maven@4, donde se especificó el uso del plugin establecido en el pom.xml de la aplicación, tambien se utilizó la tarea PublishTestResults@2 para publicar los resultados en el workspace del pipeline y adicionalmente se usó la tarea de PublishPipelineArtifact@1 para realizar la publicación del resultado del analisis de dependencias en un artefacto en el workspace del pipeline en formato HTML.

## 4. Etapa de despliegue en entorno de prueba
En esta etapa se utilizó un App service llamado EntornoPrueba con SO Linux en Azure para realizar el despliegue de la aplicación,
 
