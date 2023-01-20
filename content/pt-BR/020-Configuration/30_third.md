---
title: "Integração e OAuth"
chapter: false
weight: 30
locale: "pt-BR"
---

## Integração e OAuth
Para começar, veremos como construímos e autenticamos ações de dados (ou mergulhos de dados) na interface do usuário do Genesys CX. Antes de criar a integração para a Ação de Dados, devemos primeiro ter algo para autenticá-la. É aqui que criamos nosso token do OAuth Client. Os clientes OAuth permitem que você faça solicitações à API da plataforma ou autentique-se no Genesys Cloud ou sincronize entidades entre o Genesys Cloud e sistemas de terceiros.

Existem 3 tipos principais de integração que encontramos quando procuramos criar uma integração; API de plataforma, estruturas incorporáveis Genesys CX e Genesys SCIM.

 Hoje, vamos nos concentrar na API da plataforma..

**API da Platforma:**

Este procedimento é para provedores de aplicativos que desejam que seu aplicativo receba um token que permita fazer solicitações à API Genesys Cloud Platform. O token representa a permissão de um usuário para que o aplicativo acesse os dados do Genesys Cloud. Ele é usado quando o aplicativo deve autorizar uma solicitação para um terminal de API. Para ver uma lista de APIs da Genesys Cloud Platform, consulte os recursos da API no Genesys Cloud Developer Center.

Navegue até Administrador > OAuth e crie um novo cliente

![image](/images/auth1.PNG)

Para este workshop, selecionaremos as **credenciais do cliente** abaixo dos tipos de concessão (Grant Types). 

Tipos de concessão definem a maneira como um aplicativo obtém um token de acesso. Genesys Cloud suporta os tipos de concessão de autorização OAuth 2 listados abaixo. Clicar no nome de um tipo de concessão exibe mais informações sobre ele no Genesys Cloud Developer Center. 
  * **Concessão de credenciais do cliente:** Um processo de autenticação de etapa única exclusivamente para uso por aplicativos que não são do usuário (por exemplo, um serviço do Windows ou trabalho cron). O aplicativo cliente fornece credenciais de cliente OAuth em troca de um token de acesso. Este tipo de autorização não está no contexto de um usuário e, portanto, não poderá acessar APIs específicas do usuário (por exemplo, GET /v2/users/me). 
> Se atribuir funções para Genesys Cloud for Salesforce, consulte também permissões de cliente OAuth para Genesys Cloud for Salesforce.  
  * **Concessão de Autorização de código:** Um processo de autenticação em duas etapas em que um usuário se autentica com o Genesys Cloud e, em seguida, o aplicativo cliente recebe um código de autorização. O aplicativo cliente fornece credenciais de cliente OAuth e usa o código de autorização para obter um token de acesso. O token de acesso pode então ser usado ao fazer chamadas de API autenticadas. Esta é a opção mais segura e ideal para sites onde as solicitações de API serão feitas do lado do servidor (por exemplo, ASP.NET ou PHP) e alguns aplicativos de desktop onde um thin client autorizaria o usuário e passaria o código de autenticação para um servidor de back-end para trocar por um token de autenticação e fazer solicitações de API. 
  * **Concessão implícita de token (Navegador):** Um processo de autenticação de etapa única em que um usuário se autentica com o Genesys Cloud e o aplicativo cliente recebe diretamente um token de acesso. Esta opção fornece menos segurança para o token de acesso do que a concessão do código de autorização, mas é ideal para aplicativos de navegador do lado do cliente (ou seja, JavaScript) e a maioria dos aplicativos de desktop (por exemplo, .NET WPF/WinForms ou programas de desktop Java).. 
  * **Portador SAML2:** Um processo de autenticação em que um aplicativo cliente pode usar uma asserção Security Assertion Markup Language (SAML2) para solicitar um token de portador. Veja também: Genesys Cloud single sign-on e solução de provedor de identidade.

**Atribuir uma função**

Ao criar um token OAuth, precisamos restringir ou permitir quais tipos de dados o token tem acesso. Isso é principalmente uma medida de segurança para garantir que nenhum token de autenticação tenha mais acesso do que deveria. De acordo com as regras, atribuiremos as permissões apropriadas contra as quais o token OAuth será chamado ao executar o contrato, definimos na ação de dados.

**Para conceder funções a um cliente OAuth, você deve ter essas funções atribuídas ao seu perfil.**

![image](/images/auth2.PNG)

Agora que temos nosso cliente Oauth configurado, é hora de configurar a integração com o Genesys CX.

Navegue até Administrador > Integrações, procure por “Genesys Cloud Data Actions” e selecione “Install” neste bloco

![image](/images/auth3.PNG)

Navegue até a guia Configuração > Credenciais e selecione Configurar. Você será solicitado a fornecer as informações do cliente OAuth.

![image](/images/auth4.PNG)

Depois de salvar sua integração, agora você pode construir Genesys Cloud Data Actions!

![image](/images/integrationactive.PNG)

