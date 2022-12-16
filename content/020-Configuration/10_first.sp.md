---
title: "Developer Center"
chapter: false
weight: 10
---

> **El centro para desarrolladores de Genesys Cloud (Developer Center) contiene numerosos recursos para desarrolladores, como ejemplos de código, planos, SDK y, como veremos en breve, un sólido explorador de API.**





## Identifique y ejecute su llamada a la API

En esta sección vamos a encontrar y ejecutar una llamada para encontrar la presencia de todos los usuarios dentro de una cola dada.

Una vez que haya navegado e iniciado sesión en el **[Developer Center](https://developer.genesys.cloud/)**, seleccione el mosaico Explorador de API que se muestra a continuación.

![image](/images/devcenter.PNG)

El Explorador de API le permite filtrar y buscar en todos los puntos finales de API accesibles la llamada que está buscando y ejecutarla sin necesidad de una acción de datos totalmente configurada.

Empezaremos filtrando por las API que creamos que pueden contener la información que buscamos.
>**Es posible que tenga que trabajar con las llamadas a la API que considere relevantes y ejecutarlas para determinar qué punto final proporciona la información que necesita. También es posible que haya numerosas llamadas que proporcionen el dato que busca. En este taller navegaremos directamente a la llamada que necesitamos.**

1. En la parte derecha, borre todas las categorías y, a continuación, vuelva a añadir la categoría "Enrutamiento" (Routing) al filtro.
2. Busque "Cola" (Queue) en el cuadro de búsqueda.
3. Marque la casilla "GET" para filtrar sólo las solicitudes GET.

![image](/images/explorerfilter.PNG)

Una vez que hemos encontrado nuestra llamada, en este caso **/api/v2/routing/queues/{queueId}/members** necesitamos configurarla antes de su ejecución.

1. Desactivar el modo lectura, esto nos permitirá ejecutar directamente desde el centro de desarrollo
2. Introduzca su queueId
>**La mayoría de los componentes de configuración dentro de Genesys Cloud tienen asignado un GUID, o Identificador Único Global. Navegando a la cola desde su panel de administración puede encontrar el GUID dentro de la URL.**

![image](/images/queueguid.PNG)

3. Desplegar la llamada para mostrar la información de Presencia
>**Algunas llamadas ocultan información adicional por defecto, pero pueden ampliarse o filtrarse para añadir o eliminar información.**

![image](/images/explorerconfig.PNG)

Ahora que nuestra llamada ha sido configurada, ejecutaremos la llamada y verificaremos que la información que estamos buscando, está contenida en el cuerpo de la respuesta.
>**Sabemos que hemos encontrado la llamada correcta porque podemos ver la presencia del usuario en el cuerpo de la respuesta.**

![image](/images/explorerexecute.PNG)

