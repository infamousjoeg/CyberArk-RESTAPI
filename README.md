## CyberArk REST API

All available requests in CyberArk Privileged Account Security (PAS) REST API.

LAST UPDATED: v11.6

**THIS IS UNOFFICIAL DOCUMENTATION**

### Getting Started Guide

[Getting Started with REST Using Postman](Getting%20Started%20with%20REST%20Using%20Postman.pdf) (PDF)

### Postman Live Documentation

[View CyberArk's Live Documentation and Postman Collection](http://cybr.rocks/RESTAPI)

### Get Accounts via REST - PowerShell Example

This example demonstrates how to create a function in PowerShell for each REST call necessary and how to handle responses.

```powershell
function PASREST-Logon {

    # Declaration
    $webServicesLogon = "$PVWA_URL/PasswordVault/api/auth/ldap/logon"

    # Authentication
    $bodyParams = @{username = "Svc_CyberArkAPI"; password = "password"} | ConvertTo-JSON

    # Execution
    try {
        $logonResult = Invoke-RestMethod -Uri $webServicesLogon -Method POST -ContentType "application/json" -Body $bodyParams -ErrorVariable logonResultErr
        Return $logonResult.Trim('"')
    }
    catch {
        Write-Host "StatusCode: " $_.Exception.Response.StatusCode.value__
        Write-Host "StatusDescription: " $_.Exception.Response.StatusDescription
        Write-Host "Response: " $_.Exception.Message
        Return $false
    }
}

function PASREST-Logoff ([string]$Authorization) {

    # Declaration
    $webServicesLogoff = "$PVWA_URL/PasswordVault/api/auth/logoff"

    # Authorization
    $headerParams = @{}
    $headerParams.Add("Authorization",$Authorization)

    # Execution
    try {
        $logoffResult = Invoke-RestMethod -Uri $webServicesLogoff -Method POST -ContentType "application/json" -Header $headerParams -ErrorVariable logoffResultErr
        Return $true
    }
    catch {
        Write-Host "StatusCode: " $_.Exception.Response.StatusCode.value__
        Write-Host "StatusDescription: " $_.Exception.Response.StatusDescription
        Write-Host "Response: " $_.Exception.Message
        Return $false
    }
}

function PASREST-GetAccount ([string]$Authorization) {

    # Declaration
    $webServicesGA = "$PVWA_URL/PasswordVault/api/Accounts?Keywords=$Keywords&Safe=$Safe"

    # Authorization
    $headerParams = @{}
    $headerParams.Add("Authorization",$sessionID)

    # Execution
    try {
        $getAccountResult = Invoke-RestMethod -Uri $webServicesGA -Method GET -ContentType "application/json" -Headers $headerParams -ErrorVariable getAccountResultErr
        return $getAccountResult
    }
    catch {
        Write-Host "StatusCode:" $_.Exception.Response.StatusCode.value__
        Write-Host "StatusDescription:" $_.Exception.Response.StatusDescription
        Write-Host "Response:" $_.Exception.Message
        return $false
    }
}

# Global Declaration
$PVWA_URL = "https://components.cyberark.local"
$Keywords = "TestAccount"
$Safe = "TestSafe"

# Execute Logon
$sessionID = PASREST-Logon

# Error Handling for Logon
if ($sessionID -eq $false) {Write-Host "[ERROR] There was an error logging into the Vault." -ForegroundColor Red; break}
else {Write-Host "[INFO] Logon completed successfully." -ForegroundColor DarkYellow}

# Execute Get Accounts
$getAccountResult = PASREST-GetAccount -Authorization $sessionID
if ($getAccountResult -eq $false) {Write-Host "[ERROR] There was an error getting the account from the Vault."-ForegroundColor Red; break}
else {$getAccountResult.accounts | Format-Table -Property AccountID}

# Execute Logoff
$logoffResult = PASREST-Logoff -Authorization $sessionID
if ($logoffResult -eq $true) {Write-Host "[INFO] Logoff completed successfully." -ForegroundColor DarkYellow}
else {Write-Host "[ERROR] Logoff was not completed successfully.  Please logout manually using Authorization token:" $sessionID -ForegroundColor Red}
```

### Support or Contact

**SYMPTOM:**
A delete request was sent to the Vault, and the following response was received: `405 Method not allowed`.

**PROBLEM:**
The `DELETE`/`PUT` command is handled by the WebDAV instead of the Restful services.

**SOLUTION:**
1. Edit the PVWA's `web.config` file.
2. Search for `<add name= "WebDAV" Path=......>`
3. In that line search for the `DELETE` & `PUT` command and delete them, leaving the other ones.
4. Save the file
5. Restart IIS

Having trouble with CyberArk's REST API? Check out the [/r/CyberArk subreddit on Reddit](https://reddit.com/r/CyberArk)!
