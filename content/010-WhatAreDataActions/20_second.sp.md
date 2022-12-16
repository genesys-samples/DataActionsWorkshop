---
title: "Definición de componentes"
chapter: false
weight: 20
---

## Componentes de las acciones de datos - Data Actions

Las acciones de datos se pueden dividir en los siguientes 4 componentes

**1. Contratos de entrada - Input Contracts**

**2. Contratos de salida - Output Contracts**

**3. URLs de solicitud - Request URLs**

**4. Cuerpos de respuesta - Response Bodies**

### Contratos de entrada
Los contratos de entrada son variables o conjuntos de variables que proporcionamos en la llamada REST para decirle a la API qué dato específico estamos tratando de invocar.
   * Ejemplo: estamos tratando de recuperar datos de presencia de usuarios de una cola específica, construimos una variable de QueueID para ingresar en nuestra llamada API, lo que nos permite proporcionar dinámicamente un nuevo ID de cola en cada llamada.

> **No todas las llamadas API requieren un contrato de entrada definido, y las entradas se pueden asignar de forma estática.**

En la imagen a continuación, hemos creado un objeto con una cadena de entrada de QueueID, esta entrada luego se asigna en la URL de solicitud para permitir que se asigne un valor de ID de cola dinámico desde un flujo de architect. Alternativamente, si el valor de ID de cola no necesita cambiar, se puede asignar de forma estática.

![image](/images/inputcontracts.PNG)

### Contratos de salida
En una respuesta de API, podemos recibir cientos de líneas de datos, pero solo nos importa una sola línea.
Los Contratos de Salida son donde definimos los datos de la llamada sobre los que queremos actuar.
  * Ejemplo: Hemos ejecutado una llamada para que nos proporcione todos los detalles de la cola, sólo necesitamos ver la presencia del agente, por lo que establecemos una variable de Presencia para almacenar esta información al final de la llamada.

  >**No todas las Llamadas API requieren un Contrato de Salida definido; es posible que algunas llamadas en realidad no devuelvan datos a los que deseamos hacer referencia.**

En la imagen a continuación, hemos construido nuestro Contrato de salida y solo hemos construido Presencia como una variable de salida. En el cuerpo de la respuesta definido más adelante, podemos analizar la respuesta en busca de presencia y almacenar solo ese valor en nuestra única variable de salida.

![image](/images/outputcontracts.PNG)

### URLs de solicitud
Las URL de solicitud son la llamada real que se está ejecutando y contienen la ruta de API específica que estamos invocando. En nuestro constructor de acciones de datos, algunas llamadas se pueden definir en la vista simple donde definimos solo la URL y las entradas, mientras que otras llamadas pueden requerir predicados adicionales y formatearse en el campo JSON.

En la imagen a continuación, podemos ver las plantillas de solicitud **Simple** y **JSON**. También podemos ver la variable de entrada QueueID asignada en ambos formatos de solicitud.

![image](/images/requesturls.PNG)

### Cuerpos de respuesta

El cuerpo de la respuesta define qué información de la llamada estamos tratando de devolver y se puede dividir en 3 componentes. 
  * Los mapas de traducción (Translation Maps) nos permiten analizar y asignar datos de la respuesta de la API a variables.
  * Los valores predeterminados de los mapas de traducción (Translation Map Defaults) nos permiten definir valores predeterminados en caso de que no se devuelva información.
  * Las plantillas de éxito (Success Templates) nos permiten asignar los datos analizados a las variables de contrato de salida que hemos construido para que los datos puedan ser utilizados para el enrutamiento, secuencias de comandos, etc.

En la imagen de abajo tenemos -
1. Construido un mapa de traducción con un par clave-valor de 'Presencia' y la ruta utilizada para analizar la respuesta de la API sólo para la información de presencia.
2. Definido un mapa de traducción para aplicar un valor por defecto a 'Presence' de "NOT_SET" en el caso de que no devolvamos ningún dato. Esto permitirá que la solicitud falle con gracia si no se devuelve ninguna respuesta.
3. Asignamos el valor de 'Presence' de nuestro mapa de traducción a nuestra variable de salida *<ins>'Presences'<ins>*.

![image](/images/responsebodies.PNG)

### Herramienta de prueba de acción de datos 
El Constructor de acciones de datos proporciona una herramienta de prueba que permite probar las llamadas y validar que están configuradas correctamente antes de desplegarlas. La herramienta de prueba también contiene una secuencia de operaciones para mostrar en qué parte del proceso RESTful está fallando la llamada y facilitar así la resolución de problemas.

En la imagen de abajo podemos ver que la prueba no pudo aplicar la transformación de salida, lo que indica que el mapa de traducción debe ser el punto de partida para nuestra solución de problemas.

![image](/images/testtool.PNG)