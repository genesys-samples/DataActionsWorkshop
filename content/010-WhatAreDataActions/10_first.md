---
title: "Definiciones básicas"
chapter: false
weight: 10
---

## Definición de acciones de datos
En pocas palabras, las acciones de datos (Data Actions) son lo que Genesys define como nuestro constructor de llamadas API REST.

Las acciones de datos se utilizan para acceder a información tanto interna como externa a la plataforma que de otro modo no estaría disponible. Esta información, como la cola avanzada o los datos de CRM, se puede usar para invocar decisiones de enrutamiento poderosas o proporcionar información avanzada de secuencias de comandos/pantallas emergentes a sus agentes (entre muchas otras utilizaciones). Las acciones de datos se pueden dividir en 2 categorías, **Internas** y **Externas**.

Las acciones de datos nos permiten la flexibilidad de trabajar de manera intercambiable con otras aplicaciones y pasar datos libremente a donde deben estar. Estos pueden ser escenarios como llamar a atributos de datos personalizados para presentarlos en un script de agente, enviar datos a un almacén de CRM doméstico, exportar datos y automatizar procesos dentro del entorno Genesys CX.

**Las acciones de datos internos - Internal Data Actions** se utilizan "normalmente" para tomar decisiones de enrutamiento o actualizar los recursos y las configuraciones de la plataforma.
   * Ejemplo: verificar cuántos agentes están en cola para determinar si se debe ofrecer a la persona que llama una opción de devolución de llamada.

**Las acciones de datos externos - External Data Actions** se pueden usar para extraer información de sistemas externos para completar scripts o actualizar sistemas externos con información que el agente recopiló en la llamada.
  * Ejemplo: extraer un registro de cliente de un CRM externo para mostrar la información de la cuenta al agente dentro del script y actualizar ese registro de cliente desde el script.