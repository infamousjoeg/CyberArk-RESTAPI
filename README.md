## CyberArk REST API

All available requests in CyberArk Privileged Account Security Web Services v9.9.

### Postman Live Documentation

[View CyberArk's Live Documentation and Postman Collection](http://cybr.rocks/RESTAPIv99)

### Get Accounts via REST - PowerShell Example

This example demonstrates how to create a function in PowerShell for each REST call necessary and how to handle responses.

```powershell
function PASREST-Logon {

    # Declaration
    $webServicesLogon = "$PVWA_URL/PasswordVault/WebServices/auth/Cyberark/CyberArkAuthenticationService.svc/Logon"

    # Authentication
    $bodyParams = @{username = "Svc_CyberArkAPI"; password = "password"} | ConvertTo-JSON

    # Execution
    try {
        $logonResult = Invoke-RestMethod -Uri $webServicesLogon -Method POST -ContentType "application/json" -Body $bodyParams -ErrorVariable logonResultErr
        Return $logonResult.CyberArkLogonResult
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
    $webServicesLogoff = "$PVWA_URL/PasswordVault/WebServices/auth/Cyberark/CyberArkAuthenticationService.svc/Logoff"

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
    $webServicesGA = "$PVWA_URL/PasswordVault/WebServices/PIMServices.svc/Accounts?Keywords=$Keywords&Safe=$Safe"

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

Q.  All DEL or PUT methods return an error, how can I fix this?

A.  Make sure that your Password Vault Web Access (PVWA) Server's IIS instance does not include WebDav Publishing.

Having trouble with CyberArk's REST API? Check out the [/r/CyberArk subreddit on Reddit](https://reddit.com/r/CyberArk)!
