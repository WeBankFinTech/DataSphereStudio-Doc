# Sistema de programación de acceso DSS

## fondo

Hay muchos sistemas de programación de cronometraje por lotes utilizados actualmente en el campo del big data, como Azkaban, Dophinscheduler, Airflow, etc. DataSphere Studio (DSS) admite la publicación de flujos de trabajo diseñados por los usuarios en diferentes sistemas de programación para la programación. Actualmente, el soporte predeterminado para publicar en Azkaban. El usuario ha completado el diseño DAG del flujo de trabajo en DSS, incluida la carga de archivos de recursos de flujo de trabajo, la configuración de parámetros de flujo de trabajo, la importación de datos, la comprobación de calidad de datos, la escritura de código de nodo, el diseño de informes visuales, la salida de correo electrónico, la carga de archivos de recursos de nodo y la configuración de parámetros en ejecución Después de la operación, se puede depurar y ejecutar en DSS. Después de verificar que la ejecución de todos los nodos es correcta, se liberará al sistema de programación, y el sistema de programación programará y ejecutará regularmente de acuerdo con la configuración de la tarea programada.

### método de lanzamiento

AppConn debe acceder al nuevo sistema de programación de integración DSS. Los usuarios deben definir el XXXSchedulerAppConn correspondiente de acuerdo con los diferentes sistemas de programación, y la especificación de integración de transformación y la especificación de integración estructurada se definen en SchedulerAppConn. La especificación de integración de transformación contiene la transformación de contenido de nivel de proyecto de DSS y contenido de nivel de flujo de trabajo de DSS a sistemas de programación de terceros. El acceso de DSS al sistema de despacho se puede dividir en los dos tipos siguientes:

1.  Versión a nivel de ingeniería

Se refiere a convertir todos los flujos de trabajo en el proyecto y empaquetar y cargar uniformemente el contenido convertido en el sistema de programación. Existen principalmente la interfaz ProjectPreConversionRel, que define el flujo de trabajo que debe convertirse en el proyecto.

2.  Versión a nivel de flujo de trabajo

Se refiere a la conversión de acuerdo con la granularidad del flujo de trabajo, y solo el contenido del flujo de trabajo se empaqueta y se carga en el sistema de programación. La definición actual del flujo de trabajo de DSS se almacena en el archivo BML en forma de Json y la información de metadatos del flujo de trabajo se almacena en la base de datos.

## Los pasos principales

### Analizador

JsonToFlowParser se utiliza para convertir el Json del flujo de trabajo en flujo de trabajo. El flujo de trabajo es el formato estándar para operar el flujo de trabajo en DSS. Incluye la información del nodo del flujo de trabajo, la información de borde del flujo de trabajo, el flujo de trabajo principal, el flujo de trabajo secundario, el archivo de recursos de flujo de trabajo, el archivo de propiedades de flujo de trabajo, el tiempo de creación del flujo de trabajo, el tiempo de actualización, el usuario del flujo de trabajo, el usuario proxy del flujo de trabajo, la información de metadatos del flujo de trabajo como el nombre, el ID, la descripción, el tipo, si se trata de un flujo de trabajo raíz, etc. Estos se analizan de acuerdo con el contenido de Json y se convierten en objetos de flujo de trabajo operables DSS, como AzkabanWorkflow y DolphinSchedulerWorkflow.

### Convertidor

Convierta el flujo de trabajo de DSS en un flujo de trabajo que pueda ser reconocido por el sistema de programación de acceso. Cada sistema de programación tiene su propia definición para el flujo de trabajo. Si convierte los nodos del flujo de trabajo de DSS en los archivos de formato de trabajo de Azkaban o en las tareas de DolphinScheduler, también puede convertirlos a la inversa y convertir el flujo de trabajo del sistema de programación en un flujo de trabajo que DSS pueda cargar y mostrar, y las dependencias del flujo de trabajo. , la conexión del nodo se convierte en la dependencia del sistema de programación correspondiente. También puede comprobar si los nodos de flujo de trabajo del proyecto tienen nodos con el mismo nombre en Convertidor. Por ejemplo, el uso de nodos con el mismo nombre no está permitido en el sistema de programación de Azkaban.

WorkflowConVerter define la estructura de directorios de salida de conversión de flujo de trabajo, incluido el directorio de almacenamiento de flujo de trabajo, el almacenamiento de archivos de recursos de flujo de trabajo y el establecimiento de un directorio de almacenamiento de subflujo de trabajo. Por ejemplo, Azkaban también incluye la creación de un directorio de conversión de proyecto en la operación de conversión de nivel de proyecto y el establecimiento de un directorio de conversión de flujo de trabajo de acuerdo con la situación de flujo de trabajo en el proyecto. Convertir flujo de trabajo a dolphinSchedulerWorkflow o ScheduleWorkFlow en convertToRel

