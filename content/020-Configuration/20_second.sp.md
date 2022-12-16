---
title: "Análisis de las respuestas de la API"
chapter: false
weight: 20
---

## Comprender las rutas JSON
Las respuestas JSON son series de Objetos y Arrays en una estructura jerárquica. Se empieza con un objeto base, en el ejemplo de abajo, el objeto base será "Thestore". Nuestro ejemplo "Thestore" vende una serie de diferentes alimentos y cubiertos. Si ejecutamos una consulta sobre el inventario de "Thestore" recibiremos una respuesta con todos los objetos que vende "Thestore" y los atributos asociados a estos objetos, como su categoría (frutas, verduras, etc.) y su precio.

![image](/images/storeexample.PNG)

¿Y si sólo nos interesan las frutas que vende "Thestore"? ¿Y si tenemos un presupuesto limitado y sólo queremos ver lo que la tienda tiene por menos de 2 dólares? Definir una ruta JSON contra la respuesta nos permitirá aislar sólo los artículos que nos interesan.

Un ejemplo de ruta JSON para encontrar todos los alimentos que ofrece "Thestore" sería el siguiente - 
```
$.Thestore.food[*]
```
**'$'** - Dice que comience en el objeto raíz de "Thestore".

**'.food'** - Indica que hay que pasar al siguiente objeto o matriz ("comida") dentro de "Thestore".

**'[*]'** - es una notación de posición de array, que indica que se devuelvan todos los objetos dentro del array de comida, esto podría cambiarse a **'[0]'** para devolver sólo el primer objeto dentro de este array.
>**Nota: en la mayoría de los idiomas, las matrices se indexan o comienzan en 0, siendo 0 el primer elemento.**

Un ejemplo de una ruta (path) que sólo devuelve objetos que cuestan menos de 2,50 dólares - 
```
$.Thestore.food[?(@.price<2.50)]
```
La única diferencia aquí es que ya no estamos definiendo una posición de matriz y ahora hemos aplicado un filtro **' [?(@.price<2.50)] '** que mira la clave de precio y filtra sólo los objetos que son inferiores a 2,50 dólares.

## Análisis del cuerpo de la respuesta

Ahora que ha encontrado una llamada a la API que devuelve los datos que necesitamos, necesitamos determinar la ruta que devolverá sólo la información de presencia que estamos buscando.
>**Como hemos visto en la respuesta del centro de desarrollo, al igual que en nuestro ejemplo anterior de consulta al almacén completo, esta llamada específica devolvía muchas líneas de datos que son irrelevantes para nuestro objetivo de comprobar la presencia dentro de la cola. Dependiendo de la llamada específica, o para esta llamada - cuántos miembros están en la cola, puede devolver miles de líneas de datos.**

Existen muchas herramientas externas que pueden ayudarle a analizar las respuestas JSON en busca de los datos que necesita. Aunque Genesys no tiene herramientas preferidas para realizar estas acciones, a continuación se muestran algunas herramientas de acceso público que se utilizarán para realizar estas acciones en este taller. 
>**Estas herramientas no son necesarias, pero pueden ayudar mientras se aprende el análisis sintáctico de JSON.**

  * https://goessner.net/articles/JsonPath/index.html#e2 - Puede proporcionar definiciones de funciones JSON y ejemplos de análisis sintáctico.

  * https://jsonpathfinder.com/ - Puede ayudar a encontrar rutas a objetos específicos dentro de las respuestas JSON

  * http://jsonpath.com/ - Puede proporcionar representaciones visuales de los datos que se devolverán a partir de rutas específicas.

Al pegar la respuesta de nuestro developer center en herramientas como path finder, puede ver un desglose/jerarquía visual de los objetos y matrices dentro de la respuesta JSON.

En el ejemplo siguiente, podemos ver que la herramienta Path Finder ha identificado 2 entidades dentro de nuestra respuesta JSON, que se correlacionan con los 2 usuarios que tenemos en la cola.

![image](/images/pathfinder1.PNG)

Desplegando los campos debajo de una de las entidades, podemos encontrar el campo de presencia. Al seleccionar el campo de presencia, vemos que la herramienta ha rellenado una "Ruta" (Path) a este campo para nosotros. 

![image](/images/pathfinder2.PNG)

Ahora que tenemos nuestra ruta, es importante visualizar los datos y filtrar o ampliar nuestra ruta cuando sea necesario. Si cambiamos al evaluador en línea de JSONPath, pegamos la respuesta completa del centro de desarrollo en el campo Entrada y pegamos la ruta que descubrimos en JSONPathfinder en el campo Ruta (path), podremos visualizar el resultado de la evaluación de la ruta real.
>**tendrá que sustituir la "X" que JSONPathFinder coloca al principio de la ruta por un "$"**

![image](/images/Jsonpath1.PNG)

Sin embargo, sabemos que hay varios usuarios en esta cola y necesitamos la información de presencia de todos ellos. La ruta que estamos utilizando tiene una posición de matriz de **"[0]"**, lo que significa que sólo devuelve la presencia de la primera entidad, o usuario dentro de la respuesta. Cambiando esto por un comodín - **"[*]"**, podemos devolver la información de presencia de todos los usuarios.

![image](/images/Jsonpath2.PNG)