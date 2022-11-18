# CMPG323-Overview-25102737

Main (overview) repository for CMPG323 third (3rd) year, second (2nd) semester.

## Background

This repository is an overview of all the semester's work (projects). It contains each project's structure, how to use it, useful links as well as references. Furthermore, it provides additional topics, such as branching strategies used, handling of security (.gitignore files) and overall project management.

## Source Control

Source control is used in every project to showcase the iterative steps taken to develop, implement and deploy them. [GitHub](https://www.github.com) is used to achieve this, along with Visual Studio and/or CLI commands (depending on the project).

### Branching strategy

The [Gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow) workflow branching strategy was used in all of the projects. In this branching strategy, instead of only a single main (previously master) branch, two branches are used to capture & track the history of a project/solution. The main branch is used to store the history or previous versions of a project (often by using tags). The development branch (usually dev), is used for integration of various other branches where features are applied to it. When a feature is complete, a pull request is made and merged onto the dev branch. When a major, minor or hotfix needs to be deployed in a production environment, the dev branch is merged with the main branch.

![02 Feature branches](https://user-images.githubusercontent.com/44663302/202712508-b70e0dcb-8421-4183-8480-51bf4449a349.svg)

Atlassian Bitbucket (n.d.). Git flow workflow - Feature Branches. Available at: https://wac-cdn.atlassian.com/dam/jcr:34c86360-8dea-4be4-92f7-6597d4d5bfae/02%20Feature%20branches.svg?cdnVersion=637 [Accessed 18 Nov. 2022].

## Security

### .gitignore

The use of .gitignore files ensured that sensitive information (e.g. files, credentials) were excluded from the repository. Sensitive information includes:

- Connection strings
- Credentials (specifically admin usernames & passwords)

### Role-based authentication & authorization

In some projects (specifically project 2 & 3), the use of roles to secure certain endpoints were implemented. Only users that have the desired role are granted access to the protected resource.

## Projects

In this semester we had to learn about, implement and deploy 5 projects:

- Project 1 (Agile & Scrum) - This project/repository
- [Project 2 (API Development)](https://github.com/Nicmanthecodeman/CMPG323-Project2-25102737)
- [Project 3 (Standards & Patterns)](https://github.com/Nicmanthecodeman/CMPG323-Project3-25102737)
- [Project 4 (Testing & RPA)](https://github.com/Nicmanthecodeman/CMPG323-Project4-25102737)
- [Project 5 (Reporting & Monitoring)](https://github.com/Nicmanthecodeman/CMPG323-Project5-25102737)

### Project 2 (API Development)

# Connected Office API

Connected Office API is an ASP.NET Core 6.0 Web Api solution. The solution was written with Domain Driven Design (DDD) concepts in mind in order to implement Clean architecture. This ensures that the solution is easy maintainable as well as extendable. The Repository Pattern as well as the Command-Query Responsibility Segregation Pattern were implemented.

## Structure

The solution comprises of 4 projects, each with its own responsibilities: 
- The **Core** project contains Domain logic and is not dependant on any other projects. 
- The **Application** project contains Application-specific logic and is dependent on the Core project. 
- The **Infrastructure** project contains logic related to the physical configuration/infrastructure for the API (such as configuration of database, service registration, etc.). This project is dependent on the Application project. Lastly, 
- The **Api** project contains the Controllers/Endpoints for the API and is dependent on the Infrastructure project.

**Note.** All the services required for dependency injection can be found in the `Extensions.cs` classes in the **Application** and **Infrastructure** project in the root directory. `Program.cs` (in the Api project) calls all these extension methods for service registration.

- **Api**
  - Controllers
     -  Categories
     -  Devices
     -  Identity
     -  Zones
  - `Program.cs` 
- **Infrastructure**
  - EntityFramework
    - Configurations
    - Contexts
    - Migrations
    - Queries
    - Repositories
    - Sieve
  - Exceptions
    - ExceptionToResponseMapper
  - Initializers
  - `Extensions.cs`
- **Application**
  - Commands
     - Handlers
  - DTO
  - Exceptions
  - Models
  - Queries
  - `Extensions.cs`
- **Core** 
  - Constants
  - Entities
  - Exceptions
  - Repositories

![0_AfaPdOeYH4wKTGY1](https://user-images.githubusercontent.com/44663302/189095116-b8e8b714-367b-4784-bb33-583cb5a5ee11.png)

_Image reference:_ Slabbinck, A., 2022. Explaining Clean Architecture In .Net Core (Breakdown & Example). [online] Medium. Available at: <https://blog.devgenius.io/explaining-clean-architecture-in-net-core-breakdown-example-f197663964c7> [Accessed 8 September 2022].

## How to start the solution

1. Clone the repository on your local machine
2. Rebuild the solution to restore all nuget packages
3. Add an `appsettings.json` file with the following objects:
>`
"JWT": {
    "ValidAudience": "valid-audience-url",
    "ValidIssuer": "valid-issuer-url",
    "Secret": "Your-very-secure-secret"
  }
`
>`
"ConnectionStrings": {
    "PersistenceConnection": "your-connection-string",
  }
`
4. Ensure that the following tables exists: Device, Zone, Category. You can use the Database.sql script in the docs folder of the repository.
5. Migrations for the Identity will automatically run on application startup.
6. Run the solution

## What HTTP requests can be sent to the API?

You can test all endpoints of the Connected Office API.

The documentation for all the endpoints can be found [here](https://con-office.azurewebsites.net).

**Note.** All endpoints, except those needed for authentication, are protected.

Follow these steps to use the API:

1. Register as an admin/user by **POSTing** to the endpoints https://apim-project2-dev.azure-api.net/connected-office/api/identity/register-admin and https://apim-project2-dev.azure-api.net/connected-office/api/identity/register, respectively. You can either use the swagger docs or use a client, e.g. [Postman](https://www.postman.com/). Use the following in the request body: `{
  "username": "string",
  "email": "user@example.com",
  "password": "string"
}`
2. After getting a 200 (OK) response, you can retrieve a token by **POSTing** to this endpoint https://apim-project2-dev.azure-api.net/connected-office/api/identity/login with the same username and password used in step 1. You can use the following in the request body: `{
  "username": "string",  
  "password": "string"
}`
3. After getting a 200 (OK) response, copy the `token` value returned by the API. **Note** the expiration time.
4. Use this access token in the Authorization header for all subsequent requests. (Remember to prepend `Bearer` followed by a space and then the access token, e.g. 'Bearer \<your-access-token>')

## Important links

- [Endpoints (Swagger)](https://con-office.azurewebsites.net/swagger/index.html)
- [References](https://github.com/Nicmanthecodeman/CMPG323-Project2-25102737/blob/main/docs/25102737_CMPG323_Project2_References.pdf)
- [Api Gateway](https://apim-project2-dev.azure-api.net/connected-office)
  - If using the gateway, append `/api/[controller]/[action]` to the gateway url above, e.g. /api/devices
  
## To use the built-in filter features, follow these steps:
[Sieve](https://github.com/Biarity/Sieve) is another GitHub repository making it very easy to integrate filtering, sorting and pagination using Entity Framework Core. For a more detailed explanation on how to use it, follow [this](https://github.com/Biarity/Sieve#send-a-request) link.

If the query parameters are left empty, as seen in the following image, then the Api will retrieve **all** records without filtering, sorting and pagination.

![image](https://user-images.githubusercontent.com/44663302/189119585-e9b85f0c-5de8-4b4a-8f01-76e0086952ab.png)

You can apply pagination by specifying the `Page` and `PageSize` query parameters:
- Page indicate the index of the page, e.g. page 1, 2, 3, etc.
- PageSize indicate the amount of records on each page

In the following example, the records will be shown on page 1 with only 5 items.

![image](https://user-images.githubusercontent.com/44663302/189120096-5d0c061e-787d-4398-9f2c-abd9d97db8f5.png)

You can apply sorting by specifying the `Sorts` query parameter. Simply just enter the name of the property (camelCase), e.g. description, in order to sort it in ascending order or add a '-' before the property in order to sort in descending order, e.g. -name. Multiple properties can be seperated by a comma as seen below: 

![image](https://user-images.githubusercontent.com/44663302/189120727-757bce7b-af62-4ddf-a26a-d4860772481f.png)

In order to use the filtering capabilities of the Api, use the `Filters` query parameter. The specified format is `{Name}{Operator}{Value}` where:
- Name is the camelCase name of the property, e.g. description, name, dateCreated.
- Operator is the type of filter you want to apply, e.g. ==, !=, etc. The full list of operators can be found [here](https://github.com/Biarity/Sieve#operators).
- Value is what you are searching/filtering for.

An example can be seen in the following image: The operator is contains, thus the result would be all records where the name contains the word 'car'.

![image](https://user-images.githubusercontent.com/44663302/189121864-49b22e27-a303-4c72-acee-fe001293787f.png)

## Endpoints

**Note**: I have used annotations on the enpoints (using Swagger Nuget package: [Swashbuckle Annotations](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Annotations), and the API Manager (Azure) picks this up and displays only the annotations I gave them.) I have placed all the actual endpoints' routes below the screenshots of the API Manager.

![image](https://user-images.githubusercontent.com/44663302/189735841-54d2feb1-b752-4663-9ff3-f846a7528e05.png)
![image](https://user-images.githubusercontent.com/44663302/189735898-3b64aae4-b2c4-4b2f-866f-daa075b525a6.png)

### Categories

- **GET** api/Categories
- **GET** api/Categories/{categoryId:guid}/devices
- **GET** api/Categories/{categoryId:guid}/zones
- **GET** api/Categories/{categoryId:guid}
- **POST** api/Categories
- **PUT** api/Categories/{categoryId:guid}
- **DELETE** api/Categories/{categoryId:guid}

### Devices 

- **GET** api/Devices
- **GET** api/Devices/{deviceId:guid}
- **POST** api/Devices
- **PUT** api/Devices/{deviceId:guid}
- **DELETE** api/Devices/{deviceId:guid}

### Identity
- **POST** api/Identity/login
- **POST** api/Identity/register
- **POST** api/Identity/register-admin

### Zones

- **GET** api/Zones
- **GET** api/Zones/{zoneId:guid}/devices
- **GET** api/Zones/{zoneId:guid}
- **POST** api/Zones
- **PUT** api/Zones/{zoneId:guid}
- **DELETE** api/Zones/{zoneId:guid}

## References

1. Anodide, A., Copsey, R. and Wood, A., 2022. Is there way to use Distinct in LINQ 
query syntax?. [online] Stack Overflow. Available at: 
<https://stackoverflow.com/questions/5720945/is-there-way-to-use-distinct-in-linq-query-syntax> [Accessed 2 September 2022].
2. Babaki, N., 2022. JWT Authentication and Swagger with .NET Core 3.0. [online] 
Stack Overflow. Available at: <https://stackoverflow.com/questions/58179180/jwt-authentication-and-swagger-with-net-core-3-0> [Accessed 6 September 2022].
3. Benedichuk, V. and Reese, T., 2022. How to deploy Azure SQL S0 instance with 
ansible azure_rm_sqldatabase?. [online] Stack Overflow. Available at: 
<https://stackoverflow.com/questions/56594254/how-to-deploy-azure-sql-s0-
instance-with-ansible-azure-rm-sqldatabase> [Accessed 7 September 2022].
4. Bhatti, S., 2022. New deployed Azure web app is returning 404 Error code. 
[online] Stack Overflow. Available at: 
<https://stackoverflow.com/questions/69560100/new-deployed-azure-web-app-is-returning-404-error-code> [Accessed 6 September 2022].
5. Convey-stack.github.io. 2022. Convey. [online] Available at: <https://convey-stack.github.io/> [Accessed 4 September 2022].
6. Cooke, M., 2022. Using .gitignore file to hide appsettings.json does not actually 
hide it. [online] Stack Overflow. Available at: 
<https://stackoverflow.com/questions/49930776/using-gitignore-file-to-hideappsettings-json-does-not-actually-hide-it> [Accessed 6 September 2022].
7. Docs.microsoft.com. 2022. Configure a connection string - Azure Storage. 
[online] Available at: <https://docs.microsoft.com/en-us/azure/storage/common/storage-configure-connection-string> [Accessed 8 
September 2022].
8. Docs.microsoft.com. 2022. Define your naming convention - Cloud Adoption 
Framework. [online] Available at: <https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/resource-naming> [Accessed 7 
September 2022].
9. Docs.microsoft.com. 2022. Identity model customization in ASP.NET Core. 
[online] Available at: <https://docs.microsoft.com/en-us/aspnet/core/security/authentication/customize-identity-model?view=aspnetcore-6.0#customize-the-model> [Accessed 7 September 
2022].
10. Docs.microsoft.com. 2022. Recommended abbreviations for Azure resource 
types - Cloud Adoption Framework. [online] Available at: 
<https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/resource-abbreviations> [Accessed 2 September 2022].
11. Docs.microsoft.com. 2022. Tutorial - Import and publish your first API in Azure 
API Management. [online] Available at: <https://docs.microsoft.com/en-us/azure/api-management/import-and-publish> [Accessed 7 September 2022].
12. Evans, E. and Szpoton, R., 2022. Domain-driven design. 1st ed. Gliwice: Helion.
13. GitHub. 2022. GitHub - ketan-gote/ddd-example: Domain Driven Design. 
Examples focuses on key concept of ddd like Entities, Aggregate root, 
Repository, Value Objects & ACL.. [online] Available at: 
<https://github.com/ketan-gote/ddd-example> [Accessed 6 September 2022].
14. Hart, M. and Smith, T., 2022. Where do I find some good examples for DDD?. 
[online] Stack Overflow. Available at: 
<https://stackoverflow.com/questions/540130/where-do-i-find-some-good-examples-for-ddd> [Accessed 5 September 2022].
15. Kanjilal, J., 2022. How to implement JWT authentication in ASP.NET Core 6. 
[online] InfoWorld. Available at: <https://www.infoworld.com/article/3669188/how-to-implement-jwt-authentication-in-aspnet-core-6.html> [Accessed 6 September 
2022].
16. Klaus, A., 2022. Swagger response description not shown. [online] Stack 
Overflow. Available at: <https://stackoverflow.com/questions/62062751/swagger-response-description-not-shown> [Accessed 8 September 2022].
17. Kudryavtsev, D. and Lowe, D., 2022. Use of PUT vs PATCH methods in REST 
API real life scenarios. [online] Stack Overflow. Available at: <https://stackoverflow.com/questions/28459418/use-of-put-vs-patch-methods-in-rest-api-real-life-scenarios> [Accessed 7 September 2022].
18. McCabe, R. and Kozera, J., 2022. ASP.net Core Web API - correct swagger 
annotations. [online] Stack Overflow. Available at: 
<https://stackoverflow.com/questions/61859633/asp-net-core-web-api-correct-swagger-annotations> [Accessed 4 September 2022].
19. Shah, J., 2022. How to configure Swashbuckle to ignore property on model. 
[online] Stack Overflow. Available at: 
<https://stackoverflow.com/questions/41005730/how-to-configure-swashbuckle-to-ignore-property-on-model> [Accessed 7 September 2022].
20. siliconvalve. 2022. Understanding Azure App Service Plans and Pricing. [online]
Available at: <https://blog.siliconvalve.com/2016/05/12/understanding-azure-app-service-plans-and-pricing/> [Accessed 7 September 2022].
21. Stack Overflow. 2022. C# Swagger/Swashbuckle Schemas blank if I upgrade 
above 5.0.0-rc4. [online] Available at: 
<https://stackoverflow.com/questions/61920535/c-sharp-swagger-swashbuckle-schemas-blank-if-i-upgrade-above-5-0-0-rc4> [Accessed 3 September 2022].
22. Stack Overflow. 2022. Could not parse the JSON file. .Net Core 5.0 Web API. 
[online] Available at: <https://stackoverflow.com/questions/66260483/could-not-parse-the-json-file-net-core-5-0-web-api> [Accessed 7 September 2022].
23. Stack Overflow. 2022. How can I create the free 250GB SQL Server Database 
promised with Free Azure Subscription. [online] Available at: 
<https://stackoverflow.com/questions/64436514/how-can-i-create-the-free-250gb-sql-server-database-promised-with-free-azure-sub> [Accessed 7 September 
2022].
24. Stack Overflow. 2022. LINQ Query Syntax (Count Operation). [online] Available 
at: <https://stackoverflow.com/questions/18879399/linq-query-syntax-count-operation> [Accessed 7 September 2022].
25. Sundal, F., Jr, J. and Dimitrov, D., 2022. How to get Type of Exception in C#. 
[online] Stack Overflow. Available at: <https://stackoverflow.com/questions/3715380/how-to-get-type-of-exception-in-c-sharp> [Accessed 6 September 2022].
26. Yu, N., 2022. Swagger showing no parameters to controller actions that have 
paramters. [online] Stack Overflow. Available at: 
<https://stackoverflow.com/questions/60573462/swagger-showing-no-parameters-to-controller-actions-that-have-paramters> [Accessed 5 September 
2022].

### Project 3 (Standards & Patterns)

# Connected Office MVC

Connected Office MVC is an ASP.NET Core 6.0 MVC solution. The solution was written with Domain Driven Design (DDD) concepts in mind in order to implement Clean architecture. This ensures that the solution is easy maintainable as well as extendable. The Repository Pattern as well as the Command-Query Responsibility Segregation Pattern were implemented.

## Structure

The solution comprises of 4 projects, each with its own responsibilities: 
- The **Core** project contains Domain logic and is not dependant on any other projects. 
- The **Application** project contains Application-specific logic and is dependent on the Core project. 
- The **Infrastructure** project contains logic related to the physical configuration/infrastructure for the WebClient (such as configuration of database, service registration, etc.). This project is dependent on the Application project. Lastly, 
- The **WebClient** project contains the Controllers/Endpoints which the end-user can call and is dependent on the Infrastructure project.

**Note.** All the services required for dependency injection can be found in the `Extensions.cs` classes in the **Application** and **Infrastructure** project in the root directory. `Program.cs` (in the WebClient project) calls all these extension methods for service registration.

## Patterns
- Repository Pattern
- Unit Of Work Pattern
- CQRS Pattern

## Frontend
- Semantic UI
- Knockout.js
- JQuery
- ViewComponents (ASP.NET Core)

### Solution Structure
- **WebClient**
  - Areas
     - DeviceManagement
        - Categories
        - Devices
        - Zones
     - Identity     
  - Controllers
     -  Home
  - Models
  - Components
  - Constants
  - `Program.cs` 
- **Infrastructure**
  - Constants
  - EntityFramework
    - Configurations
    - Contexts
    - Queries
    - Repositories
    - Sieve
  - Services
  - `Extensions.cs`
- **Application**
  - Commands
     - Handlers
  - DTO
  - Exceptions
  - Models
  - Queries
  - Services
  - `Extensions.cs`
- **Core** 
  - Common  
  - Entities
  - Events
  - Exceptions
  - Repositories

![0_AfaPdOeYH4wKTGY1](https://user-images.githubusercontent.com/44663302/189095116-b8e8b714-367b-4784-bb33-583cb5a5ee11.png)

_Image reference:_ Slabbinck, A., 2022. Explaining Clean Architecture In .Net Core (Breakdown & Example). [online] Medium. Available at: <https://blog.devgenius.io/explaining-clean-architecture-in-net-core-breakdown-example-f197663964c7> [Accessed 8 September 2022].

## How to use the web application

1. Open the website by clicking [here](https://con-office-mvc.azurewebsites.net/). (Please hold the `Control` key in while pressing the link)
2. Log in to the web application with the following credentials:
> UserName: `admin`

> Password: `Pass123$`

![image](https://user-images.githubusercontent.com/44663302/193004868-27c4f6d3-cdec-4e6d-9128-44e607bcca75.png)

3. You will be redirected to the dashboard if login was successful. Here you can view an overall summary for devices, categories and zones.

![image](https://user-images.githubusercontent.com/44663302/193005141-69e6d53d-ed7d-40bc-b67d-d327d66a2317.png)

4. In order to navigate to the various pages (e.g. devices, categories or zones) you can use the side navigation bar on the left-hand side of the screen or you can use the round links on the dashboard.

![image](https://user-images.githubusercontent.com/44663302/193005856-42fdcf22-f411-4276-9659-28e0f8002013.png)
![image](https://user-images.githubusercontent.com/44663302/193005544-a29c4598-0104-481e-8837-70676ab7304c.png)

**Note** You can collapse/expand the sidebar by clicking on the hamburger icon above the sidebar

5. To maintain devices, categories or zones, you can follow these steps. (Note that devices will be used, but the other pages follow the same workflow)

Click on devices to **View the List** of devices.

![image](https://user-images.githubusercontent.com/44663302/193006245-ea15bb40-774f-46a0-ad21-9c661170dded.png)

#### Create

In order to create a new device, press the green add new device button on the right-hand side of the title bar. The following will be displayed:

![image](https://user-images.githubusercontent.com/44663302/193006414-090f3cfa-3ad5-4bbb-a7a8-9dadcd1d1331.png)

You can enter appropriate values in the form. Note that the Name field is required.

![image](https://user-images.githubusercontent.com/44663302/193006587-04ca95e2-0f04-4841-a588-de8a8cbdab42.png)

If the form's validation passed, you will see the newly added device in the list along with this message:

![image](https://user-images.githubusercontent.com/44663302/193006756-cebbff23-a5da-4801-b926-a65970548020.png)

#### Edit

If you want to edit a device, press the `Actions` dropdown button in the leftmost column of the list and choose `Edit`

![image](https://user-images.githubusercontent.com/44663302/193007112-dfc0cf5e-4018-48cd-afa1-30500cbda8c4.png)

The device's values will be populated in the form, allowing you to edit the values.

![image](https://user-images.githubusercontent.com/44663302/193007250-f6c4256d-f0f2-4eb4-8031-dd44828ad056.png)

After the form's validation passed the device will be updated in the list along with this message:

![image](https://user-images.githubusercontent.com/44663302/193007390-70d3e520-da88-4306-943a-067fe14aa93a.png)

#### Delete

If you want to edit a device, press the `Actions` dropdown button in the leftmost column of the list and choose `Delete`

![image](https://user-images.githubusercontent.com/44663302/193007112-dfc0cf5e-4018-48cd-afa1-30500cbda8c4.png)

A confirmation dialog will ask you to confirm the `Delete` action

![image](https://user-images.githubusercontent.com/44663302/193007575-e4799f21-9f4a-4222-92a1-37e5dc827417.png)

After confirming, the device will be deleted and removed from the list along with this message:

![image](https://user-images.githubusercontent.com/44663302/193007800-e439a493-69a4-40d7-8e61-4ee55a50d910.png)

### Log out

To logout of `Connected Office`, press your username in the right-most, top-most corner of the page (on the top navigation):

![image](https://user-images.githubusercontent.com/44663302/193008105-eb9b3127-637e-4a1b-841f-c78f7f846d16.png)

Click on Sign out. You will be redirected to the logout page. Confirm your logout by pressing the button in the left bottom corner of the page.

![image](https://user-images.githubusercontent.com/44663302/193008276-9a3edd1a-0dc5-45f5-a113-ec706d532a8d.png)

You will be logged out and redirected to the login page.

## Important links

- [Website (Connected Office)](https://con-office-mvc.azurewebsites.net/)
- [References](https://github.com/Nicmanthecodeman/CMPG323-Project3-25102737/blob/main/docs/project3/25102737_CMPG323_Project3_References.pdf)
- [Project](https://github.com/users/Nicmanthecodeman/projects/2)

## References

1.	Bergman, E., 2022. Repository Design Pattern. [online] Medium. Available at: <https://medium.com/@pererikbergman/repository-design-pattern-e28c0f3e4a30> [Accessed 29 September 2022].
2.	C# Corner, 2022. Design Patterns In C# .NET. [online] C-sharpcorner.com. Available at: <https://www.c-sharpcorner.com/UploadFile/bd5be5/design-patterns-in-net/> [Accessed 29 September 2022].
3.	C# Corner, 2022. Using The CQRS Pattern In C#. [online] C-sharpcorner.com. Available at: <https://www.c-sharpcorner.com/article/using-the-cqrs-pattern-in-c-sharp/> [Accessed 29 September 2022].
4.	Convey-stack.github.io. 2022. Convey. [online] Available at: <https://convey-stack.github.io/> [Accessed 22 September 2022].
5.	Cooke, M., 2022. Using .gitignore file to hide appsettings.json does not actually hide it. [online] Stack Overflow. Available at: <https://stackoverflow.com/questions/49930776/using-gitignore-file-to-hide-appsettings-json-does-not-actually-hide-it> [Accessed 28 September 2022].
6.	DigitalOcean, 2022. SOLID: The First 5 Principles of Object Oriented Design | DigitalOcean. [online] Digitalocean.com. Available at: <https://www.digitalocean.com/community/conceptual_articles/s-o-l-i-d-the-first-five-principles-of-object-oriented-design> [Accessed 29 September 2022].
7.	Docs.microsoft.com. 2022. Configure a connection string - Azure Storage. [online] Available at: <https://docs.microsoft.com/en-us/azure/storage/common/storage-configure-connection-string> [Accessed 28 September 2022].
8.	Docs.microsoft.com. 2022. Define your naming convention - Cloud Adoption Framework. [online] Available at: <https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/resource-naming> [Accessed 24 September 2022].
9.	Docs.microsoft.com. 2022. Identity model customization in ASP.NET Core. [online] Available at: <https://docs.microsoft.com/en-us/aspnet/core/security/authentication/customize-identity-model?view=aspnetcore-6.0#customize-the-model> [Accessed 22 September 2022].
10.	Dofactory, 2022. C# Design Patterns -- Tutorial with Examples - Dofactory. [online] Dofactory.com. Available at: <https://www.dofactory.com/net/design-patterns> [Accessed 29 September 2022].
11.	DotNetTutorials, 2022. Unit Of Work in Repository Pattern. [online] Dot Net Tutorials. Available at: <https://dotnettutorials.net/lesson/unit-of-work-csharp-mvc/> [Accessed 29 September 2022].
12.	Erinç, Y., 2022. The SOLID Principles of Object-Oriented Programming Explained in Plain English. [online] freeCodeCamp.org. Available at: <https://www.freecodecamp.org/news/solid-principles-explained-in-plain-english/> [Accessed 29 September 2022].
13.	Evans, E. and Szpoton, R., 2022. Domain-driven design. 1st ed. Gliwice: Helion.
14.	Fowler, M., 2022. P of EAA: Unit of Work. [online] Martinfowler.com. Available at: <https://martinfowler.com/eaaCatalog/unitOfWork.html> [Accessed 29 September 2022].
15.	GitHub. 2022. GitHub - ketan-gote/ddd-example: Domain Driven Design. Examples focuses on key concept of ddd like Entities, Aggregate root, Repository, Value Objects & ACL.. [online] Available at: <https://github.com/ketan-gote/ddd-example> [Accessed 25 September 2022].
16.	Hart, M. and Smith, T., 2022. Where do I find some good examples for DDD?. [online] Stack Overflow. Available at: <https://stackoverflow.com/questions/540130/where-do-i-find-some-good-examples-for-ddd> [Accessed 25 September 2022].
17.	Hieatt, E. and Mee, R., 2022. P of EAA: Repository. [online] Martinfowler.com. Available at: <https://martinfowler.com/eaaCatalog/repository.html> [Accessed 29 September 2022].
18.	Janssen, T., 2022. SOLID Design Principles Explained: The Open/Closed Principle with Code Examples. [online] Stackify. Available at: <https://stackify.com/solid-design-open-closed-principle/> [Accessed 29 September 2022].
19.	Malavasi, A., 2022. Why is the Open-Closed Principle so important?. [online] Medium. Available at: <https://medium.com/@alexandre.malavasi/why-is-the-open-closed-principle-so-important-bed2f2a0d4c7> [Accessed 29 September 2022].
20.	Microsoft, D., 2022. [online] Available at: <https://learn.microsoft.com/en-us/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application> [Accessed 29 September 2022].
21.	Microsoft, D., 2022. ASP.NET MVC Pattern | .NET. [online] Microsoft. Available at: <https://dotnet.microsoft.com/en-us/apps/aspnet/mvc> [Accessed 29 September 2022].
22.	siliconvalve. 2022. Understanding Azure App Service Plans and Pricing. [online] Available at: <https://blog.siliconvalve.com/2016/05/12/understanding-azure-app-service-plans-and-pricing/> [Accessed 27 September 2022].
23.	Stack Overflow. 2022. LINQ Query Syntax (Count Operation). [online] Available at: <https://stackoverflow.com/questions/18879399/linq-query-syntax-count-operation> [Accessed 27 September 2022].
24.	Sundal, F., Jr, J. and Dimitrov, D., 2022. How to get Type of Exception in C#. [online] Stack Overflow. Available at: <https://stackoverflow.com/questions/3715380/how-to-get-type-of-exception-in-c-sharp> [Accessed 26 September 2022].
25.	Tutorials, T., 2022. Learn ASP.NET Core using Step-by-Step Tutorials. [online] Tutorialsteacher.com. Available at: <https://www.tutorialsteacher.com/core> [Accessed 29 September 2022].
26.	Watts, S., 2022. The Importance of SOLID Design Principles. [online] BMC Blogs. Available at: <https://www.bmc.com/blogs/solid-design-principles/> [Accessed 29 September 2022].

### Project 4 (Testing & RPA)

# CMPG323-Project4-25102737

This project/repository showcases the implementation of User Acceptance Testing (UAT) & Robotic Process Automation (RPA) using UI Path.

## Development

UI Path Studio - Community Edition was used to develop the automation software. 

![image](https://user-images.githubusercontent.com/44663302/202717002-ef7772a9-08c5-420d-8c43-63236763c43d.png)

## How to use

1. Clone the repository into a local repository on your computer.
2. Open the the solution using the project.json file (Home -> Open -> Browse to repository -> Click on project.json).
3. Run the solution

## References

1. UiPath Community Forum. (2021). How to click on a specific row in a data 
table. [online] Available at: https://forum.uipath.com/t/how-to-click-on-a-specific-row-in-a-data-table/346101 [Accessed 27 Oct. 2022].
2. “Get Data from Any Row in Excel File.” UiPath Community Forum, 31 July 
2019, forum.uipath.com/t/get-data-from-any-row-in-excel-file/138510. 
Accessed 27 Oct. 2022.
3. “Excel Index to Column - RPA Component.” UiPath Marketplace, 
marketplace.uipath.com/listings/excel-index-to-column. Accessed 27 Oct. 
2022.
4. “Element Exists.” UiPath Activities, docs.uipath.com/activities/docs/element-exists. Accessed 27 Oct. 2022.
5. “Introduction to UiPath Portal, Orchestrator & Studio.” Www.youtube.com, 
www.youtube.com/watch?v=BAYmmUuB2Zo.
6. “Excel Automation.” UiPath StudioX, docs.uipath.com/studiox/docs/excel-automation. Accessed 27 Oct. 2022.
7. “Creating Your First Automation Project.” UiPath StudioX, 
docs.uipath.com/studiox/docs/creating-your-first-automation-project. 
Accessed 27 Oct. 2022.
8. “Read Range.” UiPath Activities, docs.uipath.com/activities/docs/read-range-x. 
Accessed 27 Oct. 2022.
9. “For Each Excel Row.” UiPath Activities, docs.uipath.com/activities/docs/excel-for-each-row. Accessed 27 Oct. 2022.
10. “Write Cell.” UiPath Activities, docs.uipath.com/activities/docs/write-cell-x. 
Accessed 27 Oct. 2022.
11. “About Publishing Automation Projects.” UiPath Studio, 
docs.uipath.com/studio/docs/about-publishing-automation-projects. Accessed 
27 Oct. 2022.
12. “Excel Automation with Studio.” Www.youtube.com, 
www.youtube.com/watch?v=7SrdrREJtcc&feature=emb_imp_woyt. Accessed 
27 Oct. 2022.
13. “UiPath Tutorial for Beginners: What Is UiPath RPA? Features.” 
Www.guru99.com, www.guru99.com/uipath-tutorial.html.
14. “UiPath Tutorial - Javatpoint.” Www.javatpoint.com, 
www.javatpoint.com/uipath.
15. “UIPath RPA Tutorial - Zero to Advanced RPA UIPath Developer.” Udemy, 
www.udemy.com/course/uipath-rpa-tutorial-0-to-advanced-robotic-process-automation-developer/. Accessed 27 Oct. 2022.
16. “UiPath Tutorial for Beginners - What Is UiPath RPA?” Intellipaat Blog, 25 Aug. 
2021, intellipaat.com/blog/uipath-tutorial/. Accessed 27 Oct. 2022.
17. “Manage DataTables.” UiPath Activities, 
docs.uipath.com/activities/docs/manage-data-tables. Accessed 27 Oct. 2022.
18. “Deployment and Configuration Considerations.” UiPath Installation and 
Upgrade, docs.uipath.com/installation-and-upgrade/docs/deployment-and-configuration-considerations. Accessed 27 Oct. 2022.
19. “How to Extract Particular Data from Array.” UiPath Community Forum, 27 
Oct. 2022, forum.uipath.com/t/how-to-extract-particular-data-from-array/485042. Accessed 27 Oct. 2022.
20. “Select a Specific Row in a Web Page Table.” UiPath Community Forum, 
forum.uipath.com/t/select-a-specific-row-in-a-web-page-table/396846/3. 
Accessed 27 Oct. 2022.
21. “Tutorial: Extracting Table Data from a Web Page and Editing It in Excel.” 
UiPath StudioX, docs.uipath.com/studiox/docs/tutorial-extracting-table-data-from-a-web-page-and-editing-it-in-excel. Accessed 27 Oct. 2022.
22. “Find Position of a Cell.” UiPath Community Forum, 26 May 2020, 
forum.uipath.com/t/find-position-of-a-cell/226185/2. Accessed 27 Oct. 2022.
23. “How to Read Specific Cell from Excel.” UiPath Community Forum, 21 June 
2018, forum.uipath.com/t/how-to-read-specific-cell-from-excel/37261/2. 
Accessed 27 Oct. 2022.
24. “Need to Select Checkbox Based on a Condition.” UiPath Community Forum, 
21 June 2018, forum.uipath.com/t/need-to-select-checkbox-based-on-a-condition/24446/5. Accessed 27 Oct. 2022.
25. “Read Data from Excel.” UiPath Community Forum, 4 Mar. 2020, 
forum.uipath.com/t/read-data-from-excel/199633. Accessed 27 Oct. 2022.
26. “How to Read Excel File with Dynamic Rows?” UiPath Community Forum, 14 
Apr. 2021, forum.uipath.com/t/how-to-read-excel-file-with-dynamic-rows/306817. Accessed 27 Oct. 2022

### Project 5 (Reporting & Monitoring)

# Connected Office - Device Monitoring

## Summary

This project (repository) uses PowerBI Service (report hosted online) and PowerBI Desktop (for development).

## Structure

The report consists of three (3) pages, namely:

1. High-Level Metrics
2. Device Monitoring
3. Device Registration

## Contents

### High-Level Metrics

A summary view that shows business stakeholders a high-level view of the ‘important’ data. This generally would refer to counts of data points based on information 
that could impact decision making.

![image](https://user-images.githubusercontent.com/44663302/201112554-f2a88bb1-f9ba-46c8-9b14-fe13b748828a.png)

### Device Monitoring

Visuals regarding devices per category, zone and each device's status.

![image](https://user-images.githubusercontent.com/44663302/201112834-53e5ee47-2b66-4df7-9aac-587e04e0b84b.png)

### Device Registration

Visuals representing the devices' registration (installed) dates with a custom date slicer (filter).

![image](https://user-images.githubusercontent.com/44663302/201113042-8311227b-9925-40b1-b100-b6b4d62468d5.png)

## How to use the report

The report is hosted online (PowerBI Service) and can be accessed by clicking [here](https://app.powerbi.com/links/8VjKagN3We?ctid=331c86e7-d032-436f-bc53-f2552d031012&pbi_source=linkShare).

Alternatively, you can download the PowerBI Report file from the repository (located [here](https://github.com/Nicmanthecodeman/CMPG323-Project5-25102737/blob/dev/src/Connected%20Office%20-%20Device%20Monitoring.pbix)) and open the report in PowerBI Desktop.
Make sure you can connect to the data source (Sharepoint) from NWU Sharepoint (msfed account).

## References

1. Anon, (2022). All About Power BI LIVE Connection Simplified 101 - Learn | 
Hevo. [online] Available at: https://hevodata.com/learn/power-bi-live-connect/.
2. BI, V. (2018). Achieving Global Filters Across The Report in Power BI. [online] 
Visual BI Solutions. Available at: 
https://visualbi.com/blogs/microsoft/powerbi/achieving-global-filters-across-report-power-bi/ [Accessed 10 Nov. 2022].
3. Castor, R. (2020). Power BI: Calculated Measures vs. Calculated Columns. 
[online] Medium. Available at: https://towardsdatascience.com/power-bi-calculated-measures-vs-calculated-columns-9be012e9bff1.
4. community.powerbi.com. (2017). Global Filter for all reports. [online] Available 
at: https://community.powerbi.com/t5/Desktop/Global-Filter-for-all-reports/m-p/224839 [Accessed 10 Nov. 2022].
5. community.powerbi.com. (2017). Send to Back. [online] Available at: 
https://community.powerbi.com/t5/Desktop/Send-to-Back/m-p/327786 
[Accessed 10 Nov. 2022].
6. community.powerbi.com. (2018). Calculating Days Between a Date and Today. 
[online] Available at: 
https://community.powerbi.com/t5/Desktop/Calculating-Days-Between-a-Date-and-Today/m-p/389696 [Accessed 10 Nov. 2022].
7. community.powerbi.com. (2019). Nowalls Analytics. [online] Available at: 
https://community.powerbi.com/t5/Themes-Gallery/Nowalls-Analytics/td-p/772564 [Accessed 10 Nov. 2022].
8. dash-intel.com. (n.d.). Measures and Calculated Columns | Dash-Intel. [online] 
Available at: https://dash-intel.com/powerbi/modeling_measure+calculated+columns.php.
9. DataFlair. (2018). Power BI Slicers - A Complete Tutorial to learn from 
Scratch! [online] Available at: https://data-flair.training/blogs/power-bi-slicer/.
10. davidiseminger (n.d.). Create and manage relationships in Power BI Desktop -
Power BI. [online] learn.microsoft.com. Available at: 
https://learn.microsoft.com/en-us/power-bi/transform-model/desktop-create-and-manage-relationships.
11. davidiseminger (n.d.). Get Power BI Desktop - Power BI. [online] 
learn.microsoft.com. Available at: https://learn.microsoft.com/en-us/power-bi/fundamentals/desktop-get-the-desktop.
12. davidiseminger (n.d.). Publish from Power BI Desktop - Power BI. [online] 
learn.microsoft.com. Available at: https://learn.microsoft.com/en-us/power-bi/create-reports/desktop-upload-desktop-files [Accessed 10 Nov. 2022].
13. davidiseminger (n.d.). Tutorial: Create calculated columns in Power BI Desktop - Power BI. [online] learn.microsoft.com. Available at: 
https://learn.microsoft.com/en-us/power-bi/transform-model/desktop-tutorial-create-calculated-columns.
14. davidiseminger (n.d.). Tutorial: Create calculated columns in Power BI Desktop - Power BI. [online] learn.microsoft.com. Available at: 
https://learn.microsoft.com/en-us/power-bi/transform-model/desktop-tutorial-create-calculated-columns.
15. KesemSharabi (n.d.). Deployment pipelines, the Power BI Application lifecycle 
management (ALM) tool, process - Power BI. [online] learn.microsoft.com. 
Available at: https://learn.microsoft.com/en-us/power-bi/create-reports/deployment-pipelines-process [Accessed 10 Nov. 2022].
16. maggiesMSFT (n.d.). Add a filter to a report in Power BI - Power BI. [online] 
learn.microsoft.com. Available at: https://learn.microsoft.com/en-us/power-bi/create-reports/power-bi-report-add-filter?tabs=powerbi-desktop.
17. maggiesMSFT (n.d.). Slicers in Power BI - Power BI. [online] 
learn.microsoft.com. Available at: https://learn.microsoft.com/en-us/power-bi/visuals/power-bi-visualization-slicers?tabs=powerbi-desktop.
18. mihart (n.d.). Tutorial: Get started creating in the Power BI service - Power BI. 
[online] learn.microsoft.com. Available at: https://learn.microsoft.com/en-us/power-bi/fundamentals/service-get-started [Accessed 10 Nov. 2022].
19. Neware, A. (2022). How to Show Last Refresh Date in Power BI. [online] 
Perficient Blogs. Available at: https://blogs.perficient.com/2022/08/24/how-to-show-last-refresh-date-in-power-bi/ [Accessed 10 Nov. 2022].
20. powerbi.microsoft.com. (n.d.). Connecting to datasets in the Power BI service 
from Desktop | Blog di Microsoft Power BI | Microsoft Power BI. [online] 
Available at: https://powerbi.microsoft.com/it-ch/blog/connecting-to-datasets-in-the-power-bi-service-from-desktop/ [Accessed 10 Nov. 2022].
21. powerbi.microsoft.com. (n.d.). Visual Awesomeness Unlocked: The Timeline 
Slicer | Microsoft Power BI Blog | Microsoft Power BI. [online] Available at: 
https://powerbi.microsoft.com/fr-fr/blog/visual-awesomeness-unlocked-the-timeline-slicer/ [Accessed 10 Nov. 2022].
22. Rad, R. (2022). Live Connection; When Power BI comes Hybrid. [online] 
RADACAD. Available at: https://radacad.com/live-connection-when-power-bi-comes-hybrid [Accessed 10 Nov. 2022].
23. www.pbiusergroup.com. (n.d.). Power Bi report not opening | Power BI 
Exchange. [online] Available at: 
https://www.pbiusergroup.com/communities/community-home/digestviewer/viewthread?MessageKey=002f24ad-b65a-4d19-bde1-
92e42b118cfd&CommunityKey=b35c8468-2fd8-4e1a-8429-
322c39fe7110&tab=digestviewer#:~:text=RE%3A%20Power%20Bi%20report%
20not%20opening&text=company%20utilize%20OneDrive%3F- [Accessed 10 
Nov. 2022].
24. www.tutorialspoint.com. (n.d.). Power BI - Supported Data Sources. [online] 
Available at: 
https://www.tutorialspoint.com/power_bi/power_bi_supported_data_source
s.htm [Accessed 10 Nov. 2022].
25. www.youtube.com. (n.d.). How to use Microsoft Power BI - Tutorial for 
Beginners. [online] Available at: 
https://www.youtube.com/watch?v=TmhQCQr_DCA.
26. www.youtube.com. (n.d.). Using custom visuals - Timeline Slicer. [online] 
Available at: https://www.youtube.com/watch?v=ozMtZ4_NZ10 [Accessed 10 
Nov. 2022]
