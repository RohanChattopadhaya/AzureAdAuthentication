
1.  ### Web Api Details for Authentication
   - Create web api App Rgistration in Azure AD by giving specific name to it which will create clientid(Application ID), object ID with tenant ID of subscription.
   
       ![image](https://user-images.githubusercontent.com/40242212/141673337-6868daaf-35e2-4327-a987-2938d14f1681.png)

   - In Authentication section add redirect URI to it's URL.

        ![image](https://user-images.githubusercontent.com/40242212/141673398-539f1e2f-9608-48f5-ad40-02924199465e.png)

   - From Expose API section need to add a scope with details for the delegated permission to access this API from UI app or Postman.
   
       ![Capture](https://user-images.githubusercontent.com/40242212/141674769-15d87316-b64c-4b6f-9668-8ee2507fdda2.PNG)


2.  ### Now Need To Create a Web API Application
   - In Web API .Net Core Application inside appsettings.json file need to add below section
   
   
   ``` 
   "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "ClientId": "", //Application (client) ID
    "TenantId": "",
    "Audience": "api://, //Application (client) ID
    "Issuer": "https://login.microsoftonline.com/{tenantid}/v2.0" //Directory (tenant) ID Overview blade of App Registration
    }
  
  ```
   - Add the package Microsoft.AspNetCore.Authentication.AzureAD.UI
   - In startup.cs file add below section of code 
   
   ```
   services.AddAuthentication(sharedOptions =>
            {
                sharedOptions.DefaultScheme = AzureADDefaults.AuthenticationScheme;
            })
                   .AddJwtBearer("AzureAD", options =>
                   {
                       options.Audience = Configuration.GetValue<string>("AzureAd:Audience");
                       options.Authority = Configuration.GetValue<string>("AzureAd:Instance") +
                       Configuration.GetValue<string>("AzureAd:TenantId");

                       options.TokenValidationParameters = new TokenValidationParameters()
                       {
                           ValidIssuer = Configuration.GetValue<string>("AzureAd:Issuer"),
                           ValidAudience = Configuration.GetValue<string>("AzureAd:Audience")
                       };
             });
   ```
   - Add Authorize attribute in controller to prevent UnAuthorized access
   
 3. ### Web App or Postman Details for Authentication and Access permitted Web API.
 
    - Create web app App Rgistration in Azure AD by giving specific name to it which will create clientid(Application ID), object ID with tenant ID of subscription.
    - In Authentication section add redirect URI to it's URL in case of postman(https://www.postman.com/oauth2/callback) and at the same page check the ID Token , Access token Checkbox. ``` (ID token - A JWT that contains claims that you can use to identify users in your application. ... Access token - A JWT that contains claims that you can use to identify the granted permissions to your APIs) ```
    
     ![image](https://user-images.githubusercontent.com/40242212/141674805-d76eca9e-218e-48d4-9ad7-ea8633af0833.png)
     
    - Add API Permission to add delegated permission of web api(App Registration of web api in Azure AD) to Web App or Postman
       ![cap](https://user-images.githubusercontent.com/40242212/141674957-d8edb4d1-007b-4f8b-805b-64c301795cbb.png)

       ![cap2](https://user-images.githubusercontent.com/40242212/141675048-3499b5b1-0e7a-428c-a29c-fa53d18de632.png)
       
    - In Web API app registration's Expose API section add Web app client ID of Web App Regitration.
       ![Capture4](https://user-images.githubusercontent.com/40242212/141675309-dd3c2cd3-4440-423f-a481-9c2d8475f528.png)
       
    - Need to call OAuth 2.0 token endpoint (v1) (We can find in Endpoint section of Azure AD) from Postman or Web APP using grant_type(client_credentials), clientId,Client Secret(Client secret can be created) and resource(Scope of Web Api what is created before -> Ex - api://ClientIDWebAPI).
        ```
        https://login.microsoftonline.com/{tenantID}/oauth2/token
        ```        
        This will fetch bearer token which will be used in Header as 'Authorization' to call web API. 
        
        *** In case of web application we can use scope to get the access token using ITokenAcquisition.Also need to add services for scope in configure service method of startup class.
        
        
        




    
              

    
