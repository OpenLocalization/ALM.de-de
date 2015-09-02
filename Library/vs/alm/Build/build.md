Title:Build reference
Description: ALM
ms.ContentId: 49B96A48-F77B-48B5-B41F-8A8BC336297A
ms.topic: build reference (API)

#Build reference

##Build reference

TBD

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