---
title: "Integration and oAuth"
chapter: false
weight: 30
---

## Integration and OAuth
To start, we will walk through how we build and authenticate data actions (or data dips) within the Genesys CX UI. Before creating the integration for the data action, we must first have something to authenticate it to. This is where we create our Oauth Client token. OAuth clients allow you to make requests to the Platform API or to authenticate against Genesys Cloud, or to sync entities between Genesys Cloud and third-party systems.

There are 3 primary integration types we encounter when looking to create an integration, the first is Platform API, Genesys CX Embeddable frameworks, and Genesys SCIM. For today, we will be focusing on a Platform API.

**Platform API:**

This procedure is for application providers who want their app to receive a token allowing it to make requests to the Genesys Cloud Platform API. The token represents a user’s permission for the app to access Genesys Cloud data. It is used when the app must authorize a request to an API endpoint. To see a list of Genesys Cloud Platform APIs, see the API resources in the Genesys Cloud Developer Center.

![image](/images/auth1.PNG)

Make a selection below Grant Types. Grant Types set the way an application gets an access token. Genesys Cloud supports the OAuth 2 authorization grant types listed below. Clicking the name of a grant type displays more information about it from the Genesys Cloud Developer Center. 
  * **Client Credentials Grant:** A single-step authentication process exclusively for use by non-user applications (e.g. a Windows Service or cron job). The client application provides OAuth client credentials in exchange for an access token. This authorization type is not in the context of a user and therefore will not be able to access user-specific APIs (e.g GET /v2/users/me). 
> If assigning roles for Genesys Cloud for Salesforce, see also OAuth client permissions for Genesys Cloud for Salesforce. 
   * **Code Authorization Grant:** A two-step authentication process where a user authenticates with Genesys Cloud, then the client application is returned an authorization code. The client application provides OAuth client credentials and uses the authorization code to get an access token. The access token can then be used when making authenticated API calls. This is the most secure option and ideal for websites where API requests will be made server-side (e.g. ASP.NET or PHP) and some desktop applications where a thin client would authorize the user and pass the auth code to a back-end server to exchange for an auth token and make API requests. 
  * **Implicit Grant (Browser):** A single-step authentication process where a user authenticates with Genesys Cloud and the client application is directly returned an access token. This option provides less security for the access token than the authorization code grant, but is ideal for client-side browser applications (i.e. JavaScript) and most desktop applications (e.g. .NET WPF/WinForms or Java desktop programs). 
  * **SAML2 Bearer:** An authentication process wherein a client application may use a Security Assertion Markup Language (SAML2) assertion to request a bearer token. See also: Genesys Cloud single sign-on and identity provider solution.

**Assign a Role:**

When creating an Oauth token, we need to restrict or allow what types of data the token has access to. This is primarily a security measure to ensure that no auth token has more access than it should. Under the rules, we will assign the appropriate permissions the Auth token will call against when executing the contract, we define in the data action.

>**To grant roles to an OAuth client, you must have those roles assigned to your profile.**

![image](/images/auth2.PNG)

Now that we have our Oauth Client configured, its time we set up the integration with Genesys CX.

Navigate to Admin > Integrations and search for "Genesys Cloud Data Actions", you will select install on this tile.

![image](/images/auth3.PNG)

Navigate to the Configuration tab > Credentials and select Configure. You will be prompted for your OAuth client information.

![image](/images/auth4.PNG)

After saving your integration, you can now construct Genesys Cloud data actions!