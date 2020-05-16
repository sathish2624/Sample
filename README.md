

# BargeEx  Control Panel

**Database Connection**

Usually for all web based .NET applications  the database connectivity will be specified in **Web.config** file, It is a good practice to store the connection string for your application in a Config file rather than a hard coded string in your code.

    <connectionStrings>   
    		<add connectionString="Server=ServerName/IP;Database=DatabaseName;Trusted_Connection=True" name="Test" providerName="System.Data.SqlClient" />
    </connectionStrings>

We can have multiple database connection strings with a different connection name key. But not only connection strings other application configuration is also handled in *Web.Config* file.

## Tables Interactions

**Entity Framework**

“EF is an open-source ORM framework for .NET applications supported by Microsoft. It eliminates the need for most of the data-access code that developers usually need to write”

In BargeEx, EF is used for database interaction. The complete detail of tables will be available in entity class files and .dbml file.  Each table contains separate entity class files it will take care of the data handling part of that particular table.

**Repository Pattern**

The Repository Design Pattern in C# Mediates between the domain and the data mapping layers using the collection-like interface for accessing the domain objects. It holds all the CURD operation logics in it.
BusinessLogic.Entites contains all the entity files and BusinessContext class will take-care the data creation/ updation & deletion process using corresponding repository file. It holds the data transaction process as well.

Ex:
If we work on TradingPartner UI on new trading partner creation or updation on existing trading partner it will call the TradingPartnerRepository  for business logic to do the corresponding operations and save the data in Trading partner table.    

**User Creation:**

When we create a new user, edit or delete the existing user’s the following tables will be affected in database.
-	dbo.User 
-	dbo.aspnet_Users
-	dbo.aspnet_Membership

**API Interactions:**

***Document Sent:***
Document Sent page is used to view/ verify the documents which are sent by the trading partner. It will list all the documents which have been sent by the trading partner with different search criteria’s. Below are the file interactions.

*~\Views\DocumentSent\List.cshtml* 

*~\Views\DocumentSent\ListParial.cshtml*

*~\Models\ViewModels\DocumentsViewModel.cs*

*~\Controllers\DocumentSentController.cs*

*~\Contollers\BaseController\DocumentBaseController.cs*

*BusinessLogic\Entites\Document.cs*

*BusinessLogic\Entites\Document.generated.cs*

*BusinessLogic\Entites\Interface\IDocument.cs*

*BusinessLogic\Repositories\DocumentRepository.cs*

*BusinessLogic\Repositories\IRepository.cs*

*Other document related repositories & extension class files*

***Document Received:***
It is used to view/ verify the documents which are received by the trading partner. It will list all the documents which have been sent by the other trading partners to the selected trading partner. Below are the file interactions.

*~\Views\DocumentReceived\List.cshtml* 

*~\Views\DocumentReceived\ListParial.cshtml*

*~\Models\ViewModels\DocumentsViewModel.cs*

*~\Controllers\DocumentReceivedController.cs*

*~\Contollers\BaseController\DocumentBaseController.cs*

*BusinessLogic\Entites\Document.cs*

*BusinessLogic\Entites\Document.generated.cs*

*BusinessLogic\Entites\Interface\IDocument.cs*

*BusinessLogic\Repositories\DocumentRepository.cs*

*BusinessLogic\Repositories\IRepository.cs*

*Other document related repositories & extension class files*

**Build and Deployment process**

Build process is used to compile the solution/ projects if any changes made in Model, View and Controllers. Once the build was successful then we need to publish the project with Release mode. 

Different options to publish the projects are
-	File System
-	FTP
-	Web Deploy
-	Web Deploy Package
