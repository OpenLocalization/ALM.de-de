<link rel="stylesheet" type="text/css" href="https://www.microsoft.com/resources/msdn/en-us/office/media/O365APIstart/O365Build2015.css" />



# Outlook Mail REST API reference

 _**Applies to:** Exchange Online | Office 365_

<!--CAUTION
This topic is now linked to from the Connected Services wizard in Visual Studio 2015. Please don't repurpose or delete this topic without notifying the Visual Studio product team. Thanks.
-->

The Outlook Mail API lets you read, create, and send messages and attachments, view and respond to event messages, and manage folders in a user's mailbox in Office 365 or Exchange Online.

The API relies on Microsoft Azure Active Directory and OAuth to [authenticate application requests](https://msdn.microsoft.com/office/office365/HowTo/common-app-authentication-tasks).
You can access the Mail API by calling the corresponding REST APIs directly in your apps, or by using the Office 365 client libraries and SDKs.

To access the Mail REST API from your application, you'll need to explicitly register it in Azure AD with [permissions at the appropriate scope](https://msdn.microsoft.com/office/office365/HowTo/application-manifest#AppManifest_ExchangeScopes).
You also need to manage authentication tokens, and construct the correct URL and queries for your needs.


As an alternative, the Office 365 client libraries and SDKs make it easier to interact with the Mail API:
[.NET](https://msdn.microsoft.com/office/office365/HowTo/getting-started-Office-365-APIs), [JavaScript](https://msdn.microsoft.com/office/office365/HowTo/getting-started-Office-365-APIs),
 [Android](https://msdn.microsoft.com/office/office365/HowTo/getting-started-Office-365-APIs), and [iOS](https://msdn.microsoft.com/office/office365/HowTo/getting-started-Office-365-APIs). They help manage authentication tokens, simplify the code needed to query and consume data, and perform discovery.
 To learn more, see [Office 365 APIs platform overview](https://msdn.microsoft.com/office/office365/HowTo/platform-development-overview).  

**Note** If you develop apps for Office 365 in China, see [API endpoints of Office 365 for China](https://msdn.microsoft.com/office/office365/APi/o365-china-endpoints) for the specification of the required URLs.

## All Mail API operations

**Message operations**
Messages are stored in mailbox folders, so message endpoints often include the folder that contains the message.
 A folder is either specified by ID or by one of the following well-known folder names: `Inbox`, `Drafts`, `SentItems`, or `DeletedItems`.

[Get messages](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetMessages) | [Create and send messages](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SendMessages) | [Reply or reply all to messages](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#ReplyToMessages) |
[Forward new or drafted messages](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#ForwardMessages) | [Update messages](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#UpdateMessages)

[Delete messages](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#DeleteMessages) | [Move or copy messages](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#MoveCopyMessages) | [Get attachments](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetAttachments) |
[Create attachments](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#CreateAttachments) | [Delete attachments](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#DeleteAttachments)


**Folder operations**
Mailbox folders can contain messages and other folders. You can get, create, change, delete, and manage folders.
 You can use the following well-known folder names instead of the ID to specify the corresponding folder: `Inbox`, `SentItems`, `Drafts`, or `DeletedItems`.

[Get folders](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetFolders) | [Create folders](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#CreateFolders) | [Update folders](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#UpdateFolders) | [Delete folders](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#DeleteFolders) | [Move or copy folders](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#MoveCopyFolders)

See also:

[REST API message resource](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#MessageResource) | [REST API Folder resource](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#FolderResource)

<a name="Overview"> </a>
## Use the Mail REST API

To interact with the Mail REST API, you send HTTP requests that use a supported method: GET, POST, PATCH, or DELETE.

All Mail REST API requests use the following root URL format:

 `https://outlook.office365.com/api/{version}/{user_context}`

<a name="SupportedVersions"> </a>

`{version}` represents the version of the REST API in the specified root URL. Specify one of the following values: 

- `v1.0`. The REST API in this version is in General Availability status, and can be used in production code. For example, `http://outlook.office365.com/api/v1.0/me/messages`. 

- `beta`. The REST API in this version is in preview status, and should not be used in production code yet. For example, `http://outlook.office365.com/api/beta/me/messages`.

The majority of the REST operations in the Mail API is supported and behaves the same way as described across the above 
listed versions. Operations that are added or behave differently in a later version are specifically called out.

`{user_context}` is the currently signed-in user, as Mail API requests are always performed on behalf of the current user. You can specify the user context in one of two ways:

- With the `me` shortcut: `/api/{version}/me`. The root URL becomes `https://outlook.office365.com/api/{version}/me`.

- With the `users/{upn}` format to pass the UPN or a proxy address of the signed-in user, such as: `/api/v1.0/users/sadie@contoso.com`. In this example, the root URL would become `https://outlook.office365.com/api/v1.0/users/sadie@contoso.com`.

Usage is a matter of preference. In server responses, using the same example, the user context is identified in this format: `/api/v1.0/users('sadie@contoso.com')`

You send requests to endpoints that you want to act on. For example, to get the user's Inbox messages in Office 365, send a GET request to the following URL:

 `https://outlook.office365.com/api/v1.0/me/messages`

POST and PATCH request bodies and server responses are sent in JSON payloads.

The Mail API supports two ways of specifying an ID or key in an entity collection. They are evaluated identically, so usage is a matter of preference.

- In the URL segment after the collection, for example:  `/api/v1.0/me/messages/AAMkAGI3...`

- In parentheses as a quoted string, for example:  `/api/v1.0/me/messages('AAMkAGI3...')`

The API supports query parameters to filter messages and folders in the user's mailbox.
The path URL resource names, query parameters, and well-known folder names are case insensitive. However, values you assign, entity IDs, and other base64 encoded values are case sensitive.


## Use a client library to access the Mail API

The Office 365 API client libraries and SDKs make it easier to interact with the REST APIs.
You can get the .NET and Cordova JavaScript client libraries in the latest version of [Office 365 API Tools for Visual Studio](http://aka.ms/clientlibrary).

To access the Mail API by using the .NET or JavaScript client library, you need to acquire an access token and get the Outlook Services client.
 Then, you can send async queries to interact with mail data.

[Get an access token](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetAuthToken) | [Get the Outlook Services client](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetClient)


<a name="GetAuthToken"> </a>
### Get an access token

Acquire the access token used for authentication. The client ID and authorization URI are assigned when you register your app with Microsoft Azure Active Directory.

**Note** This pattern applies only to code that runs on Windows devices. In a Windows Store App, values for ClientID and AuthorizationUri are added to your project's App.xaml file when you register your application.
 AuthorizationUri is used as the host name for the CommonAuthority variable.

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
// Properties of the native client app. Get the ClientId from the resources section of the App.xaml file.
public const string ClientID = Application.Current.Resources["ida:ClientID"] as String;
// Get the _returnUri from app settings.
public static Uri _returnUri = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();

// Properties used to communicate with a Windows Azure AD tenant.  
public const string CommonAuthority = "https://login.windows.net/Common";
public const string DiscoveryResourceId = "https://api.office.com/discovery/";

//Store authority in application data so that it isn't tied to the lifetime of the access token.
private static ApplicationDataContainer _settings = ApplicationData.Current.LocalSettings;
private static string LastAuthority
{
    get
    {
        if (_settings.Values.ContainsKey("LastAuthority") && _settings.Values["LastAuthority"] != null)
        {
            return _settings.Values["LastAuthority"].ToString();
        }
        else
        {
            return string.Empty;
        }

    }

    set
    {
        _settings.Values["LastAuthority"] = value;
    }
}

public static AuthenticationContext _authenticationContext { get; set; }
private static async Task<string> GetTokenHelperAsync(AuthenticationContext context, string resourceId)
{
    string accessToken = null;
    AuthenticationResult result = null;

    result = await context.AcquireTokenAsync(resourceId, ClientID, _returnUri);

    accessToken = result.AccessToken;
    //Store authority in application data.
    _settings.Values["LastAuthority"] = context.Authority;

    return accessToken;
}
```
```javascript
var authContext;
var authToken; // for use with creating an outlookClient later.
authContext = new O365Auth.Context();
authContext.getIdToken("https://outlook.office365.com/")
   .then((function (token) {
       authToken = token;
	   // The auth token also carries additional information. For example:
       userName = token.givenName + " " + token.familyName;
   }).bind(this), function (reason) {
       console.log('Failed to login. Error = ' + reason.message);
   });

```
<!-- ENDSECTION -->

<a name="GetClient"> </a>
### Get the Outlook Services client


Get the **OutlookServicesClient** object. You can call this code from other methods that use the Outlook Services client.

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
public static async Task<OutlookServicesClient> CreateOutlookClientAsync(string capability)
{
    try
    {
        //First, look for the authority used during the last authentication.
        //If that value is not populated, use CommonAuthority.
        string authority = null;
        if (String.IsNullOrEmpty(LastAuthority))
        {
            authority = CommonAuthority;
        }
        else
        {
            authority = LastAuthority;
        }
        // Create an AuthenticationContext using this authority.
        _authenticationContext = new AuthenticationContext(authority);

        //See the Discovery Service Sample (https://github.com/OfficeDev/Office365-Discovery-Service-Sample)
        //for an approach that improves performance by storing the discovery service information in a cache.
        DiscoveryClient discoveryClient = new DiscoveryClient(
            async () => await GetTokenHelperAsync(_authenticationContext, DiscoveryResourceId));

        // Get the specified capability ("Mail").
        CapabilityDiscoveryResult result =
            await discoveryClient.DiscoverCapabilityAsync(capability);
        var client = new OutlookServicesClient(
            result.ServiceEndpointUri,
            async () =>
                await GetTokenHelperAsync(_authenticationContext, result.ServiceResourceId));
        return client;
    }
    catch (Exception)
    {
        if (_authenticationContext != null && _authenticationContext.TokenCache != null)
        _authenticationContext.TokenCache.Clear();
        return null;
    }
}
```
```javascript
// Once the authToken has been acquired, create an outlookClient. One place to do this is inside of the
//    ".then" function callback of authContext.getIdToken(...) above.
var outlookClient = new Microsoft.OutlookServices.Client('https://outlook.office365.com/api/v1.0', authToken.getAccessTokenFn('https://outlook.office365.com'));
```
<!-- ENDSECTION -->




****


<a name="GetMessages"> </a>
##Get messages

You can get a message collection or an individual message from a mailbox folder.

REST API: [Get a message collection (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetMessageCollection) | [Get a message (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetMessage)

Client libraries: [Get a message collection (Client)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetMessagesClient) | [Get a message (Client)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetMessageClient)

<a name="GetMessageCollection"> </a>
###Get a message collection (REST)

_Required scope: **Mail.Read** or **Mail.Write**_

Get a message collection. The collection depends on the [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API specified:
- If {version} is `v1.0`, get the message collection from the Inbox.
- If {version} is `beta`, get the message collection from the entire mailbox of the signed-in user.

```no-highlight
GET https://outlook.office365.com/api/{version}/me/messages
```

You can also specify a folder in the user's mailbox and get the message collection from that folder.

```no-highlight
GET https://outlook.office365.com/api/{version}/me/folders/{folder_id}/messages
```


|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.| 
|folder_id|string|The folder ID, or the `Inbox`, `Drafts`, `SentItems`, or `DeletedItems` well-known folder name, if you're getting messages from a specific folder.|

**Note** By default, each message in the response includes all its properties. Use **$select** to specify only those properties you need for
best performance. The **Id** property is always returned.
See [OData query parameters](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#OdataQueryParams) for filtering, sorting, and paging parameters.

The following example shows how to use **$select** to specify returning only the **Sender** and **Subject** properties of each message in the
response. Refer to the sample response in [Get a message (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetMessage) for a full list of properties that would be returned for a message if you don't use **$select**.

```REST-i
{
    "method": "GET",
    "resourceFormat": "https://outlook.office365.com/api/v1.0/me/folders/{folder_id}/messages/?$select=Sender,Subject",
    "requestUrl": "https://outlook.office365.com/api/v1.0/me/folders/sentitems/messages/?$select=Sender,Subject",
    "requestHeaders": {
        "Accept": "application/json"
    },
    "parameterDetails": [
        {
            "name": "folder_id",
            "type": "string",
            "description": "The target folder ID or well-known name: Inbox, SentItems, Drafts, or DeletedItems.",
            "defaultValue": "sentitems"
        }
    ],
    "statusCode": 200,
    "responseBody": {
        "@odata.context": "https://outlook.office365.com/api/v1.0/$metadata#Me/Folders('sentitems')/Messages",
        "value": [
            {
                "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Messages('AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEJAACd9nJ-tVysQos2hTfspaWRAAADTIzAAAA=')",
                "@odata.etag": "W/\"CQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTJqS\"",
                "Id": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEJAACd9nJ-tVysQos2hTfspaWRAAADTIzAAAA=",
                "ChangeKey": "CQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTJqS",
                "Categories": [],
                "DateTimeCreated": "2014-10-20T00:28:24Z",
                "DateTimeLastModified": "2014-10-20T00:28:26Z",
                "Subject": "Meeting Notes",
                "BodyPreview": "Please send me the meeting notes ASAP",
                "Body": {
                    "ContentType": "Text",
                    "Content": "Please send me the meeting notes ASAP"
                },
                "Importance": "Normal",
                "HasAttachments": false,
                "ParentFolderId": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEJAAA=",
                "From": {
                    "EmailAddress": {
                        "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
                        "Name": "Alex D"
                    }
                },
                "Sender": {
                    "EmailAddress": {
                        "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
                        "Name": "Alex D"
                    }
                },
                "ToRecipients": [
                    {
                        "EmailAddress": {
                            "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com",
                            "Name": "Katie Jordan"
                        }
                    }
                ],
                "CcRecipients": [],
                "BccRecipients": [],
                "ReplyTo": [],
                "ConversationId": "AAQkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAQAEmjRj3pwjRLrbymGMYyEto=",
                "DateTimeReceived": "2014-10-20T00:28:24Z",
                "DateTimeSent": "2014-10-20T00:28:24Z",
                "IsDeliveryReceiptRequested": false,
                "IsReadReceiptRequested": false,
                "IsDraft": false,
                "IsRead": true
            },
            {
                "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Messages('AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEJAACd9nJ-tVysQos2hTfspaWRAAADTIy-AAA=')",
                "@odata.etag": "W/\"CQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTJqP\"",
                "Id": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEJAACd9nJ-tVysQos2hTfspaWRAAADTIy-AAA=",
                "ChangeKey": "CQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTJqP",
                "Categories": [],
                "DateTimeCreated": "2014-10-20T00:13:21Z",
                "DateTimeLastModified": "2014-10-20T00:13:23Z",
                "Subject": "Contract Signing",
                "BodyPreview": "There will be a detailed legal review of Project Falcon once the contract is ready.",
                "Body": {
                    "ContentType": "Text",
                    "Content": "There will be a detailed legal review of Project Falcon once the contract is ready."
                },
                "Importance": "Normal",
                "HasAttachments": false,
                "ParentFolderId": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEJAAA=",
                "From": {
                    "EmailAddress": {
                        "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
                        "Name": "Alex D"
                    }
                },
                "Sender": {
                    "EmailAddress": {
                        "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
                        "Name": "Alex D"
                    }
                },
                "ToRecipients": [
                    {
                        "EmailAddress": {
                            "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com",
                            "Name": "Katie Jordan"
                        }
                    },
                    {
                        "EmailAddress": {
                            "Address": "garthf@a830edad9050849NDA1.onmicrosoft.com",
                            "Name": "Garth Fort"
                        }
                    }
                ],
                "CcRecipients": [],
                "BccRecipients": [],
                "ReplyTo": [],
                "ConversationId": "AAQkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAQAKjRc0YJSUBJpofjWgitxag=",
                "DateTimeReceived": "2014-10-20T00:13:21Z",
                "DateTimeSent": "2014-10-20T00:13:21Z",
                "IsDeliveryReceiptRequested": false,
                "IsReadReceiptRequested": false,
                "IsDraft": false,
                "IsRead": true
            },
            {
                "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Messages('AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEJAACd9nJ-tVysQos2hTfspaWRAAADTIy9AAA=')",
                "@odata.etag": "W/\"CwAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTJqJ\"",
                "Id": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEJAACd9nJ-tVysQos2hTfspaWRAAADTIy9AAA=",
                "ChangeKey": "CwAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTJqJ",
                "Categories": [],
                "DateTimeCreated": "2014-10-19T23:12:40Z",
                "DateTimeLastModified": "2014-10-19T23:12:42Z",
                "Subject": "Rob:Alex 1:1",
                "BodyPreview": "Let's meet every month to engage in project review and career discussion",
                "Body": {
                    "ContentType": "HTML",
                    "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n<style type=\"text/css\" style=\"display:none\"><!-- p { margin-top: 0px; margin-bottom: 0px; }--></style>\r\n</head>\r\n<body dir=\"ltr\" style=\"font-size:12pt;color:#000000;background-color:#FFFFFF;font-family:Calibri,Arial,Helvetica,sans-serif;\">\r\n<p>Let's meet every month to engage in project review and career discussion<br>\r\n</p>\r\n</body>\r\n</html>\r\n"
                },
                "Importance": "Normal",
                "HasAttachments": false,
                "ParentFolderId": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEJAAA=",
                "From": {
                    "EmailAddress": {
                        "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
                        "Name": "Alex D"
                    }
                },
                "Sender": {
                    "EmailAddress": {
                        "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
                        "Name": "Alex D"
                    }
                },
                "ToRecipients": [
                    {
                        "EmailAddress": {
                            "Address": "roby@a830edad9050849NDA1.onmicrosoft.com",
                            "Name": "Rob Young"
                        }
                    }
                ],
                "CcRecipients": [],
                "BccRecipients": [],
                "ReplyTo": [],
                "ConversationId": "AAQkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAQAFeDWE5GradIk5oqUb-_emE=",
                "DateTimeReceived": "2014-10-19T23:12:42Z",
                "DateTimeSent": "2014-10-19T23:12:40Z",
                "IsDeliveryReceiptRequested": null,
                "IsReadReceiptRequested": false,
                "IsDraft": false,
                "IsRead": true
            }
        ]
    }
}
```

 **Response type**

The requested [message](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#MessageResource) collection. 


****

<a name="GetMessage"> </a>
###Get a message (REST)

_Required scope: **Mail.Read** or **Mail.Write**_

Get a message by ID.


```no-highlight
GET https://outlook.office365.com/api/{version}/me/messages/{message_id}
```


|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|message_id|string|The message ID.|

```REST-i
{
    "method": "GET",
    "resourceFormat": "https://outlook.office365.com/api/v1.0/me/messages/{message_id}",
    "requestUrl": "https://outlook.office365.com/api/v1.0/me/messages/AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEMAACd9nJ-tVysQos2hTfspaWRAAADTHVSAAA=",
    "requestHeaders": {
        "Accept": "application/json"
    },
    "parameterDetails": [
        {
            "name": "message_id",
            "type": "string",
            "description": "The message ID.",
            "defaultValue": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEMAACd9nJ-tVysQos2hTfspaWRAAADTHVSAAA="
        }
    ],
    "statusCode": 200,
    "responseBody": {
        "@odata.context": "https://outlook.office365.com/api/v1.0/$metadata#Me/Messages/$entity",
        "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Messages('AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEMAACd9nJ-tVysQos2hTfspaWRAAADTHVSAAA=')",
        "@odata.etag": "W/\"CQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTIKz\"",
        "Id": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEMAACd9nJ-tVysQos2hTfspaWRAAADTHVSAAA=",
        "ChangeKey": "CQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTIKz",
        "Categories": [],
        "DateTimeCreated": "2014-10-20T00:41:57Z",
        "DateTimeLastModified": "2014-10-20T00:41:57Z",
        "Subject": "Re: Meeting Notes",
        "BodyPreview": "________________________________________\nFrom: Alex D\nSent: Sunday, October 19, 2014 5:28 PM\nTo: Katie Jordan\nSubject: Meeting Notes\n\nPlease send me the meeting notes ASAP",
        "Body": {
            "ContentType": "Text",
            "Content": "\n________________________________________\nFrom: Alex D\nSent: Sunday, October 19, 2014 5:28 PM\nTo: Katie Jordan\nSubject: Meeting Notes\n\nPlease send me the meeting notes ASAP\n"
        },
        "Importance": "Normal",
        "HasAttachments": true,
        "ParentFolderId": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEMAAA=",
        "From": {
            "EmailAddress": {
                "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com",
                "Name": "Katie Jordan"
            }
        },
        "Sender": {
            "EmailAddress": {
                "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com",
                "Name": "Katie Jordan"
            }
        },
        "ToRecipients": [
            {
                "EmailAddress": {
                    "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
                    "Name": "Alex D"
                }
            }
        ],
        "CcRecipients": [],
        "BccRecipients": [],
        "ReplyTo": [],
        "ConversationId": "AAQkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAQAEmjRj3pwjRLrbymGMYyEto=",
        "DateTimeReceived": "2014-10-20T00:41:57Z",
        "DateTimeSent": "2014-10-20T00:41:53Z",
        "IsDeliveryReceiptRequested": false,
        "IsReadReceiptRequested": false,
        "IsDraft": false,
        "IsRead": false
    }
}
```

 **Response type**

The requested [message](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#MessageResource).

**Note** By default, the response includes all the properties of the specified message. Use **$select** to specify only those properties you need for
best performance. The **Id** property is always returned. See [OData query parameters](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#OdataQueryParams) for filtering, sorting, and paging parameters.

The following example shows how to use **$select** to specify returning only the **Sender** and **Subject** properties of the message
in the response.

```
GET https://outlook.office365.com/api/v1.0/me/messages/AAMkAGE1M2IyNGNmLTI5MTAAA=?$select=Sender,Subject
```

****

<a name="GetMessagesClient"> </a>
###Get a message collection (Client)

Get the messages in the Inbox by using the `Me.Messages` shortcut property. To get the messages from a different folder, use the folder's **Messages** property.
 You can use the following well-known folder names instead of the ID for the corresponding folder: `Inbox`, `SentItems`, `Drafts`, `DeletedItems`.

Example: `outlookClient.Me.Folders["SentItems"].Messages.ExecuteAsync()`

**Note** Message collections support query expressions such as **Select**, **OrderBy**, and **Take**.

This example calls the method that [creates the Outlook Services client](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetClient).

<!-- BEGINSECTION class="tabbedCodeSnippets"  data-resources="OutlookServices.Mail" -->

<!--tested-->
```cs-i
var outlookClient = await CreateOutlookClientAsync("Mail");
    var messages = await outlookClient.Me.Folders["Inbox"].Messages
      .OrderByDescending(m => m.DateTimeReceived)
      .Take(10)
      .ExecuteAsync();

foreach(var message in messages.CurrentPage)
{
  System.Diagnostics.Debug.WriteLine("Message '{0}' received at '{1}'.",
    message.Subject,
    message.DateTimeReceived.ToString());
}

```
```javascript-i
outlookClient.me.folders.getFolder('Inbox').messages.getMessages().orderBy('DateTimeReceived desc').fetchAll(10).then(function (result) {
    result.forEach(function (message) {
        console.log('Message "' + message.subject + '" received at "' + message.dateTimeReceived.toString() + '"');
    });
}, function(error) {
    console.log(error);
});
```

<!-- ENDSECTION -->

****

<a name="GetMessageClient"> </a>
###Get a message (Client)

To get a particular message, specify the message ID as the index of the **Messages** collection or use the **GetById** method.

****


<a name="SendMessages"> </a>
##Create and send messages

You can send a new message on the fly, or create a draft message and then send it. You can create drafts in any folder.

REST API: [Send a new message on the fly (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SendMessageOnTheFly) | [Create a draft message (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#CreateNewDraft) | [Send a draft message (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SendDraftMessages)

Client libraries: [Send a new message on the fly (Client)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SendMessageOnTheFlyClient) | [Create a draft message (Client)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#CreateDraftClient) | [Send a draft message (Client)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SendDraftClient)


<a name="SendMessageOnTheFly"> </a>
###Send a new message on the fly (REST)

_Required scope: **Mail.Send**_

Send the message supplied in the request body by using the **SendMail** method. You can optionally save the message in the Sent Items folder.

```no-highlight
POST https://outlook.office365.com/api/{version}/me/sendmail
```

|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameter_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|_Body parameters_|
|Message|[Message](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#MessageResource)|The message to send.|
|SavetoSentItems|boolean|Indicates whether to save the message in Sent Items. Default is **true**.|

Specify the **Message** parameter with the required **ToRecipients** property and any writable [message](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#MessageResource) properties in the request body.
 The **SaveToSentItems** parameter is required only if **false**.

```REST
{
    "method": "POST",
    "resourceFormat": "https://outlook.office365.com/api/v1.0/me/sendmail",
    "requestUrl": "https://outlook.office365.com/api/v1.0/me/sendmail",
    "requestHeaders": {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "Content-Length": 282
    },
    "parameterDetails": [],
    "requestBody": {
        "Message": {
            "Subject": "Meet for lunch?",
            "Body": {
                "ContentType": "Text",
                "Content": "The new cafeteria is open."
            },
            "ToRecipients": [
                {
                    "EmailAddress": {
                        "Address": "garthf@a830edad9050849NDA1.onmicrosoft.com"
                    }
                }
            ],
            "Attachments": [
                {
                    "@odata.type": "#Microsoft.OutlookServices.FileAttachment",
                    "Name": "menu.txt",
                    "ContentBytes": "bWFjIGFuZCBjaGVlc2UgdG9kYXk="
                }
            ]
        },
        "SaveToSentItems": "false"
    },
    "statusCode": 202

}
```

****

<a name="CreateNewDraft"> </a>
###Create a draft message (REST)

_Required scope: **Mail.Write**_

Create a draft of a new message. Drafts can be created in any folder and optionally [updated](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#UpdateMessages) before [sending](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SendDraftMessages).
 To save to the Drafts folder, you use the `/me/messages` shortcut.

```no-highlight
POST https://outlook.office365.com/api/{version}/me/messages
POST https://outlook.office365.com/api/{version}/me/folders/{folder_id}/messages
```

|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|folder_id|string|The destination folder ID, or the `Inbox` or `Drafts` well-known folder name.|

Specify any writable [message](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#MessageResource) properties in the request body.

```REST
{
    "method": "POST",
    "resourceFormat": "https://outlook.office365.com/api/v1.0/me/folders/{folder_id}/messages",
    "requestUrl": "https://outlook.office365.com/api/v1.0/me/folders/inbox/messages",
    "requestHeaders": {
        "Accept": "application/json",
	"Content-Type": "application/json",
    },
    "parameterDetails": [
        {
            "name": "folder_id",
            "type": "string",
            "description": "The destination folder ID, or the well-known name: Inbox or Drafts. To save to the Drafts folder, you can use the '/me/messages' endpoint.",
            "defaultValue": "inbox"
        }
    ],
    "requestBody": {
        "Subject": "Did you see last night's game?",
        "Importance": "Low",
        "Body": {
            "ContentType": "HTML",
            "Content": "They were <b>awesome</b>!"
        },
        "ToRecipients": [
            {
                "EmailAddress": {
                    "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com"
                }
            }
        ]
    },
    "statusCode": 201,
    "responseBody": {
        "@odata.context": "https://outlook.office365.com/api/v1.0/$metadata#Me/Messages/$entity",
        "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Messages('AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEPAAAmP1Ln1wcHRariNdTMGAO9AAASz7k0AAA=')",
        "@odata.etag": "W/\"CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0Ag5\"",
        "Id": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEPAAAmP1Ln1wcHRariNdTMGAO9AAASz7k0AAA=",
        "ChangeKey": "CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0Ag5",
        "Categories": [],
        "DateTimeCreated": "2014-10-18T20:06:51Z",
        "DateTimeLastModified": "2014-10-18T20:06:51Z",
        "Subject": "Did you see last night's game?",
        "BodyPreview": "They were awesome!",
        "Body": {
            "ContentType": "HTML",
            "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n</head>\r\n<body>\r\nThey were <b>awesome</b>!\r\n</body>\r\n</html>\r\n"
        },
        "Importance": "Low",
        "HasAttachments": false,
        "ParentFolderId": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAAAAAEPAAA=",
        "From": null,
        "Sender": null,
        "ToRecipients": [
            {
                "EmailAddress": {
                    "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com",
                    "Name": "Katie Jordan"
                }
            }
        ],
        "CcRecipients": [],
        "BccRecipients": [],
        "ReplyTo": [],
        "ConversationId": "AAQkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAQADXiGXoAtX9IubRTpv2hisc=",
        "DateTimeReceived": "2014-10-18T20:06:51Z",
        "DateTimeSent": "2014-10-18T20:06:51Z",
        "IsDeliveryReceiptRequested": false,
        "IsReadReceiptRequested": false,
        "IsDraft": true,
        "IsRead": true
    }
}
```

 **Response type**

The draft [message](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#MessageResource).

****

<a name="SendDraftMessages"> </a>
###Send a draft message (REST)

_Required scope: **Mail.Send**_

Send a [new message draft](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#CreateNewDraft), a [Reply draft](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#CreateReplyDraft), a [Reply All draft](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#CreateReplyAllDraft), or a [Forward draft](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#CreateForwardDraft)  by using the **Send** method.
 The message is then saved in the Sent Items folder.

```no-highlight
POST https://outlook.office365.com/api/{version}/me/messages/{message_id}/send
```

|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|message_id|string|The ID of the draft message to send.|

```REST
{
    "method": "POST",
    "resourceFormat": "https://outlook.office365.com/api/v1.0/me/messages/{message_id}/send",
    "requestUrl": "https://outlook.office365.com/api/v1.0/me/messages/AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEPAAAmP1Ln1wcHRariNdTMGAO9AAASz7k0AAA=/send",
    "requestHeaders": {
        "Accept": "application/json"
    },
    "parameterDetails": [
        {
            "name": "message_id",
            "type": "string",
            "description": "The ID of the message to send.",
            "defaultValue": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEPAAAmP1Ln1wcHRariNdTMGAO9AAASz7k0AAA="
        }
    ],
    "statusCode": 202

}
```

****

<a name="SendMessageOnTheFlyClient"> </a>
###Send a new message on the fly (Client)

Create a message and pass it to the **SendMailAsync** method.


This example assumes you already [got the Outlook Services client](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetClient).

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
ItemBody body = new ItemBody
{
    Content = "It was <b>awesome</b>!",
    ContentType = BodyType.HTML
};
List<Recipient> toRecipients = new List<Recipient>();
toRecipients.Add(new Recipient
{
    EmailAddress = new EmailAddress
    {
        Address = "katiej@a830edad9050849NDA1.onmicrosoft.com"
    }
});
toRecipients.Add(new Recipient
{
    EmailAddress = new EmailAddress
    {
        Address = "pavelb@a830edad9050849NDA1.onmicrosoft.com"
    }
});
Message newMessage = new Message
{
    Subject = "Did you see last night's game?",
    Body = body,
    ToRecipients = toRecipients
};

// To send a message without saving to Sent Items, specify false for  
// the SavetoSentItems parameter.
await outlookClient.Me.SendMailAsync(newMessage, true);
```

<!-- ENDSECTION -->

****

<a name="CreateDraftClient"> </a>
###Create a draft message (Client)

Create a draft message and pass it to the **AddMessageAsync** method. Then you can [update the draft](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#UpdateMessagesClient) and [send it](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SendDraftClient).

This example assumes you already [got the Outlook Services client](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetClient).

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
ItemBody body = new ItemBody
{
    Content = "I'm coming out a week later.",
    ContentType = BodyType.HTML
};
List<Recipient> toRecipients = new List<Recipient>();
toRecipients.Add(new Recipient
{
    EmailAddress = new EmailAddress
    {
        Address = "katiej@a830edad9050849NDA1.onmicrosoft.com"
    }
});
Message draftMessage = new Message
{
    Subject = "Changed my travel plans",
    Body = body,
    ToRecipients = toRecipients,
    Importance = Importance.High
};

// Save the draft message. Saving to Me.Messages saves the message in the Drafts folder.
await outlookClient.Me.Messages.AddMessageAsync(draftMessage);

// Get the ID of the message.
string messageId = draftMessage.Id;
```

<!-- ENDSECTION -->

Adding a message to the **Me.Messages** collection saves the draft in the Drafts folder, but you can save a draft in the **Messages** collection of any folder.

Example: `outlookClient.Me.Folders["AAMkADE3N..."].Messages.AddMessageAsync(newMessage)`

****

<a name="SendDraftClient"> </a>
###Send a draft message (Client)

Send a draft message by calling **SendAsync**. You can send a draft of a new, reply, reply-all, or forward message.

This example assumes you already [got the Outlook Services client](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetClient) and [got the message ID](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetMessagesClient).

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
await outlookClient.Me.Messages[messageId].SendAsync();
```

<!-- ENDSECTION -->

****

<a name="ReplyToMessages"> </a>
##Reply or reply all to messages

You can reply with a message on the fly, or you can first create a reply draft, then update and send the draft.
You can reply to only the sender of the message or reply to all recipients at once.

REST API: [Reply to sender on the fly (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#ReplyToSender) | [Reply all on the fly (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#ReplyAll) | [Create a draft reply message (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#CreateReplyDraft) | [Create a draft reply-all message (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#CreateReplyAllDraft)

Client libraries: [Reply or reply all on the fly (Client)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#ReplyOnTheFlyClient) | [Create a draft reply or draft reply-all message (Client)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#CreateDraftReplyClient)

<a name="ReplyToSender"> </a>
###Reply to sender on the fly (REST)

_Required scope: **Mail.Send**_

Reply to the sender of a message by using the **Reply** method. The message is then saved in the Sent Items folder.

Or, you can [create draft Reply message](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#CreateReplyDraft) and then [update](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#UpdateMessages) and [send](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SendDraftMessages) it.

```no-highlight
POST https://outlook.office365.com/api/{version}/me/messages/{message_id}/reply
```

|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|message_id|string|The ID of the message to reply to.|
|_Body parameters_|
|Comment|string|A comment to include. Can be an empty string.|

```REST
{
    "method": "POST",
    "resourceFormat": "https://outlook.office365.com/api/v1.0/me/messages/{message_id}/reply",
    "requestUrl": "https://outlook.office365.com/api/v1.0/me/messages/AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEJAAAmP1Ln1wcHRariNdTMGAO9AAASz8DmAAA=/reply",
    "requestHeaders": {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "Content-Length": 51
    },
    "parameterDetails": [
        {
            "name": "message_id",
            "type": "string",
            "description": "The ID of the message to reply to.",
            "defaultValue": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEJAAAmP1Ln1wcHRariNdTMGAO9AAASz8DmAAA="
        }
    ],
    "requestBody": {
  "Comment":"Sounds great! See you tomorrow."
},
    "statusCode": 202

}
```

****


<a name="ReplyAll"> </a>
###Reply all on the fly (REST)

_Required scope: **Mail.Send**_

Reply to all recipients of a message by using the **ReplyAll** method. The message is then saved in the Sent Items folder.

Or, you can [create draft Reply All message](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#CreateReplyAllDraft) and then [update](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#UpdateMessages) and [send](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SendDraftMessages) it.

```no-highlight
POST https://outlook.office365.com/api/{version}/me/messages/{message_id}/replyall
```

|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|message_id|string|The ID of the message to reply to.|
|_Body parameters_|
|Comment|string|A comment to include. Can be an empty string.|

```REST
{
    "method": "POST",
    "resourceFormat": "https://outlook.office365.com/api/v1.0/me/messages/{message_id}/replyall",
    "requestUrl": "https://outlook.office365.com/api/v1.0/me/messages/AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEJAAAmP1Ln1wcHRariNdTMGAO9AAASz8DmAAA=/replyall",
    "requestHeaders": {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "Content-Length": 44
    },
    "parameterDetails": [
        {
            "name": "message_id",
            "type": "string",
            "description": "The ID of the message to reply to.",
            "defaultValue": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEJAAAmP1Ln1wcHRariNdTMGAO9AAASz8DmAAA="
        }
    ],
    "requestBody": {
        "Comment": "Thanks for the heads up."
    },
    "statusCode": 202

}
```

****

<a name="CreateReplyDraft"> </a>
###Create a draft reply message (REST)

_Required scope: **Mail.Write**_

Create a draft of the Reply message by using the **CreateReply** method. You can then [update](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#UpdateMessages) and [send](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SendDraftMessages) the draft.

```no-highlight
POST https://outlook.office365.com/api/{version}/me/messages/{message_id}/createreply
```

|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|message_id|string|The ID of the message to reply to.|

```REST
{
    "method": "POST",
    "resourceFormat": "https://outlook.office365.com/api/v1.0/me/messages/{message_id}/createreply",
    "requestUrl": "https://outlook.office365.com/api/v1.0/me/messages/AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEMAAAmP1Ln1wcHRariNdTMGAO9AAASz8TAAAA=/createreply",
    "requestHeaders": {
        "Accept": "application/json"
    },
    "parameterDetails": [
        {
            "name": "message_id",
            "type": "string",
            "description": "The ID of the message to reply to.",
            "defaultValue": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEMAAAmP1Ln1wcHRariNdTMGAO9AAASz8TAAAA="
        }
    ],
    "statusCode": 201,
    "responseBody":
{
    "@odata.context": "https://outlook.office365.com/api/v1.0/$metadata#Me/Messages/$entity",
    "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Messages('AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEPAAAmP1Ln1wcHRariNdTMGAO9AAASz7k2AAA=')",
    "@odata.etag": "W/\"CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0Ag+\"",
    "Id": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEPAAAmP1Ln1wcHRariNdTMGAO9AAASz7k2AAA=",
    "ChangeKey": "CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0Ag+",
    "Categories": [],
    "DateTimeCreated": "2014-10-18T20:59:38Z",
    "DateTimeLastModified": "2014-10-18T20:59:38Z",
    "Subject": "RE: Did you see last night's game?",
    "BodyPreview": "________________________________\r\nFrom: Alex D\r\nSent: Saturday, October 18, 2014 8:49:03 PM\r\nTo: Katie Jordan\r\nSubject: RE: Did you see last night's game?\r\n\r\nI was at the game!\r\n\r\nFrom: Alex D [mailto:alexd@a830edad9050849NDA1.onmicrosoft.com]",
    "Body": {
        "ContentType": "HTML",
        "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n<meta content=\"text/html; charset=us-ascii\">\r\n<meta name=\"Generator\" content=\"Microsoft Word 15 (filtered medium)\">\r\n<style>\r\n<!--\r\n@font-face\r\n\t{font-family:\"Cambria Math\"}\r\n@font-face\r\n\t{font-family:Calibri}\r\np.MsoNormal, li.MsoNormal, div.MsoNormal\r\n\t{margin:0in;\r\n\tmargin-bottom:.0001pt;\r\n\tfont-size:12.0pt;\r\n\tfont-family:\"Times New Roman\",\"serif\"}\r\na:link, span.MsoHyperlink\r\n\t{color:#0563C1;\r\n\ttext-decoration:underline}\r\na:visited, span.MsoHyperlinkFollowed\r\n\t{color:#954F72;\r\n\ttext-decoration:underline}\r\nspan.EmailStyle17\r\n\t{font-family:\"Calibri\",\"sans-serif\";\r\n\tcolor:#1F497D}\r\n.MsoChpDefault\r\n\t{font-size:10.0pt}\r\n@page WordSection1\r\n\t{margin:1.0in 1.0in 1.0in 1.0in}\r\ndiv.WordSection1\r\n\t{}\r\n-->\r\n</style>\r\n</head>\r\n<body lang=\"EN-US\" link=\"#0563C1\" vlink=\"#954F72\">\r\n<hr tabindex=\"-1\" style=\"display:inline-block; width:98%\">\r\n<div id=\"divRplyFwdMsg\" dir=\"ltr\"><font face=\"Calibri, sans-serif\" color=\"#000000\" style=\"font-size:11pt\"><b>From:</b> Katie Jordan;<br>\r\n<b>Sent:</b> Saturday, October 18, 2014 8:49:03 PM<br>\r\n<b>To:</b> Alex D<br>\r\n<b>Subject:</b> RE: Did you see last night's game?</font>\r\n<div>&nbsp;</div>\r\n</div>\r\n<div>\r\n<div class=\"WordSection1\">\r\n<p class=\"MsoNormal\"><span style=\"font-size:11.0pt; font-family:&quot;Calibri&quot;,&quot;sans-serif&quot;; color:#1F497D\">I was at the game!</span></p>\r\n<p class=\"MsoNormal\"><span style=\"font-size:11.0pt; font-family:&quot;Calibri&quot;,&quot;sans-serif&quot;; color:#1F497D\">&nbsp;</span></p>\r\n<div>\r\n<div style=\"border:none; border-top:solid #E1E1E1 1.0pt; padding:3.0pt 0in 0in 0in\">\r\n<p class=\"MsoNormal\"><b><span style=\"font-size:11.0pt; font-family:&quot;Calibri&quot;,&quot;sans-serif&quot;\">From:</span></b><span style=\"font-size:11.0pt; font-family:&quot;Calibri&quot;,&quot;sans-serif&quot;\"> Alex D [mailto:alexd@a830edad9050849NDA1.onmicrosoft.com]\r\n<br>\r\n<b>Sent:</b> Saturday, October 18, 2014 1:39 PM<br>\r\n<b>To:</b> Katie Jordan<br>\r\n<b>Subject:</b> Did you see last night's game?<br>\r\n<b>Importance:</b> Low</span></p>\r\n</div>\r\n</div>\r\n<p class=\"MsoNormal\">&nbsp;</p>\r\n<p class=\"MsoNormal\">They were <b>awesome</b>! </p>\r\n</div>\r\n</div>\r\n</body>\r\n</html>\r\n"
    },
    "Importance": "Normal",
    "HasAttachments": false,
    "ParentFolderId": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAAAAAEPAAA=",
    "From": null,
    "Sender": {
        "EmailAddress": {
            "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
            "Name": "Alex D"
        }
    },
    "ToRecipients": [
        {
            "EmailAddress": {
                "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com",
                "Name": "Katie Jordan"
            }
        }
    ],
    "CcRecipients": [],
    "BccRecipients": [],
    "ReplyTo": [],
    "ConversationId": "AAQkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAQADXiGXoAtX9IubRTpv2hisc=",
    "DateTimeReceived": "2014-10-18T20:59:38Z",
    "DateTimeSent": "2014-10-18T20:59:38Z",
    "IsDeliveryReceiptRequested": false,
    "IsReadReceiptRequested": false,
    "IsDraft": true,
    "IsRead": true
}
}
```

 **Response type**

The draft Reply [message](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#MessageResource) with the **ToRecipient**, **IsDraft**, and other appropriate properties prepopulated.

****


<a name="CreateReplyAllDraft"> </a>
###Create a draft reply-all message (REST)

_Required scope: **Mail.Write**_

Create a draft of the Reply All message by using the **CreateReplyAll** method. You can then [update](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#UpdateMessages) and [send](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SendDraftMessages) the draft.

```no-highlight
POST https://outlook.office365.com/api/{version}/me/messages/{message_id}/createreplyall
```

|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|message_id|string|The ID of the message to reply-all to.|

```REST
{
    "method": "POST",
    "resourceFormat": "https://outlook.office365.com/api/v1.0/me/messages/{message_id}/createreplyall",
    "requestUrl": "https://outlook.office365.com/api/v1.0/me/messages/AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEJAAAmP1Ln1wcHRariNdTMGAO9AAASz8DmAAA=/createreplyall",
    "requestHeaders": {
        "Accept": "application/json"
    },
    "parameterDetails": [
        {
            "name": "message_id",
            "type": "string",
            "description": "The ID of the message to reply to.",
            "defaultValue": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEJAAAmP1Ln1wcHRariNdTMGAO9AAASz8DmAAA="
        }
    ],
    "statusCode": 201,
    "responseBody":
{
    "@odata.context": "https://outlook.office365.com/api/v1.0/$metadata#Me/Messages/$entity",
    "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Messages('AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEPAAAmP1Ln1wcHRariNdTMGAO9AAASz7k5AAA=')",
    "@odata.etag": "W/\"CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0AhF\"",
    "Id": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEPAAAmP1Ln1wcHRariNdTMGAO9AAASz7k5AAA=",
    "ChangeKey": "CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0AhF",
    "Categories": [],
    "DateTimeCreated": "2014-10-18T21:21:06Z",
    "DateTimeLastModified": "2014-10-18T21:21:06Z",
    "Subject": "RE: Check out the new Office 365 APIs",
    "BodyPreview": "________________________________\r\nFrom: Alex D\r\nSent: Saturday, October 18, 2014 9:18:18 PM\r\nTo: Katie Jordan; Garth Fort\r\nSubject: Check out the new Office 365 APIs\r\n\r\n\r\nhttp://msdn.microsoft.com/library/office/dn605892(v=office.15)",
    "Body": {
        "ContentType": "HTML",
        "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n<meta content=\"text/html; charset=iso-8859-1\">\r\n<style type=\"text/css\" style=\"\">\r\n<!--\r\np\r\n\t{margin-top:0px;\r\n\tmargin-bottom:0px}\r\n-->\r\n</style>\r\n</head>\r\n<body dir=\"ltr\">\r\n<hr tabindex=\"-1\" style=\"display:inline-block; width:98%\">\r\n<div id=\"divRplyFwdMsg\" dir=\"ltr\"><font face=\"Calibri, sans-serif\" color=\"#000000\" style=\"font-size:11pt\"><b>From:</b> Alex D<br>\r\n<b>Sent:</b> Saturday, October 18, 2014 9:18:18 PM<br>\r\n<b>To:</b> Katie Jordan; Garth Fort<br>\r\n<b>Subject:</b> Check out the new Office 365 APIs</font>\r\n<div>&nbsp;</div>\r\n</div>\r\n<div>\r\n<div id=\"divtagdefaultwrapper\" style=\"font-size:12pt; color:#000000; background-color:#FFFFFF; font-family:Calibri,Arial,Helvetica,sans-serif\">\r\n<p><a id=\"lnk681678\" href=\"http://msdn.microsoft.com/library/office/dn605892(v=office.15\">http://msdn.microsoft.com/library/office/dn605892(v=office.15)</a></p>\r\n</div>\r\n</div>\r\n</body>\r\n</html>\r\n"
    },
    "Importance": "Normal",
    "HasAttachments": false,
    "ParentFolderId": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAAAAAEPAAA=",
    "From": null,
    "Sender": {
        "EmailAddress": {
            "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
            "Name": "Alex D"
        }
    },
    "ToRecipients": [
        {
            "EmailAddress": {
                "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com",
                "Name": "Katie Jordan"
            }
        },
        {
            "EmailAddress": {
                "Address": "garthf@a830edad9050849NDA1.onmicrosoft.com",
                "Name": "Garth Fort"
            }
        }
    ],
    "CcRecipients": [],
    "BccRecipients": [],
    "ReplyTo": [],
    "ConversationId": "AAQkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAQAOg2iJIA6sVCj8rl3HbTkEU=",
    "DateTimeReceived": "2014-10-18T21:21:06Z",
    "DateTimeSent": "2014-10-18T21:21:06Z",
    "IsDeliveryReceiptRequested": false,
    "IsReadReceiptRequested": false,
    "IsDraft": true,
    "IsRead": true
}
}
```

 **Response type**

The draft Reply All [message](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#MessageResource) with the **ToRecipient**, **IsDraft**, and other appropriate properties prepopulated.

****

<a name="ReplyOnTheFlyClient"></a>
###Reply or reply all on the fly (Client)

Reply directly to the sender of the message or to all recipients by calling **ReplyAsync** or **ReplyAllAsync** and passing in a comment.


This example assumes you already [got the Outlook Services client](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetClient) and [got the message ID](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetMessagesClient).

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
await outlookClient.Me.Messages[messageId].ReplyAsync("Count me in.");
```

<!-- ENDSECTION -->

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
await outlookClient.Me.Messages[messageId].ReplyAllAsync("Count me in.");
```

<!-- ENDSECTION -->

<a name="CreateDraftReplyClient"> </a>
###Create a draft reply or draft reply-all message (Client)

Create a draft Reply or ReplyAll message by calling **CreateReplyAsync** or **CreateReplyAllAsync**. Then you can [update the draft](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#UpdateMessagesClient) and [send it](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SendDraftClient).


This example assumes you already [got the Outlook Services client](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetClient) and [got the message ID](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetMessagesClient).

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
IMessage replyDraft = await outlookClient.Me.Messages[messageId].CreateReplyAsync();

// Get the ID of the draft Reply message.
string replyMessageId = replyDraft.Id;
```

<!-- ENDSECTION -->

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
IMessage replyAllDraft = await outlookClient.Me.Messages[messageId].CreateReplyAllAsync();

// Get the ID of the draft Reply All message.
string replyAllMessageId = replyAllDraft.Id;
```

<!-- ENDSECTION -->

**CreateReplyAsync** and **CreateReplyAllAsync** create a draft message with the **ToRecipients**, **IsDraft**, and other appropriate properties prepopulated.


****


<a name="ForwardMessages"> </a>
##Forward new or drafted messages

You can forward a message directly, or you can create a draft forward message, update and then send it.

REST API: [Forward a message directly (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#ForwardDirectly) | [Create a Forward message (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#CreateForwardDraft)

Client libraries: [Forward a message directly (Client)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#ForwardDirectlyClient) | [Create a draft Forward message (Client)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#CreateDraftForwardClient)

<a name="ForwardDirectly"></a>
###Forward a message directly (REST)

_Required scope: **Mail.Send**_

Forward a message by using the **Forward** method. The message is saved in the Sent Items folder.

Or, you can [create draft Forward message](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#CreateForwardDraft) and then update and send it.

```no-highlight
POST https://outlook.office365.com/api/{version}/me/messages/{message_id}/forward
```

|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|message_id|string|The ID of the message to forward.|
|_Body parameters_|
|Comment|string|A comment to include. Can be an empty string.|
|ToRecipients|Collection([Recipient](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#ComplexTypes))|The list of recipients.|

Specify the **Comment** and **ToRecipients** parameters in the request body.

```REST
{
    "method": "POST",
    "resourceFormat": "https://outlook.office365.com/api/v1.0/me/messages/{message_id}/forward",
    "requestUrl": "https://outlook.office365.com/api/v1.0/me/messages/AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEJAAAmP1Ln1wcHRariNdTMGAO9AAASz8DmAAA=/forward",
    "requestHeaders": {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "Content-Length": 291
    },
    "parameterDetails": [
        {
            "name": "message_id",
            "type": "string",
            "description": "The ID of the message to reply to.",
            "defaultValue": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEJAAAmP1Ln1wcHRariNdTMGAO9AAASz8DmAAA="
        }
    ],
    "requestBody": {
        "Comment": "FYI",
        "ToRecipients": [
            {
                "EmailAddress": {
                    "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com"
                }
            },
            {
                "EmailAddress": {
                    "Address": "garthf@a830edad9050849NDA1.onmicrosoft.com"
                }
            }
        ]
    },
    "statusCode": 202

}
```

****

<a name="CreateForwardDraft"> </a>
###Create a draft forward message (REST)

_Required scope: **Mail.Write**_

Create a draft of the Forward message by using the **CreateForward** method. You can then [update](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#UpdateMessages) and [send](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SendDraftMessages) the draft.

```no-highlight
POST https://outlook.office365.com/api/{version}/me/messages/{message_id}/createforward
```

|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|message_id|string|The ID of the message to forward.|

```REST
{
    "method": "POST",
    "resourceFormat": "https://outlook.office365.com/api/v1.0/me/messages/{message_id}/createforward",
    "requestUrl": "https://outlook.office365.com/api/v1.0/me/messages/AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEJAAAmP1Ln1wcHRariNdTMGAO9AAASz8DmAAA=/createforward",
    "requestHeaders": {
        "Accept": "application/json"
    },
    "parameterDetails": [
        {
            "name": "message_id",
            "type": "string",
            "description": "The ID of the message to reply to.",
            "defaultValue": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEJAAAmP1Ln1wcHRariNdTMGAO9AAASz8DmAAA="
        }
    ],
    "statusCode": 201,
    "responseBody":
{
    "@odata.context": "https://outlook.office365.com/api/v1.0/$metadata#Me/Messages/$entity",
    "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Messages('AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEPAAAmP1Ln1wcHRariNdTMGAO9AAASz7k6AAA=')",
    "@odata.etag": "W/\"CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0AhG\"",
    "Id": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEPAAAmP1Ln1wcHRariNdTMGAO9AAASz7k6AAA=",
    "ChangeKey": "CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0AhG",
    "Categories": [],
    "DateTimeCreated": "2014-10-18T21:29:28Z",
    "DateTimeLastModified": "2014-10-18T21:29:28Z",
    "Subject": "FW: Check out the new Office 365 APIs",
    "BodyPreview": "________________________________\r\nFrom: Alex D\r\nSent: Saturday, October 18, 2014 9:18:18 PM\r\nTo: Katie Jordan; Garth Fort\r\nSubject: Check out the new Office 365 APIs\r\n\r\n\r\nhttp://msdn.microsoft.com/library/office/dn605892(v=office.15)",
    "Body": {
        "ContentType": "HTML",
        "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n<meta content=\"text/html; charset=iso-8859-1\">\r\n<style type=\"text/css\" style=\"\">\r\n<!--\r\np\r\n\t{margin-top:0px;\r\n\tmargin-bottom:0px}\r\n-->\r\n</style>\r\n</head>\r\n<body dir=\"ltr\">\r\n<hr tabindex=\"-1\" style=\"display:inline-block; width:98%\">\r\n<div id=\"divRplyFwdMsg\" dir=\"ltr\"><font face=\"Calibri, sans-serif\" color=\"#000000\" style=\"font-size:11pt\"><b>From:</b> Alex D<br>\r\n<b>Sent:</b> Saturday, October 18, 2014 9:18:18 PM<br>\r\n<b>To:</b> Katie Jordan; Garth Fort<br>\r\n<b>Subject:</b> Check out the new Office 365 APIs</font>\r\n<div>&nbsp;</div>\r\n</div>\r\n<div>\r\n<div id=\"divtagdefaultwrapper\" style=\"font-size:12pt; color:#000000; background-color:#FFFFFF; font-family:Calibri,Arial,Helvetica,sans-serif\">\r\n<p><a id=\"lnk681678\" href=\"http://msdn.microsoft.com/library/office/dn605892(v=office.15\">http://msdn.microsoft.com/library/office/dn605892(v=office.15)</a></p>\r\n</div>\r\n</div>\r\n</body>\r\n</html>\r\n"
    },
    "Importance": "Normal",
    "HasAttachments": false,
    "ParentFolderId": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAAAAAEPAAA=",
    "From": null,
    "Sender": {
        "EmailAddress": {
            "Address": "'alexd@a830edad9050849NDA1.onmicrosoft.com'",
            "Name": "Alex D"
        }
    },
    "ToRecipients": [],
    "CcRecipients": [],
    "BccRecipients": [],
    "ReplyTo": [],
    "ConversationId": "AAQkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAQAOg2iJIA6sVCj8rl3HbTkEU=",
    "DateTimeReceived": "2014-10-18T21:29:28Z",
    "DateTimeSent": "2014-10-18T21:29:28Z",
    "IsDeliveryReceiptRequested": false,
    "IsReadReceiptRequested": false,
    "IsDraft": true,
    "IsRead": true
}
}
```

 **Response type**

The draft Forward [message](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#MessageResource) with **IsDraft** and appropriate properties prepopulated.

****


<a name="ForwardDirectlyClient"></a>
###Forward a message directly (Client)

Forward a message directly by calling **ForwardAsync** and passing in a comment and the recipients.


This example assumes you already [got the Outlook Services client](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetClient) and [got the message ID](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetMessagesClient).

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
List<Recipient> recipients = new List<Recipient>();
recipients.Add(new Recipient
{
    EmailAddress = new EmailAddress
    {
        Address = "garthf@a830edad9050849NDA1.onmicrosoft.com"
    }
});
await outlookClient.Me.Messages[messageId].ForwardAsync("Interested?", recipients);
```

<!-- ENDSECTION -->

<a name="CreateDraftForwardClient"> </a>
###Create a draft Forward message (Client)

Create a draft Forward message by calling **CreateForwardAsync**. Then you can [update the draft](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#UpdateMessagesClient) and [send it](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SendDraftClient).


This example assumes you already [got the Outlook Services client](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetClient) and [got the message ID](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetMessagesClient).

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
IMessage forwardDraft = await outlookClient.Me.Messages[messageId].CreateForwardAsync();

// Get the ID of the draft Forward message.
string forwardMessageId = forwardDraft.Id;
```

<!-- ENDSECTION -->

**CreateForward** creates a draft message with the **IsDraft** and other appropriate properties prepopulated.

****

<a name="UpdateMessages"> </a>
##Update messages

Change writable properties on a message and save the changes.

REST API: [Update a message (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#UpdateAMessage)

Client libraries: [Update a message (Client)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#UpdateAMessageClient)

<a name="UpdateAMessage"></a>
###Update a message (REST)

_Required scope: **Mail.Write**_


Change writable properties on a draft or existing [message](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#MessageResource). Only the properties that you specify are changed.

```no-highlight
PATCH https://outlook.office365.com/api/{version}/me/messages/{message_id}
```

|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|message_id|string|The ID of the message to update.|

Specify one or more writable [message](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#MessageResource) properties in the request body.

```REST
{
    "method": "PATCH",
    "resourceFormat": "https://outlook.office365.com/api/v1.0/me/messages/{message_id}",
    "requestUrl": "https://outlook.office365.com/api/v1.0/me/messages/AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEMAAAmP1Ln1wcHRariNdTMGAO9AAASz8S-AAA=",
    "requestHeaders": {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "Content-Length": 107
    },
    "parameterDetails": [
        {
            "name": "message_id",
            "type": "string",
            "description": "The message ID.",
            "defaultValue": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEMAAAmP1Ln1wcHRariNdTMGAO9AAASz8S-AAA="
        }
    ],
    "requestBody": {
        "Categories": [
            "Orange category",
            "Green category"
        ],
        "IsRead": true
    },
    "statusCode": 200,
    "responseBody": {
        "@odata.context": "https://outlook.office365.com/api/v1.0/$metadata#Me/Messages/$entity",
        "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Messages('AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEMAAAmP1Ln1wcHRariNdTMGAO9AAASz8S-AAA=')",
        "@odata.etag": "W/\"CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0AIP\"",
        "Id": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEMAAAmP1Ln1wcHRariNdTMGAO9AAASz8S-AAA=",
        "ChangeKey": "CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0AIP",
        "Categories": [
            "Orange category",
            "Green category"
        ],
        "DateTimeCreated": "2014-10-17T17:12:15Z",
        "DateTimeLastModified": "2014-10-19T03:24:35Z",
        "Subject": "Meeting notes from today",
        "BodyPreview": "​See attached​",
        "Body": {
            "ContentType": "HTML",
            "Content": "<html>\r\n<head>\r\n<meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n<style type=\"text/css\" style=\"display:none\"><!-- p { margin-top: 0px; margin-bottom: 0px; }--></style>\r\n</head>\r\n<body dir=\"ltr\">\r\n<div id=\"divtagdefaultwrapper\" style=\"color: rgb(0, 0, 0); font-family: Calibri,Arial,Helvetica,sans-serif; font-size: 12pt; background-color: rgb(255, 255, 255);\">\r\n<p>​See attached</p>\r\n</div>\r\n</body>\r\n</html>\r\n"
        },
        "Importance": "Normal",
        "HasAttachments": true,
        "ParentFolderId": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAAAAAEMAAA=",
        "From": {
            "EmailAddress": {
                "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
                "Name": "Alex D"
            }
        },
        "Sender": {
            "EmailAddress": {
                "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
                "Name": "Alex D"
            }
        },
        "ToRecipients": [
            {
                "EmailAddress": {
                    "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com",
                    "Name": "Katie Jordan"
                }
            }
        ],
        "CcRecipients": [],
        "BccRecipients": [],
        "ReplyTo": [],
        "ConversationId": "AAQkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAQANKneItHNYxNlwnKip-qvhs=",
        "DateTimeReceived": "2014-10-17T17:12:15Z",
        "DateTimeSent": "2014-10-17T17:12:12Z",
        "IsDeliveryReceiptRequested": false,
        "IsReadReceiptRequested": false,
        "IsDraft": false,
        "IsRead": true
    }
}
```


 **Response type**

The updated [message](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#MessageResource).

****

<a name="UpdateAMessageClient"> </a>
###Update a message (Client)

Change writable properties on a message, and call **UpdateAsync** to save the changes.


This example assumes you already [got the Outlook Services client](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetClient) and [got the message ID](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetMessagesClient).

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
IMessage message = await outlookClient.Me.Messages[messageId].ExecuteAsync();
message.Body = new ItemBody
{
        Content = "I'm coming out a week earlier."
};
message.ToRecipients.Add(new Recipient
{
    EmailAddress = new EmailAddress
    {
        Address = "garthf@a830edad9050849NDA1.onmicrosoft.com"
    }
});
await message.UpdateAsync();
```

<!-- ENDSECTION -->

You can define multiple updates client-side and send the requests all at once (batch them) by using the following pattern:
1. Call `UpdateAsync(true)` for each entity you want to update. Specifying `true` registers the updates locally on the client but doesn't post them to the server.
2. Call `outlookClient.Context.SaveChangesAsync()` to post all updates that are registered locally.



****


<a name="DeleteMessages"> </a>
##Delete messages

Delete a message.

**Note** Be careful when you delete messages. Deleted contents might not be recoverable.
To learn more, see [Deleting items](http://msdn.microsoft.com/library/c81e3160-e12b-47e0-b3d6-4be28537f301%28Office.15%29.aspx).

REST API: [Delete a message (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#DeleteAMessage)

Client libraries: [Delete a message (Client)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#DeleteMessagesClient)

<a name="DeleteAMessage"></a>
###Delete a message (REST)

_Required scope: **Mail.Write**_

```no-highlight
DELETE https://outlook.office365.com/api/{version}/me/messages/{message_id}
```

|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|message_id|string|The ID of the message to delete.|

```REST
{
    "method": "DELETE",
    "resourceFormat": "https://outlook.office365.com/api/v1.0/me/messages/{message_id}",
    "requestUrl": "https://outlook.office365.com/api/v1.0/me/messages/AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEMAAAmP1Ln1wcHRariNdTMGAO9AAASz8TBAAA=",
    "requestHeaders": {
        "Accept": "application/json"
    },
    "parameterDetails": [
        {
            "name": "message_id",
            "type": "string",
            "description": "The message ID.",
            "defaultValue": "None"
        }
    ],
    "statusCode": 204

}
```

****

<a name="DeleteMessagesClient"> </a>
###Delete a message (Client)

Delete a message by calling **DeleteAsync**.


This example assumes you already [got the Outlook Services client](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetClient) and [got the message ID](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetMessagesClient).

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
IMessage message = await outlookClient.Me.Messages[messageId].ExecuteAsync();
await message.DeleteAsync();
```

<!-- ENDSECTION -->

****


<a name="MoveCopyMessages"> </a>
##Move or copy messages

You can move or copy a message to a folder.

REST API: [Move a message (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#MoveMessage) | [Copy a message (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#CopyMessage)

Client Libraries: [Move or copy a message (Client)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#MoveMessagesClient)

<a name="MoveMessage"> </a>
###Move a message (REST)

_Required scope: **Mail.Write**_

Move a message to a folder. This creates a new copy of the message in the destination folder.

```no-highlight
POST https://outlook.office365.com/api/{version}/me/messages/{message_id}/move
```

|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|message_id|string|The ID of the message to move.|
|_Body parameters_|
|DestinationId|string|The destination folder ID, or the `Inbox`, `Drafts`, `SentItems`, or `DeletedItems` well-known folder name.|

```REST
{
    "method": "POST",
    "resourceFormat": "https://outlook.office365.com/api/v1.0/me/messages/{message_id}/move",
    "requestUrl": "https://outlook.office365.com/api/v1.0/me/messages/AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEJAACd9nJ-tVysQos2hTfspaWRAAADTIy-AAA=/move",
    "requestHeaders": {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "Content-Length": 41
    },
    "parameterDetails": [
        {
            "name": "message_id",
            "type": "string",
            "description": "The ID of the message to move.",
            "defaultValue": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEJAACd9nJ-tVysQos2hTfspaWRAAADTIy-AAA="
        }
    ],
    "requestBody": {
        "DestinationId": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEJAAA="
    },
    "statusCode": 201,
    "responseBody": {
        "@odata.context": "https://outlook.office365.com/api/v1.0/$metadata#Me/Messages/$entity",
        "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Messages('AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEKAAAmP1Ln1wcHRariNdTMGAO9AAASz_vSAAA=')",
                "@odata.etag": "W/\"CQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTJqP\"",
                "Id": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEJAACd9nJ-tVysQos2hTfspaWRAAADTshBhAAA=",
                "ChangeKey": "CQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTJqP",
                "Categories": [],
                "DateTimeCreated": "2014-10-20T00:13:21Z",
                "DateTimeLastModified": "2014-10-20T00:13:23Z",
                "Subject": "Contract Signing",
                "BodyPreview": "There will be a detailed legal review of Project Falcon once the contract is ready.",
                "Body": {
                    "ContentType": "Text",
                    "Content": "There will be a detailed legal review of Project Falcon once the contract is ready."
                },
                "Importance": "Normal",
                "HasAttachments": false,
                "ParentFolderId": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEJAAA=",
                "From": {
                    "EmailAddress": {
                        "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
                        "Name": "Alex D"
                    }
                },
                "Sender": {
                    "EmailAddress": {
                        "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
                        "Name": "Alex D"
                    }
                },
                "ToRecipients": [
                    {
                        "EmailAddress": {
                            "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com",
                            "Name": "Katie Jordan"
                        }
                    },
                    {
                        "EmailAddress": {
                            "Address": "garthf@a830edad9050849NDA1.onmicrosoft.com",
                            "Name": "Garth Fort"
                        }
                    }
                ],
                "CcRecipients": [],
                "BccRecipients": [],
                "ReplyTo": [],
                "ConversationId": "AAQkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAQAKjRc0YJSUBJpofjWgitxag=",
                "DateTimeReceived": "2014-10-20T00:13:21Z",
                "DateTimeSent": "2014-10-20T00:13:21Z",
                "IsDeliveryReceiptRequested": false,
                "IsReadReceiptRequested": false,
                "IsDraft": false,
                "IsRead": true
            }
}
```


 **Response type**

The [message](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#MessageResource) that was moved.

****


<a name="CopyMessage"> </a>
###Copy a message (REST)

_Required scope: **Mail.Write**_

Copy a message to a folder.

```no-highlight
POST https://outlook.office365.com/api/{version}/me/messages/{message_id}/copy
```

|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|message_id|string|The ID of the message to copy.|
|_Body parameters_|
|DestinationId|string|The destination folder ID, or the `Inbox`, `Drafts`, `SentItems`, or `DeletedItems` well-known folder name.|


```REST
{
    "method": "POST",
    "resourceFormat": "https://outlook.office365.com/api/v1.0/me/messages/{message_id}/copy",
    "requestUrl": "https://outlook.office365.com/api/v1.0/me/messages/AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEJAACd9nJ-tVysQos2hTfspaWRAAADTIy-AAA=/copy",
    "requestHeaders": {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "Content-Length": 34
    },
    "parameterDetails": [
        {
            "name": "message_id",
            "type": "string",
            "description": "The ID of the message to move.",
            "defaultValue": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEJAACd9nJ-tVysQos2hTfspaWRAAADTIy-AAA="
        }
    ],
    "requestBody": {
        "DestinationId": "inbox"
    },
    "statusCode": 201,
    "responseBody": {
        "@odata.context": "https://outlook.office365.com/api/v1.0/$metadata#Me/Messages/$entity",
        "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Messages('AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEMAAAmP1Ln1wcHRariNdTMGAO9AAASz8TDAAA=')",
        "@odata.etag": "W/\"CQAAABYAAAAmP1Ln1wcHRariNdTMGAO9AAAS0AIS\"",
        "Id": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEJAACd9nJ-tVysQos2hTfspaWRAAADT8DtAAA=",
        "ChangeKey": "CQAAABYAAACd9nJ/tVysQos2hTfspaWRAAADTJqP",
        "Categories": [],
        "DateTimeCreated": "2014-10-20T00:13:21Z",
        "DateTimeLastModified": "2014-10-20T00:13:23Z",
        "Subject": "Contract Signing",
        "BodyPreview": "There will be a detailed legal review of Project Falcon once the contract is ready.",
        "Body": {
            "ContentType": "Text",
            "Content": "There will be a detailed legal review of Project Falcon once the contract is ready."
        },
        "Importance": "Normal",
        "HasAttachments": false,
        "ParentFolderId": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAAAAAEMAAA=",
        "From": {
            "EmailAddress": {
                "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
                "Name": "Alex D"
            }
        },
        "Sender": {
            "EmailAddress": {
                "Address": "alexd@a830edad9050849NDA1.onmicrosoft.com",
                "Name": "Alex D"
            }
        },
        "ToRecipients": [
            {
                "EmailAddress": {
                    "Address": "katiej@a830edad9050849NDA1.onmicrosoft.com",
                    "Name": "Katie Jordan"
                }
            },
            {
                "EmailAddress": {
                    "Address": "garthf@a830edad9050849NDA1.onmicrosoft.com",
                    "Name": "Garth Fort"
                }
            }
        ],
        "CcRecipients": [],
        "BccRecipients": [],
        "ReplyTo": [],
        "ConversationId": "AAQkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAQAKjRc0YJSUBJpofjWgitxag=",
        "DateTimeReceived": "2014-10-20T00:13:21Z",
        "DateTimeSent": "2014-10-20T00:13:21Z",
        "IsDeliveryReceiptRequested": false,
        "IsReadReceiptRequested": false,
        "IsDraft": false,
        "IsRead": true
    }
}
```

 **Response type**

The new copy of the [message](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#MessageResource).

****

<a name="MoveMessagesClient"> </a>
### Move or copy a message (Client)

Move a or copy a message by passing the ID of the destination folder to the **MoveAsync** or **CopyAsync** method.


This example assumes you already [got the Outlook Services client](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetClient), [got the message ID](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetMessagesClient), and [got the desination folder ID](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetFoldersClient).

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
IMessage messageToMove = await outlookClient.Me.Messages[messageId].ExecuteAsync();
await messageToMove.MoveAsync("AAMkADE3N...");
```

<!-- ENDSECTION -->

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
IMessage messageToCopy = await outlookClient.Me.Messages[messageId].ExecuteAsync();
await messageToCopy.CopyAsync("Inbox");
```

<!-- ENDSECTION -->

****

<a name="GetAttachments"> </a>
##Get attachments

You can get an attachment collection or get an attachment.

REST API: [Get an attachment collection (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetAttachmentCollection) | [Get an attachment (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetAttachment)

Client libraries: [Get one or more message attachments (Client)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetAttachmentsClient)


<a name="GetAttachmentCollection"> </a>
###Get an attachment collection (REST)

_Required scope: **Mail.Read** or **Mail.Write**_

Get the attachments from a particular message.

```no-highlight
GET https://outlook.office365.com/api/{version}/me/messages/{message_id}/attachments
```


|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|message_id|string|The message ID.|

**Note** By default, each attachment in the response includes all its properties corresponding to that attachment type. Use **$select** to specify only those properties you need for
best performance. The **Id** property is always returned. See [OData query parameters](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#OdataQueryParams) for filtering, sorting, and paging parameters.

The following example shows how to use **$select** to specify returning only the **Name** property of each file attachment in the response. Refer to the sample response in [Get an attachment (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetAttachment) 
for a full list of properties that would be returned for an attachment if you don't use **$select**.

```REST-i
{
    "method": "GET",
    "resourceFormat": "https://outlook.office365.com/api/v1.0/me/messages/{message_id}/attachments?$select=Name",
    "requestUrl": "https://outlook.office365.com/api/v1.0/me/messages/AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEMAACd9nJ-tVysQos2hTfspaWRAAADTHVSAAA=/attachments?$select=Name",
    "requestHeaders": {
        "Accept": "application/json"
    },
    "parameterDetails": [
        {
            "name": "message_id",
            "type": "string",
            "description": "The message ID.",
            "defaultValue": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEMAACd9nJ-tVysQos2hTfspaWRAAADTHVSAAA="
        }
    ],
    "statusCode": 200,
    "responseBody": {
        "@odata.context": "https://outlook.office365.com/api/v1.0/$metadata#Me/Messages('AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEMAACd9nJ-tVysQos2hTfspaWRAAADTHVSAAA%3D')/Attachments",
        "value": [
            {
                "@odata.type": "#Microsoft.OutlookServices.FileAttachment",
                "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Messages('AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEMAACd9nJ-tVysQos2hTfspaWRAAADTHVSAAA=')/Attachments('AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEMAACd9nJ-tVysQos2hTfspaWRAAADTHVSAAABEgAQANQEWV4bM8FIlPwxj4kShdM=')",
                "Id": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEMAACd9nJ-tVysQos2hTfspaWRAAADTHVSAAABEgAQANQEWV4bM8FIlPwxj4kShdM=",
                "Name": "minutes.docx",
                "ContentType": "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
                "Size": 11585,
                "IsInline": false,
                "DateTimeLastModified": "2014-10-20T00:41:52Z",
                "ContentId": null,
                "ContentLocation": null,
                "IsContactPhoto": false,
                "ContentBytes": "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"
            }
        ]
    }
}
```

 **Response type**

The [file attachment](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#FileAttachmentResource) or [item attachment](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#ItemAttachmentResource) collection for the message.

****


<a name="GetAttachment"> </a>
###Get an attachment (REST)

_Required scope: **Mail.Read** or **Mail.Write**_

Get an attachment from a particular message.

```no-highlight
GET https://outlook.office365.com/api/{version}/me/messages/{message_id}/attachments/{attachment_id}
```

**Note** See [OData query parameters](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#OdataQueryParams) for filtering, sorting, and paging parameters.

|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|message_id|string|The message ID.|
|attachment_id|string|The attachment ID.|

```REST-i
{
    "method": "GET",
    "resourceFormat": "https://outlook.office365.com/api/v1.0/me/messages/{message_id}/attachments/{attachment_id}",
    "requestUrl": "https://outlook.office365.com/api/v1.0/me/messages/AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEMAACd9nJ-tVysQos2hTfspaWRAAADTHVSAAA=/attachments/AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEMAACd9nJ-tVysQos2hTfspaWRAAADTHVSAAABEgAQANQEWV4bM8FIlPwxj4kShdM=",
    "requestHeaders": {
        "Accept": "application/json"
    },
    "parameterDetails": [
        {
            "name": "message_id",
            "type": "string",
            "description": "The message ID.",
            "defaultValue": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEMAACd9nJ-tVysQos2hTfspaWRAAADTHVSAAA="
        },
        {
            "name": "attachment_id",
            "type": "string",
            "description": "The attachment ID.",
            "defaultValue": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEMAACd9nJ-tVysQos2hTfspaWRAAADTHVSAAABEgAQANQEWV4bM8FIlPwxj4kShdM="
        }
    ],
    "statusCode": 200,
    "responseBody": {
        "@odata.context": "https://outlook.office365.com/api/v1.0/$metadata#Me/Messages('AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEMAACd9nJ-tVysQos2hTfspaWRAAADTHVSAAA%3D')/Attachments/$entity",
        "@odata.type": "#Microsoft.OutlookServices.FileAttachment",
        "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Messages('AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEMAACd9nJ-tVysQos2hTfspaWRAAADTHVSAAA=')/Attachments('AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEMAACd9nJ-tVysQos2hTfspaWRAAADTHVSAAABEgAQANQEWV4bM8FIlPwxj4kShdM=')",
        "Id": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQBGAAAAAADUuTJK1K9aTpCdqXop_4NaBwCd9nJ-tVysQos2hTfspaWRAAAAAAEMAACd9nJ-tVysQos2hTfspaWRAAADTHVSAAABEgAQANQEWV4bM8FIlPwxj4kShdM=",
        "Name": "minutes.docx",
        "ContentType": "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
        "Size": 11585,
        "IsInline": false,
        "DateTimeLastModified": "2014-10-20T00:41:52Z",
        "ContentId": null,
        "ContentLocation": null,
        "IsContactPhoto": false,
        "ContentBytes": "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"
    }
}
```

 **Response type**

The requested [file attachment](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#FileAttachmentResource) or [item attachment](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#ItemAttachmentResource).

**Note** By default, the response includes all the properties of the attachment. Use **$select** to specify only those properties you need for
best performance. Refer to [Get an attachment collection (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetAttachmentCollection) for an example.  
The **Id** property is always returned. See [OData query parameters](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#OdataQueryParams) for filtering, sorting, and paging parameters.


****

<a name="GetAttachmentsClient"> </a>
###Get one or more message attachments (Client)

Get attachments on message by calling the **Attachments** property. To get a particular attachment, specify the attachment ID as the index of the **Attachments**
 collection or use the **GetById** method.

**Note** Attachment collections support query expressions such as **Select**, **OrderBy**, and **Take**.


This example calls the method that [creates the Outlook Services client](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetClient). The .NET code assumes you already [got the message ID](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetMessagesClient).

<!-- BEGINSECTION class="tabbedCodeSnippets" data-resources="OutlookServices.Mail" -->

<!--tested-->
```cs-i
var outlookClient = await CreateOutlookClientAsync("Mail");
var messages = await outlookClient.Me.Folders["Inbox"].Messages
  .Where(m => m.HasAttachments == true)
  .Expand(m => m.Attachments)
  .Take(10)
  .ExecuteAsync();

foreach (var message in messages.CurrentPage)
{
  var attachments = message.Attachments.CurrentPage;
  System.Diagnostics.Debug.WriteLine("Message '{0}' contains '{1}' attachment(s).",
    message.Subject,
    attachments.Count);

  foreach (var attachment in attachments)
  {
    System.Diagnostics.Debug.WriteLine("*    '{0}' ({1} KB)",
      attachment.Name,
      attachment.Size);
  }
}

```
```javascript-i
outlookClient.me.folders.getFolder('Inbox').messages.getMessages().filter('HasAttachments eq true').fetchAll(10).then(function (result) {
    result.forEach(function (message) {
        message.attachments.getAttachments().fetchAll(100).then(function (attachmentResult) {
            console.log('Message "' + message.subject + '" contains ' + attachmentResult.length + ' attachment(s)');
            attachmentResult.forEach(function (attachment) {
                console.log('*    "' + attachment.name + '" (' + attachment.size + ' KB)');
            });
        });
    }, function (error) {
        console.log(error);
    });
}, function (error) {
    console.log(error);
});
```

<!-- ENDSECTION -->

****

<a name="CreateAttachments"> </a>
##Create attachments
You can create a file attachment or [create an item attachment](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#CreateItemAttachment) for a message.

REST API: [Create a file attachment (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#CreateFileAttachment) | [Create an item attachment (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#CreateItemAttachment)

<a name="CreateFileAttachment"></a>
###Create a file attachment (REST)

_Required scope: **Mail.Read** or **Mail.Write**_

Add a file attachment to a message.


```no-highlight
POST https://outlook.office365.com/api/{version}/me/messages/{message_id}/attachments
```

|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|message_id|string|The message ID.|
|_Body parameters_|
|@odata.type|string|```#Microsoft.OutlookServices.FileAttachment```|
|Name|string|The name of the attachment.|
|ContentBytes|binary|The file to attach.|

Specify the **Name** and **ContentBytes** parameters and any writable [file attachment](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#FileAttachmentResource) properties in the request body.


<!--comment out until I get a working request body
```REST
{
    //placeholder text
}

```
-->

 **Response type**

The new [file attachment](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#FileAttachmentResource).

****


<a name="CreateItemAttachment"></a>
###Create an item attachment (REST)

_Required scope: **Mail.Read** or **Mail.Write**_

Add an item attachment to a message.

```no-highlight
POST https://outlook.office365.com/api/{version}/me/messages/{message_id}/attachments
```

|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|message_id|string|The message ID.|
|_Body parameters_|
|@odata.type|string|```#Microsoft.OutlookServices.ItemAttachment```|
|Name|string|The name of the attachment.|
|Item|A [Message](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#MessageResource) or [Event](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#EventResource) entity.|The item to attach.|

Specify the **Name** and **Item** parameters and any writable [item attachment](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#ItemAttachmentResource) properties in the request body.

<!--comment out until I get a working request body
```REST
{
    //placeholder text
}

```
-->

 **Response type**

The new [item attachment](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#ItemAttachmentResource).

<!--
<a name="AddAttachments"> </a>
## Add attachments

Add a **FileAttachment** or an **ItemAttachment** to a message.

Updating attachments is not supported. Instead, delete the existing attachment and add a new one with updated values.

### Add file attachments

Add a file attachment to a message by getting the file and calling **AddAttachmentAsync**.

This example assumes you already [got the Outlook Services client](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetClient) and [got the message ID](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetMessages).



```cs
    FileAttachment newFileAttachment = new FileAttachment
    {
        ContentBytes = System.IO.File.ReadAllBytes(@"C:\Attachments\capture.png"),
        Name = "capture.png"
    };
    await outlookClient.Me.Messages[messageId].Attachments.AddAttachmentAsync(newFileAttachment);
```



### Add item attachments

Add message attachment to a message by getting the message and calling **AddAttachmentAsync**.

This example assumes you already [got the Outlook Services client](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetClient) and [got the message ID](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetMessages) of the message to add the attachment to and the message to attach.



```cs

    IMessage messageToAttach = await outlookClient.Me.Messages[messageId].ExecuteAsync();
    ItemAttachment newItemAttachment = new ItemAttachment
    {
        Item = (Message)messageToAttach
    };
    await outlookClient.Me.Messages[messageId].Attachments.AddAttachmentAsync(newItemAttachment);
```
-->

****


<a name="DeleteAttachments"> </a>
##Delete attachments

_Required scope: **Mail.Read** or **Mail.Write**_

Delete a message attachment.

```no-highlight
DELETE https://outlook.office365.com/api/{version}/me/messages/{message_id}/attachments/{attachment_id}
```

|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|message_id|string|The message ID.|
|attachment_id|string|The attachment ID.|

```REST
{
  "method": "DELETE", 
  "resourceFormat": "https://outlook.office365.com/api/v1.0/me/messages/{message_id}/attachments/{attachment_id}", 
  "requestUrl": "https://outlook.office365.com/api/v1.0/me/messages/AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEMAAAmP1Ln1wcHRariNdTMGAO9AAASz8S-AAA=/attachments/AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEMAAAmP1Ln1wcHRariNdTMGAO9AAASz8S-AAABEgAQAG1aLWLNI65Iu36Ng67gL7o=", 
  "requestHeaders": { 
    "Accept": "application/json"
  }, 
  "parameterDetails": [ 
    { 
      "name": "message_id",
      "type": "string", 
      "description": "The message ID.", 
      "defaultValue": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEMAAAmP1Ln1wcHRariNdTMGAO9AAASz8S-AAA="
    },
    { 
      "name": "attachment_id",
      "type": "string",
      "description": "The attachment ID.", 
      "defaultValue": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MABGAAAAAAC_0WfqSjt_SqLtNkuO-bj1BwAmP1Ln1wcHRariNdTMGAO9AAAAAAEMAAAmP1Ln1wcHRariNdTMGAO9AAASz8S-AAABEgAQAG1aLWLNI65Iu36Ng67gL7o="
    }
  ], 
  "statusCode": 204 

}


```

<!--
<a name="DeleteAttachments"> </a>
## Delete attachments

You can delete an attachment by calling **DeleteAsync** on a **FileAttachment** or **ItemAttachment**.


This example assumes you already [got the Outlook Services client](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetClient) and [got the message ID](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetMessages).



```cs

    //No ToFileAttachment or Attachment.ExecuteAsync
    Attachment attachmentToDelete = await outlookClient.Me.Messages[messageId].
        Attachments[attachmentId].ToFileAttachment().ExecuteAsync();
    await attachmentToDelete.DeleteAsync();

    IMessage message = await outlookClient.Me.Messages[messageId].ExecuteAsync();
    IPagedCollection<IAttachment> attachmentsResults = message.Attachments;
    List<IAttachment> attachmentsPage = attachmentsResults.CurrentPage.ToList();

    // Delete the first attachment in the collection.
    Attachment attachmentToDelete = (Attachment)attachmentsPage[0];
    await attachmentToDelete.DeleteAsync();
```
-->



****


<a name="GetFolders"> </a>
##Get folders

You can get a folder collection or get a folder in the user's mailbox.

REST API: [Get a folder collection (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetFolderCollection) | [Get a folder (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetFolder)

Client libraries: [Get a folder or folder collection (Client)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetFoldersClient)


<a name="GetFolderCollection"> </a>
###Get a folder collection (REST)

_Required scope: **Mail.Read** or **Mail.Write**_

Get a folder collection. You can use the `.../me/folders` shortcut to get the top-level folder collection or navigate to another folder.

```no-highlight
GET https://outlook.office365.com/api/{version}/me/folders
GET https://outlook.office365.com/api/{version}/me/folders/{folder_id}/childfolders
```

**Note** See [OData query parameters](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#OdataQueryParams) for filtering, sorting, and paging parameters.

|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|folder_id|string|The folder ID, or the `Inbox`, `Drafts`, `SentItems`, or `DeletedItems` well-known folder name, if you're getting folders from a specific folder.|

```REST-i
{
    "method": "GET",
    "resourceFormat": "https://outlook.office365.com/api/v1.0/me/folders",
    "requestUrl": "https://outlook.office365.com/api/v1.0/me/folders",
    "requestHeaders": {
        "Accept": "application/json"
    },
    "statusCode": 200,
    "responseBody": {
        "@odata.context": "https://outlook.office365.com/api/v1.0/$metadata#Me/Folders",
        "value": [
            {
                "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Folders('AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEfAAA=')",
                "Id": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEfAAA=",
                "ParentFolderId": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEIAAA=",
                "DisplayName": "Conversation Action Settings",
                "ChildFolderCount": 0
            },
            {
                "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Folders('AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEKAAA=')",
                "Id": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEKAAA=",
                "ParentFolderId": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEIAAA=",
                "DisplayName": "Deleted Items",
                "ChildFolderCount": 0
            },
            {
                "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Folders('AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEPAAA=')",
                "Id": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEPAAA=",
                "ParentFolderId": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEIAAA=",
                "DisplayName": "Drafts",
                "ChildFolderCount": 0
            },
            {
                "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Folders('AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEMAAA=')",
                "Id": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEMAAA=",
                "ParentFolderId": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEIAAA=",
                "DisplayName": "Inbox",
                "ChildFolderCount": 0
            },
            {
                "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Folders('AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEQAAA=')",
                "Id": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEQAAA=",
                "ParentFolderId": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEIAAA=",
                "DisplayName": "Journal",
                "ChildFolderCount": 0
            },
            {
                "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Folders('AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEeAAA=')",
                "Id": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEeAAA=",
                "ParentFolderId": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEIAAA=",
                "DisplayName": "Junk Email",
                "ChildFolderCount": 0
            },
            {
                "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Folders('AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAERAAA=')",
                "Id": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAERAAA=",
                "ParentFolderId": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEIAAA=",
                "DisplayName": "Notes",
                "ChildFolderCount": 0
            },
            {
                "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Folders('AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAELAAA=')",
                "Id": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAELAAA=",
                "ParentFolderId": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEIAAA=",
                "DisplayName": "Outbox",
                "ChildFolderCount": 0
            }
        ]
    }
}
```

 **Response type**

The requested [folder](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#FolderResource) collection.

****


<a name="GetFolder"> </a>
###Get a folder (REST)

_Required scope: **Mail.Read** or **Mail.Write**_

Get a folder by ID.

```no-highlight
GET https://outlook.office365.com/api/{version}/me/folders/{folder_id}
```

**Note** See [OData query parameters](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#OdataQueryParams) for filtering, sorting, and paging parameters.

|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|folder_id|string|The folder ID, or the `Inbox`, `Drafts`, `SentItems`, or `DeletedItems` well-known folder name.|

```REST-i
{
    "method": "GET",
    "resourceFormat": "https://outlook.office365.com/api/v1.0/me/folders/{folder_id}",
    "requestUrl": "https://outlook.office365.com/api/v1.0/me/folders/sentitems",
    "requestHeaders": {
        "Accept": "application/json"
    },
    "parameterDetails": [
        {
            "name": "folder_id",
            "type": "string",
            "description": "The folder ID or well-known name: Inbox, SentItems, Drafts, or DeletedItems.",
            "defaultValue": "sentitems"
        }
    ],
    "statusCode": 200,
    "responseBody": {
        "@odata.context": "https://outlook.office365.com/api/v1.0/$metadata#Me/Folders/$entity",
        "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Folders('AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEJAAA=')",
        "Id": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEJAAA=",
        "ParentFolderId": "AAMkAGI2NGVhZTVlLTI1OGMtNDI4My1iZmE5LTA5OGJiZGEzMTc0YQAuAAAAAADUuTJK1K9aTpCdqXop_4NaAQCd9nJ-tVysQos2hTfspaWRAAAAAAEIAAA=",
        "DisplayName": "Sent Items",
        "ChildFolderCount": 0
    }
}
```

 **Response type**

The requested [folder](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#FolderResource).

****

<a name="GetFoldersClient"> </a>
###Get a folder or folder collection (Client)

Get the top-level folders in the mailbox by using the `Me.Folders` shortcut property. To get the folders from a specific folder, use its **ChildFolders** property.
 You can use the following well-known folder names instead of the ID for the corresponding folder: `Inbox`, `SentItems`, `Drafts`, `DeletedItems`.

Example: `outlookClient.Me.Folders["Drafts"].ChildFolders.ExecuteAsync()`

To get a particular folder, specify the folder ID as the index of the **Folders** collection or use the **GetById** method.

**Note** Folder collections support query expressions such as **Select**, **OrderBy**, and **Take**.

This example calls the method that [gets the Outlook Services client](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetClient).

<!-- BEGINSECTION class="tabbedCodeSnippets" data-resources="OutlookServices.Mail" -->

<!--tested-->
```cs-i
var outlookClient = await CreateOutlookClientAsync("Mail");
var mailFolders = await outlookClient.Me.Folders
  .Take(10)
  .ExecuteAsync();

foreach(var mailFolder in mailFolders.CurrentPage)
{
  System.Diagnostics.Debug.WriteLine("Mail folder '{0}'.", mailFolder.DisplayName);
}

```
```javascript-i
outlookClient.me.folders.getFolders().fetchAll(100).then(function (result) {
    result.forEach(function (folder) {
        console.log('Folder "' + folder.displayName + '"');
    });
}, function (error) {
    console.log(error);
});
```

<!-- ENDSECTION -->


****

<a name="CreateFolders"> </a>
##Create folders

Add a new folder to a folder collection.

REST API: [Create a folder (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#CreateAFolder)

Client libraries: [Create a folder (Client)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#CreateFoldersClient)

<a name="CreateAFolder"> </a>
###Create a folder (REST)

_Required scope: **Mail.Write**_

Create a child folder by the name specified in **DisplayName**. **DisplayName** is the only writable property for a [folder](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#FolderResource).

```no-highlight
POST https://outlook.office365.com/api/{version}/me/folders/{folder_id}/childfolders
```

|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|folder_id|string|The folder ID, or the `Inbox`, `Drafts`, `SentItems`, or `DeletedItems` well-known folder name.|
|_Body parameters_|
|DisplayName|string|The display name of the folder.|

```REST
{
    "method": "POST",
    "resourceFormat": "https://outlook.office365.com/api/v1.0/me/folders/{folder_id}/childfolders",
    "requestUrl": "https://outlook.office365.com/api/v1.0/me/folders/inbox/childfolders",
    "requestHeaders": {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "Content-Length": 34
    },
    "parameterDetails": [
        {
            "name": "folder_id",
            "type": "string",
            "description": "The folder ID or well-known name: Inbox, SentItems, Drafts, or DeletedItems.",
            "defaultValue": "inbox"
        }
    ],
    "requestBody": {
        "DisplayName": "Company"
    },
    "statusCode": 201,
    "responseBody": {
        "@odata.context": "https://outlook.office365.com/api/v1.0/$metadata#Me/Folders('inbox')/ChildFolders/$entity",
        "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Folders('AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAASz-l_AAA=')",
        "Id": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAASz-l_AAA=",
        "ParentFolderId": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAAAAAEMAAA=",
        "DisplayName": "Company",
        "ChildFolderCount": 0
    }
}
```

 **Response type**

The new [folder](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#FolderResource).

**Remarks**

You can't create a top-level folder. You can only add a folder to a `childfolders` endpoint.

****

<a name="CreateFoldersClient"> </a>
###Create a folder (Client)

Create a **Folder** object and pass it to the **AddFolderAsync** method on the destination folder's **ChildFolders** collection.

This example assumes you already [got the Outlook Services client](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetClient) and [got the folder ID](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetFolders) of the parent folder.


<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
Folder newFolder = new Folder
{
    DisplayName = "Private"
};
await outlookClient.Me.Folders["Inbox"].ChildFolders.AddFolderAsync(newFolder);

// Get the ID of the new folder.
string folderId = newFolder.Id;
```

<!-- ENDSECTION -->


****


<a name="UpdateFolders"> </a>
##Update folders

Change a folder name.

REST API: [Update a folder (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#UpdateAFolder)

Client libraries: [Update a folder (Client)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#UpdateFoldersClient)

<a name="UpdateAFolder"> </a>
###Update a folder (REST)

_Required scope: **Mail.Write**_

Change the folder name to that specified in **DisplayName**. The name is the only writable property for a [folder](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#FolderResource).

```no-highlight
PATCH https://outlook.office365.com/api/{version}/me/folders/{folder_id}
```

|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|folder_id|string|The folder ID, or the `Inbox`, `Drafts`, `SentItems`, or `DeletedItems` well-known folder name.|
|_Body parameters_|
|DisplayName|string|The new display name of the folder.|

```REST
{
    "method": "PATCH",
    "resourceFormat": "https://outlook.office365.com/api/v1.0/me/folders/{folder_id}",
    "requestUrl": "https://outlook.office365.com/api/v1.0/me/folders/AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAASz-l_AAA=",
    "requestHeaders": {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "Content-Length": 34
    },
    "parameterDetails": [
        {
            "name": "folder_id",
            "type": "string",
            "description": "The folder ID or well-known name: Inbox, SentItems, Drafts, or DeletedItems.",
            "defaultValue": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAASz-l_AAA="
        }
    ],
    "requestBody": {
        "DisplayName": "Business"
    },
    "statusCode": 200,
    "responseBody": {
        "@odata.context": "https://outlook.office365.com/api/v1.0/$metadata#Me/Folders/$entity",
        "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Folders('AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAASz-l_AAA=')",
        "Id": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAASz-l_AAA=",
        "ParentFolderId": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAAAAAEMAAA=",
        "DisplayName": "Business",
        "ChildFolderCount": 0
    }
}

```

 **Response type**

The updated [folder](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#FolderResource).

****

<a name="UpdateFoldersClient"> </a>
###Update a folder (Client)

Change a folder name. **DisplayName** is the only writeable property for a folder.

This example assumes you already [got the Outlook Services client](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetClient) and [got the folder ID](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetFoldersClient).


<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
IFolder folder = await outlookClient.Me.Folders[folderId].ExecuteAsync();
folder.DisplayName = "Personal";
await folder.UpdateAsync();

// Get the updated property.
string updatedName = folder.DisplayName;
```

<!-- ENDSECTION -->

You can define multiple updates client-side and send the requests all at once (batch them) by using the following pattern:
1. Call `UpdateAsync(true)` for each entity you want to update. Specifying `true` registers the updates locally on the client but doesn't post them to the server.
2. Call `outlookClient.Context.SaveChangesAsync()` to post all updates that are registered locally.

****


<a name="DeleteFolders"> </a>
##Delete folders

Delete a folder and all of its contents.

**Note** Be careful when you delete folders. Deleted contents might not be recoverable.
To learn more, see [Deleting items](http://msdn.microsoft.com/library/c81e3160-e12b-47e0-b3d6-4be28537f301%28Office.15%29.aspx).

REST API: [Delete a folder (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#DeleteAFolder)

Client libraries: [Delete a folder (Client)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#DeleteFoldersClient)

<a name="DeleteAFolder"> </a>
###Delete a folder (REST)

_Required scope: **Mail.Write**_

Delete the folder specified in **folder_id**.

```no-highlight
DELETE https://outlook.office365.com/api/{version}/me/folders/{folder_id}
```

|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|folder_id|string|The folder ID, or the `Inbox`, `Drafts`, `SentItems`, or `DeletedItems` well-known folder name.|

```REST
{
    "method": "DELETE",
    "resourceFormat": "https://outlook.office365.com/api/v1.0/me/folders/{folder_id}",
    "requestUrl": "https://outlook.office365.com/api/v1.0/me/folders/AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAASz-l_AAA=",
    "requestHeaders": {
        "Accept": "application/json"
    },
    "parameterDetails": [
        {
            "name": "folder_id",
            "type": "string",
            "description": "The folder ID.",
            "defaultValue": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAASz-l_AAA="
        }
    ],
    "statusCode": 204

}

```

****

<a name="DeleteFoldersClient"> </a>
###Delete a folder (Client)

Delete a folder by its ID and calling **DeleteAsync**.

This example assumes you already [got the Outlook Services client](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetClient) and [got the folder ID](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetFolders).


<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
IFolder folder = await outlookClient.Me.Folders[folderId].ExecuteAsync();
await folder.DeleteAsync();
```

<!-- ENDSECTION -->

****

<a name="MoveCopyFolders"> </a>
##Move or copy folders
You can move or copy a folder to another folder.

REST API: [Move a folder (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#MoveFolders) | [Copy a folder (REST)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#CopyFolders)

Client libraries: [Move or copy a folder (Client)](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#MoveFoldersClient)

<a name="MoveFolders"> </a>
###Move a folder (REST)

_Required scope: **Mail.Write**_

Move a folder and its contents to another folder by using the **Move** method.

```no-highlight
POST https://outlook.office365.com/api/{version}/me/folders/{folder_id}/move
```

|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|folder_id|string|The folder ID, or the `Inbox`, `Drafts`, `SentItems`, or `DeletedItems` well-known folder name.|
|_Body parameters_|
|DestinationId|string|The destination folder ID, or the `Inbox`, `Drafts`, `SentItems`, or `DeletedItems` well-known folder name.|

```REST
{
    "method": "POST",
    "resourceFormat": "https://outlook.office365.com/api/v1.0/me/folders/{folder_id}/move",
    "requestUrl": "https://outlook.office365.com/api/v1.0/me/folders/AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAASz-l_AAA=/move",
    "requestHeaders": {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "Content-Length": 149
    },
    "parameterDetails": [
        {
            "name": "folder_id",
            "type": "string",
            "description": "The folder ID or well-known name: Inbox, SentItems, Drafts, or DeletedItems.",
            "defaultValue": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAASz-l_AAA="
        }
    ],
    "requestBody": {
        "DestinationId": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAAOyxQ9AAA="
    },
    "statusCode": 201,
    "responseBody": {
        "@odata.context": "https://outlook.office365.com/api/v1.0/$metadata#Me/Folders/$entity",
        "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Folders('AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAASz-l_AAA=')",
        "Id": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAASz-l_AAA=",
        "ParentFolderId": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAAOyxQ9AAA=",
        "DisplayName": "Business",
        "ChildFolderCount": 0
    }
}
```

 **Response type**

The [folder](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#FolderResource) that was moved.

****


<a name="CopyFolders"> </a>
###Copy a folder (REST)

_Required scope: **Mail.Write**_

Copy a folder and its contents to another folder by using the **Copy** method.

```no-highlight
POST https://outlook.office365.com/api/{version}/me/folders/{folder_id}/copy
```

|**Required parameter**|**Type**|**Description**|
|:-----|:-----|:-----|
|_URL parameters_|
|version|string|The [version](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#SupportedVersions) of the API.|
|folder_id|string|The folder ID, or the `Inbox`, `Drafts`, `SentItems`, or `DeletedItems` well-known folder name.|
|_Body parameters_|
|DestinationId|string|The destination folder ID, or the `Inbox`, `Drafts`, `SentItems`, or `DeletedItems` well-known folder name.|

```REST
{
    "method": "POST",
    "resourceFormat": "https://outlook.office365.com/api/v1.0/me/folders/{folder_id}/copy",
    "requestUrl": "https://outlook.office365.com/api/v1.0/me/folders/AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAASz-l_AAA=/copy",
    "requestHeaders": {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "Content-Length": 34
    },
    "parameterDetails": [
        {
            "name": "folder_id",
            "type": "string",
            "description": "The folder ID or well-known name: Inbox, SentItems, Drafts, or DeletedItems.",
            "defaultValue": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAASz-l_AAA="
        }
    ],
    "requestBody": {
        "DestinationId": "inbox"
    },
    "statusCode": 201,
    "responseBody": {
        "@odata.context": "https://outlook.office365.com/api/v1.0/$metadata#Me/Folders/$entity",
        "@odata.id": "https://outlook.office365.com/api/v1.0/Users('alexd@a830edad9050849NDA1.onmicrosoft.com')/Folders('AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAASz-mAAAA=')",
        "Id": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAASz-mAAAA=",
        "ParentFolderId": "AAMkAGE0MGM1Y2M5LWEzMmUtNGVlNy05MjRlLTk0YmJjYzVkN2I5MAAuAAAAAAC_0WfqSjt_SqLtNkuO-bj1AQAmP1Ln1wcHRariNdTMGAO9AAAAAAEMAAA=",
        "DisplayName": "Business",
        "ChildFolderCount": 0
    }
}
```

 **Response type**

The new copy of the [folder](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar#FolderResource).

****

<a name="MoveFoldersClient"> </a>
###Move or copy a folder (Client)

Move or copy a folder by passing the ID of the destination folder to the **MoveAsync** or **CopyAsync** method.

This example assumes you already [got the Outlook Services client](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetClient) and [got the folder ID](https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#GetFoldersClient) of the folder to move and the destination folder.

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

<!--tested-->
```cs
IFolder folder = await outlookClient.Me.Folders[folderId].ExecuteAsync();
await folder.MoveAsync("AAMkADE3N...");
```

<!-- ENDSECTION -->

<!-- BEGINSECTION class="tabbedCodeSnippets" -->

```cs
IFolder folder = await outlookClient.Me.Folders[folderId].ExecuteAsync();
await folder.CopyAsync("Inbox");
```

<!-- ENDSECTION -->

****

<a name="NextSteps"> </a>
## Next steps

Whether you're ready to start building an app or just want to learn more, we've got you covered.


- Ready to get started? Start by [setting up your environment](https://msdn.microsoft.com/office/office365/HowTo/setup-development-environment), then check out our [first app walkthrough](https://msdn.microsoft.com/office/office365/HowTo/getting-started-Office-365-APIs).

- Explore the Office 365 APIs using the interactive [API Sandbox](http://apisandbox.msdn.microsoft.com/).

- Want samples? [We've got them](https://msdn.microsoft.com/office/office365/HowTo/starter-projects-and-code-samples).


Or, learn more about using the Office 365 platform:

- [Overview of developing on the Office 365 platform](https://msdn.microsoft.com/office/office365/HowTo/platform-development-overview)

- [Office 365 app authentication and resource authorization](https://msdn.microsoft.com/office/office365/HowTo/common-app-authentication-tasks)

- [Manually register your app with Azure AD so it can access Office 365 APIs](https://msdn.microsoft.com/office/office365/HowTo/add-common-consent-manually)

- [Calendar API reference](https://msdn.microsoft.com/office/office365/APi/calendar-rest-operations)

- [Contacts API reference](https://msdn.microsoft.com/office/office365/APi/contacts-rest-operations)

- [Files API reference](https://msdn.microsoft.com/office/office365/APi/files-rest-operations)

- [Discovery Service REST API operations reference](https://msdn.microsoft.com/office/office365/APi/discovery-service-rest-operations)

- [Resource reference for the Mail, Calendar, and Contacts REST APIs](https://msdn.microsoft.com/office/office365/APi/complex-types-for-mail-contacts-calendar)
