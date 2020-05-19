

# BargeEx  Control Panel

**Database Connection**

Usually for all web based .NET applications  the database connectivity will be specified in **`Web.config`** file, It is a good practice to store the connection string for your application in a Config file rather than a hard coded string in your code.

    <connectionStrings>   
    		<add connectionString="Server=ServerName/IP;Database=DatabaseName;Trusted_Connection=True" name="Test" providerName="System.Data.SqlClient" />
    </connectionStrings>

We can have multiple database connection strings with a different connection name key. But not only connection strings other application configuration is also handled in *`Web.Config`* file.

## Tables Interactions

**Entity Framework**

“EF is an open-source ORM framework for .NET applications supported by Microsoft. It eliminates the need for most of the data-access code that developers usually need to write”

In BargeEx, EF is used for database interaction. The complete detail of tables will be available in entity class files and .dbml file.  Each table contains separate entity class files it will take care of the data handling part of that particular table.

**Repository Pattern**

The Repository Design Pattern in C# Mediates between the domain and the data mapping layers using the collection-like interface for accessing the domain objects. It holds all the CURD operation logics in it.
BusinessLogic.Entites contains all the entity files and BusinessContext class will take-care the data creation/ updation & deletion process using corresponding repository file. It holds the data transaction process as well.

Ex:
If we work on TradingPartner UI on new trading partner creation or updation on existing trading partner it will call the TradingPartnerRepository  for business logic to do the corresponding operations and save the data in Trading partner table.    

**Project Dependencies**
Below are external DLL's referred in BargeEx Control Panel. There are 2 ways to add project references.

 1. Directly refer the DLL in project
 2. Install from Package Manager --> Nuget package 
```
 - CodeSmith.Data.dll
 - CommonServiceLocator.NinjectAdapter.dll
 - DocumentFormat.OpenXml.dll
 - DocumentFormat.OpenXml.Extensions.dll
 - Ninject.dll
 - Ninject.MVC3.dll
 - WebActivator.dll
```
**User Creation:**

When we create a new user, edit or delete the existing user’s the following tables will be affected in database.
```
- dbo.User 
- dbo.aspnet_Users
- dbo.aspnet_Membership
```
## API Interactions:

***Document Sent:***
Document Sent page is used to view/ verify the documents which are sent by the trading partner. It will list all the documents which have been sent by the trading partner with different search criteria’s. Below are the file interactions.
```
~\Views\DocumentSent\List.cshtml
~\Views\DocumentSent\ListParial.cshtml
~\Models\ViewModels\DocumentsViewModel.cs
~\Controllers\DocumentSentController.cs
~\Contollers\BaseController\DocumentBaseController.cs
BusinessLogic\Entites\Document.cs
BusinessLogic\Entites\Document.generated.cs
BusinessLogic\Entites\Interface\IDocument.cs
BusinessLogic\Repositories\DocumentRepository.cs
BusinessLogic\Repositories\IRepository.cs
Other document related repositories & extension class files
```

***Document Received:***
It is used to view/ verify the documents which are received by the trading partner. It will list all the documents which have been sent by the other trading partners to the selected trading partner. Below are the file interactions.
```
~\Views\DocumentReceived\List.cshtml
~\Views\DocumentReceived\ListParial.cshtml
~\Models\ViewModels\DocumentsViewModel.cs
~\Controllers\DocumentReceivedController.cs
~\Contollers\BaseController\DocumentBaseController.cs
BusinessLogic\Entites\Document.cs
BusinessLogic\Entites\Document.generated.cs
BusinessLogic\Entites\Interface\IDocument.cs
BusinessLogic\Repositories\DocumentRepository.cs
BusinessLogic\Repositories\IRepository.cs
Other document related repositories & extension class files
```
###### Tables
```
 - dbo.Document
 - dbo.DocumentApplicationHeader
 - dbo.DocumentQueue
```
## BargeExAgent
**Web Services [SOAP Call]**

***SendDocument:***
SendDocument is  a web method available in the BargeExAgent web service project, it sends documents to a trading partners. It can either consumed (referred) in different project or directly send request by using SOAP UI.
###### Input param:
```
 - UserName
 - Password
 - TradingPartnerNumber
 - Document
```
### SOAP 1.1