NodeConverter define el contenido de salida de la conversión de nodos: como ConvertNode de Azkaban, convertirá el contenido del nodo del flujo de trabajo en el contenido del archivo de trabajo correspondiente. Incluyendo el nombre, el tipo, la dependencia del nodo de conversión, el comando de ejecución del nodo (dependiendo del análisis linkis-jobtype), los parámetros de configuración del nodo, la etiqueta del nodo, etc. Finalmente, se almacena en el formato definido por el archivo de trabajo. El convertidor de DolphinScheduler convierte los nodos en DSS en tareas en DolphinScheduler, y crea el script de ejecución de Shell type Task, y convierte el contenido de nodo de DSS en los parámetros necesarios para la ejecución del script de dss-dolphinscheduler-client.sh personalizado.

```--java
            addLine.accept("LINKIS_TYPE", dssNode.getNodeType());   //Workflow Node Type
            addLine.accept("PROXY_USER", dssNode.getUserProxy());   //proxy user
            addObjectLine.accept("JOB_COMMAND", dssNode.getJobContent()); //Excuting an order
            addObjectLine.accept("JOB_PARAMS", dssNode.getParams());      //Node execution parameters
            addObjectLine.accept("JOB_RESOURCES", dssNode.getResources()); //Node execution resource file
            addObjectLine.accept("JOB_SOURCE", sourceMap);                 //Node's source information
            addLine.accept("CONTEXT_ID", workflow.getContextID());         //context ID
            addLine.accept("LINKIS_GATEWAY_URL", Configuration.getGateWayURL());  //linkis gateway address
            addLine.accept("RUN_DATE", "${system.biz.date}");                    //run date variable
```

### Tunning

Se utiliza para completar la operación de ajuste general antes de que se publique el proyecto. En la implementación de Azkaban, la configuración de ruta del proyecto y la configuración de ruta de almacenamiento del flujo de trabajo se completan principalmente. Debido a que en este momento es posible operar el proyecto = 》flujo de trabajo = 》 sub-flujo de trabajo, es conveniente para configurar la operación de afuera hacia adentro. Por ejemplo, el almacenamiento del flujo de trabajo depende de la ubicación de almacenamiento del proyecto y el almacenamiento del flujo de trabajo secundario depende de la ubicación del flujo de trabajo primario. El cálculo del nodo secundario se completa en FlowTuning y el nodo final se agrega automáticamente.

## Programación de la implementación de AppConn

### AbstractSchedulerAppConn

La clase abstracta de programación AppConn, el nuevo sistema de programación appConn access puede heredar directamente esta clase abstracta, implementa la interfaz SchedulerAppConn y hereda AbstractOnlySSOAppConn, para pasar por el inicio de sesión SSO entre DSS y el sistema de programación. Por ejemplo, dolphins y SchedulisAppConn, ya integrados, heredan esta clase abstracta.

Esta clase abstracta contiene dos tipos de Estándar

El primero es ConversionIntegrationStandard para admitir flujos de trabajo que transforman la orquestación DSS en un sistema de programación

El segundo es SchedulerStructureIntegrationStandard, una especificación de integración estructurada para DSS y sistemas de programación.

### ConversionIntegrationStandard

Especificación de integración de conversión para sistemas de programación, incluido DSSToRelConversionService para convertir la orquestación de DSS en flujos de trabajo del sistema de programación. También se reserva una interfaz para admitir la conversión del flujo de trabajo del sistema de programación en la orquestación de DSS

### AbstractSchedulerStructureIntegrationStandard

La especificación de integración de la estructura organizativa del sistema de programación se utiliza especialmente para la gestión de la estructura organizativa del sistema de programación, incluidos principalmente los servicios de ingeniería y los servicios de orquestación.

### ProjectService

*   Se realizan las operaciones unificadas de creación, actualización, eliminación y comprobación duplicada del proyecto.
*   Se utiliza para abrir el sistema de ingeniería de la ingeniería DSS y acceder a herramientas de aplicaciones de terceros, y realizar la gestión colaborativa de la ingeniería.
*   Si el sistema de programación necesita abrir el sistema de ingeniería con DSS, debe implementar todas las interfaces de los servicios de ingeniería en la especificación de integración estructurada.

### OrchestrationService

El servicio de orquestación se utiliza para la especificación de orquestación unificada del sistema de programación y tiene las siguientes funciones:

*   Especificación de orquestación unificada, especialmente utilizada para abrir el sistema de orquestación de DSS y SchedulerAppConn (sistema de programación).
*   Por ejemplo: conecte el flujo de trabajo de DSS y el flujo de trabajo de programaciones.
*   Tenga en cuenta que si el sistema SchedulerAppConn de acoplamiento no admite el flujo de trabajo de administración en sí, no es necesario implementar esta interfaz.
