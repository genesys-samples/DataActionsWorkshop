---
title: "Integración y OAuth"
chapter: false
weight: 30
---

## Integración y OAuth
Para empezar, veremos cómo crear y autenticar Data Actions (o Data Dips) en la interfaz de usuario de Genesys CX. Antes de crear la integración para la Acción de Datos, primero debemos tener algo para autenticarla. Aquí es donde creamos nuestro token de Cliente OAuth. Los clientes OAuth le permiten realizar solicitudes a la API de la plataforma o autenticarse contra Genesys Cloud, o sincronizar entidades entre Genesys Cloud y sistemas de terceros.

Hay 3 tipos principales de integración que encontramos cuando buscamos crear una integración: Platform API, Genesys CX Embeddable frameworks y Genesys SCIM.

 Hoy nos centraremos en la API de plataforma.

**API de la Platforma:**

Este procedimiento es para proveedores de aplicaciones que desean que su aplicación reciba un token que le permita realizar solicitudes a la API de Genesys Cloud Platform. El token representa el permiso de un usuario para que la aplicación acceda a los datos de Genesys Cloud. Se utiliza cuando la aplicación debe autorizar una solicitud a un punto final de la API. Para ver una lista de las API de Genesys Cloud Platform, consulte los recursos de API en el Centro de Desarrolladores de Genesys Cloud.

Vaya a Admin > Oauth y cree un nuevo cliente.

![image](/images/auth1.PNG)

Para este taller seleccionaremos **Client Credentials** debajo de Tipos de Concesión (Grant Types). 

Los tipos de concesión establecen la forma en que una aplicación obtiene un token de acceso. Genesys Cloud admite los tipos de concesión de autorización OAuth 2 que se enumeran a continuación. Al hacer clic en el nombre de un tipo de concesión se muestra más información sobre él en el Centro de desarrollo de Genesys Cloud (Genesys Cloud Developer Center). 
  * **Concesión de credenciales de cliente:** Un proceso de autenticación de un solo paso exclusivamente para uso de aplicaciones que no son de usuario (por ejemplo, un servicio de Windows o un trabajo cron). La aplicación cliente proporciona credenciales de cliente OAuth a cambio de un token de acceso. Este tipo de autorización no está en el contexto de un usuario y, por lo tanto, no podrá acceder a las API específicas de usuario (por ejemplo, GET /v2/users/me). 
> Si asigna roles para Genesys Cloud for Salesforce, consulte también los permisos de cliente OAuth para Genesys Cloud for Salesforce.  
  * **Concesión de autorización de código:** Un proceso de autenticación de dos pasos en el que un usuario se autentica con Genesys Cloud y, a continuación, se devuelve a la aplicación cliente un código de autorización. La aplicación cliente proporciona credenciales de cliente OAuth y utiliza el código de autorización para obtener un token de acceso. El token de acceso se puede utilizar para realizar llamadas a la API autenticadas. Esta es la opción más segura e ideal para sitios web en los que las solicitudes de API se realizan desde el servidor (por ejemplo, ASP.NET o PHP) y algunas aplicaciones de escritorio en las que un cliente ligero autoriza al usuario y pasa el código de autenticación a un servidor back-end para intercambiarlo por un token de autenticación y realizar solicitudes de API. 
  * **Concesión implícita (navegador):** Un proceso de autenticación de un solo paso en el que un usuario se autentica con Genesys Cloud y la aplicación cliente recibe directamente un token de acceso. Esta opción proporciona menos seguridad para el token de acceso que la concesión de código de autorización, pero es ideal para aplicaciones de navegador del lado del cliente (es decir, JavaScript) y la mayoría de las aplicaciones de escritorio (por ejemplo, .NET WPF/WinForms o programas de escritorio Java). 
  * **Portador SAML2:** Proceso de autenticación en el que una aplicación cliente puede utilizar una aserción del Lenguaje de Marcado de Aserción de Seguridad (SAML2) para solicitar un token de portador. Véase también: Solución de inicio de sesión único y proveedor de identidad de Genesys Cloud (Genesys Cloud single sign-on and identity provider solution).

**Asignar un Rol**

Al crear un token OAuth, necesitamos restringir o permitir a qué tipos de datos tiene acceso el token. Esto es principalmente una medida de seguridad para asegurar que ningún auth token tiene más acceso del que debería. Bajo las reglas, asignaremos los permisos apropiados a los que el token Auth llamará cuando ejecute el contrato, que definimos en la acción de datos.

>**Para conceder funciones a un cliente OAuth, debe tener esas funciones asignadas a su perfil.**

![image](/images/auth2.PNG)

Ahora que tenemos nuestro cliente Oauth configurado, es el momento de configurar la integración con Genesys CX.

Vaya a Admin > Integraciones, busque "Genesys Cloud Data Actions" y seleccione "Instalar" en este mosaico.

![image](/images/auth3.PNG)

Vaya a la pestaña Configuración > Credenciales y seleccione Configurar. Se te pedirá la información de tu cliente OAuth.

![image](/images/auth4.PNG)

Después de guardar su integración, ya puede construir las Acciones de Datos en la Nube de Genesys.