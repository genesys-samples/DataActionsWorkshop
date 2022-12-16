---
title: "Construcción de la Acción de Datos"
chapter: false
weight: 40
---


Llegados a este punto, ya ha identificado su llamada a la API y la ha analizado para obtener los datos que intenta recuperar. Ahora tenemos que unirlo todo en una acción de datos que pueda utilizarse en flujos o scripts de Architect.

Para empezar, vaya a Admin > Acciones y cree una nueva acción de datos en el tipo de integración que ha creado.

![image](/images/createaction.PNG)

## Contratos
El primer paso es construir nuestro contrato de entrada, que como se explica en el Developer Center, sólo requiere el Queue ID como entrada. Al nombrar su primer objeto puede añadir su cadena de entrada de QueueID debajo de este seleccionando el "+" y asegurándose de que el campo se establece en el tipo cadena.

![image](/images/DAinputcontract.PNG)

A continuación, definirá su contrato de salida, mientras que hay varias maneras de construir contratos de salida para permitir que los datos adicionales, se construirá un objeto que es una matriz de cadenas.

![image](/images/DAoutputcontract.PNG)

## Configuración
Dentro de la pestaña de configuración comenzaremos definiendo nuestra Plantilla de URL de solicitud utilizando la URL de API que hemos obtenido del centro de desarrolladores, en este caso tendrá el siguiente aspecto -
```
/api/v2/routing/queues/INSERTQUEUEGUIDHERE/members?expand=presence
```
Necesitaremos usar nuestras entradas disponibles para reemplazar el GUID estático de la cola con la variable de entrada que creamos de QueueID. Puede hacerlo resaltando el GUID de la cola y haciendo clic en la entrada "QueueID (String)" en la parte derecha de la pantalla.

![image](/images/DAconfiginput.PNG)

El campo de respuesta es donde construiremos nuestro mapa de traducción para analizar la respuesta, los valores por defecto del mapa de traducción para manejar con gracia las respuestas vacías, y las plantillas de éxito para asignar nuestra traducción a nuestra variable de salida. A continuación se muestra un ejemplo de código de una configuración de respuesta.

```
{
  "translationMap": {
    "Presence": "$.entities[*].user.presence.presenceDefinition.systemPresence"
  },
  "translationMapDefaults": {
    "Presence": "null"
  },
  "successTemplate": "{\"PresenceArray\" : $Presence}"
}
```

Dentro de la sección del mapa de traducción tenemos un Par de Valores Clave de Presencia, y la ruta al array de presencias de usuarios que descubrimos anteriormente. 

La plantilla Success está configurada para mapear el valor del Par de Valores Clave 'Presencia', al array de salida que construimos en el área de contrato.

Ahora podemos probar esta configuración para asegurarnos de que la acción de datos funciona.

## Pruebas

Introduciendo manualmente un QueueID podemos realizar una prueba con nuestra configuración para asegurarnos de que los datos se devuelven según lo esperado.
En la imagen de abajo podemos ver una ejecución exitosa con nuestros valores de presencia correctamente mapeados a nuestro array de salida.

![image](/images/DAtest.PNG)

Si hubiéramos cometido un error de tipografía, o un error en la configuración, la prueba nos devolvería el error y el paso en el proceso de ejecución para ayudarnos en la resolución de problemas. En los ejemplos se ha encontrado un error de tipografía en el valor del par clave-valor de presencia, creando un fallo en el paso 10 del proceso REST.

![image](/images/DAtestfailure1.PNG)

Navegando hasta el paso 10 podemos ver un desglose del error real
```
{
  "message": "Transform failed to process result using 'successTemplate' template due to error:'Variable $Presences has not been set at successTemplate[line 1, column 20]'\n Template:'{\"PresenceArray\" : $Presences}'.",
  "code": "bad.request",
  "status": 400,
  "messageParams": {},
  "contextId": "fc7c43df-fb93-4651-83b2-2dc942e623d7",
  "details": [
    {
      "errorCode": "ACTION.PROCESSING"
    }
  ],
  "errors": []
}
```
Este ejemplo indica que "$Presences no se ha establecido en la plantilla de éxito" -  "$Presences has not been set at the successtemplate", lo que nos ayuda a identificar que tenemos un desajuste entre el mapa de traducción y la plantilla de éxito. En el siguiente ejemplo, la línea 8 tiene presencias en plural, mientras que la línea 3 tiene presencias en singular.

![image](/images/DAtestfailure2.PNG)

Cuando las pruebas se hayan superado y funcionen según lo esperado, podrá publicar su acción de datos para utilizarla en flujos o secuencias de comandos (scripts).