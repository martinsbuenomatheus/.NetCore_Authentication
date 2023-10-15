# .Net Core Authentication

## References
![React.JS + MSAL with ASP.NET Core to use Azure AD with User & Role](https://dnilvincent.com/blog/posts/reactjs-msal-with-aspnetcore-to-use-azure-ad-part3)

![OAuth 2.0 authorization code flow with a React SPA, ASP.NET Core Web API, RBAC roles, and MSAL](https://keithbabinec.com/2020/09/27/oauth-2-0-authorization-code-flow-with-a-react-spa-asp-net-core-web-api-rbac-roles-and-msal/)

## Libraries
> using Microsoft.AspNetCore.Authentication.JwtBearer; </br>
> using Microsoft.Identity.Web; </br>

## Architecture
### Azure Configuration
* Web App
    * Configuration
        * APP_CLIENT_ID: ClientID from App Registration
        * APP_CLIENT_SECRET_ID: Secret from App Registration
        * APP_TENANT_ID: TenantId Azure
        * ASPNETCORE_ENVIRONMENT: Defines the environment (default: Production)
    * Authentication: left blank

* App Registration
    * Note: Application (client) ID e Directory (tenant) ID
    * Authentication
        * Single Page Application: set the online redirect URL
        * Mobile and desktop applications: set the local redirect URL (for debug purposes)
        * Implicit grant: Access tokens and ID tokens
        * Supported account types: Directory Only
        * Allow public client flows: Yes

    * Certificates and Secrets
        * Create a client secret to access the KeyVault

    * API Permissions
        * Create a permission for user impersonation

    * Expose an API
        * Create a scope, selecting the permission created before

### Configurações da API
* launchSettings.json
    * File is override by the Azure Configuration

* Program.cs
    * Initialize KeyVault

* Startup.cs
    * Set AddAuthentication


## Implementation
### Configuration file (appsettings.json)
    "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "{your_domain}",
    "ClientId": "{clientID}",
    "Audience": "{clientID}",
    "TenantId": "{tenantID}",
    "ClientCertificates": []
    },
  
 ## Program.cs (ou Startup.cs)
 Add Authenticação using appsettings parameters
 
    // Add Microsoft Identity Platform authentication
    builder.Services
    .AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddMicrosoftIdentityWebApi(builder.Configuration, "AzureAd");

    // Add authorization policies if needed
    builder.Services.AddAuthorization();


> Initialize the application in this order

    app.UseHttpsRedirection();

    app.UseAuthentication();
    app.UseAuthorization();

    app.MapControllers();

## Controllers
> using Microsoft.AspNetCore.Authorization;

## Endpoints
> Decorate with [Authorize] to force authorization/authentication