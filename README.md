## CyberArk REST API

All available requests in CyberArk Privileged Account Security Web Services

# THIS IS UNOFFICIAL DOCUMENTATION 

Happy automating!

## Getting Started Guide

[Getting Started with REST Using Postman](https://github.com/infamousjoeg/CyberArk-RESTAPI/blob/master/Getting%20Started%20with%20REST%20Using%20Postman.pdf) (PDF)

## Community Tools

*   [psPAS](https://github.com/pspete/psPAS) - PowerShell Module for CyberArk's REST API
*   [CredentialRetriever](https://github.com/pspete/CredentialRetriever) - PowerShell Module for CyberArk's Application Access Manager (AAM)
*   [pyAIM](https://github.com/infamousjoeg/pyAIM) - Python Client Library for CyberArk's Application Access Manager (AAM)
    

## Code Examples

*   [cyberark/epv-api-scripts](https://github.com/cyberark/epv-api-scripts)
*   [infamousjoeg on GitHub](https://github.com/infamousjoeg?tab=repositories)
*   [CyberArk's Automation Greatest Hits (Awesome List of Automation)](https://cybr.rocks/greatesthits)
    

## YouTube Videos Playlist

*   [CyberArk Videos Playlist Curated by InfamousJoeG](https://www.youtube.com/playlist?list=PL-p_9AwMQDmkS6rCXQrINn0Xc7dv73dWU)
    

## Maintainer

[Joe Garcia](https://github.com/infamousjoeg)

[Buy me a coffee](https://www.buymeacoffee.com/infamousjoeg)

## Status Codes

| Status Name | Status Code | Status Description |
| --- | --- | --- |
| Success | 200 | The request succeeded. The actual response will depend on the request method used. |
| Created | 201 | The request was fulfilled and resulted in a new resource being created. |
| Bad Request | 400 | The request could not be understood by the server due to incorrect syntax. |
| Unauthorized | 401 | The request requires user authentication. |
| Forbidden | 403 | The server received and understood the request, but will not fulfill it. Authorization will not help and the request MUST NOT be repeated. |
| Not Found | 404 | The server did not find anything that matches the Request-URI. No indication is given of whether the condition is temporary or permanent. |
| Conflict | 409 | The request could not be completed due to a conflict with the current state of the resource. |
| Internal Server Error | 500 | The server encountered an unexpected condition which prevented it from fulfilling the request. |

*NOTE: If you are having issues with DEL or PUT methods, make sure that your Password Vault Web Access (PVWA) Server's IIS instance does not include WebDav Publishing. This will cause known issues.*

## Community

Having trouble with CyberArk's REST API? Check out the [/r/CyberArk subreddit on Reddit](https://reddit.com/r/CyberArk)!