The following is a sample SOAP 1.1 request and response. The placeholders shown need to be replaced with actual values.
```
	POST /BargeExAgent/BargeExService.asmx HTTP/1.1 Host: 
	localhost Content-Type: text/xml; charset=utf-8 Content-
	Length: length SOAPAction: 
	"http://www.BargeEx.com/BargeExService/SendDocument" <?xml 
	version="1.0" encoding="utf-8"?> <soap:Envelope 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:xsd="http://www.w3.org/2001/XMLSchema" 
	xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/"> 
	<soap:Header> <TradingPartnerCredentials 
	xmlns="http://www.BargeEx.com/BargeExService"> 
	<UserName>string</UserName> <Password>string</Password> 
	<TradingPartnerNumber>string</TradingPartnerNumber> 
	</TradingPartnerCredentials> </soap:Header> <soap:Body> 
	<SendDocument xmlns="http://www.BargeEx.com/BargeExService"> 
	<Document>xml</Document> </SendDocument> </soap:Body> 
	</soap:Envelope>
```
```
	HTTP/1.1 200 OK Content-Type: text/xml; charset=utf-8 
	Content-Length: length <?xml version="1.0" encoding="utf-8"?> 
	<soap:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-
	instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" 
	xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/"> 
	<soap:Body> <SendDocumentResponse 
	xmlns="http://www.BargeEx.com/BargeExService"> 
	<SendDocumentResult>UnknownError or Success or 
	FailedAuthentication or UnauthorizedTradingPair or 
	FailedSchemaValidation or UnsupportedNamespace or 
	DuplicateInstanceIdentifier</SendDocumentResult> 
	<ErrorDetails>string</ErrorDetails> </SendDocumentResponse> 
	</soap:Body> </soap:Envelope>
```
 ***GetNextDocument:***
GetNextDocument is a web method available in the BargeExAgent web service project, it retrieves the oldest document that are ready for pickup or returns *`NoMoreDocuments`* if no documents are ready for pickup. It can either consumed (referred) in different project or directly send request by using SOAP UI.
###### Input param:
```
 - UserName
 - Password
 - TradingPartnerNumber
```
```
	POST /BargeExAgent/BargeExService.asmx HTTP/1.1 Host: 
	localhost Content-Type: text/xml; charset=utf-8 Content-
	Length: length SOAPAction: 
	"http://www.BargeEx.com/BargeExService/GetNextDocument"
	<?xml version="1.0" encoding="utf-8"?> <soap:Envelope 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:xsd="http://www.w3.org/2001/XMLSchema" 
	xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/"> 
	<soap:Header> <TradingPartnerCredentials 
	xmlns="http://www.BargeEx.com/BargeExService"> 
	<UserName>string</UserName> <Password>string</Password> 
	<TradingPartnerNumber>string</TradingPartnerNumber> 
	</TradingPartnerCredentials> </soap:Header> <soap:Body> 
	<GetNextDocument 
	xmlns="http://www.BargeEx.com/BargeExService" /> 
	</soap:Body> 
	</soap:Envelope>
```
```
	HTTP/1.1 200 OK Content-Type: text/xml; charset=utf-8 
	Content-Length: length <?xml version="1.0" encoding="utf-8"?>
	<soap:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-
	instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema"
	xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
	<soap:Body> <GetNextDocumentResponse
	xmlns="http://www.BargeEx.com/BargeExService">
	<GetNextDocumentResult>UnknownError or Success or
	FailedAuthentication or 
	NoMoreDocuments</GetNextDocumentResult> 
	<Document>xml</Document> </GetNextDocumentResponse> 
	</soap:Body> </soap:Envelope>
```
**Code files**
```
BargeExService.asmx
App_Code\BargeExService.vb
App_Code\BargeExWebServiceHelper.vb
```
## Build and Deployment process

Build process is used to compile the solution/ projects if any changes made in Model, View and Controllers. Build/ Rebuild will be compile entire solution or individual project, if we build the solution it will build each projects in order wise and if we build individual project it will first build it's dependencies and build the main project. Once the build was successful then we need to publish the project with the release mode. 

[More details on build and deployment](https://docs.microsoft.com/en-us/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process)

Different options to publish the projects are
-	File System
-	FTP
-	Web Deploy
-	Web Deploy Package

