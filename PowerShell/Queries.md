# PowerShell queries

## Invoke url with query parameters
Use backtick character (`ALT+96` shortcut) before query parameter to avoid that PS will handle parameter as a property.

Possible query parameters are `$count`, `$expand`, `$format`, `$orderby`, `$search`, `$select`, `$skip`, `$skipToken`, and `$top`.

``` PowerShell
 $mail_user = "name@tenant.com"
 $uri = "https://graph.microsoft.com/v1.0/users/$mail_user/mailFolders/inbox/messages?`$filter=categories/any(a:a eq 'Green category')&count=true" 
 $resp = Invoke-RestMethod -Method GET -Uri $blue_uri -ContentType "application/json" -Headers @{Authorization=("bearer {0}" -f $accessToken)
 ```

 ## Encode shared url
Some Graph API endpoints require properly encoded a sharing URL as one of a parameter.

To encode the sharing URL, use the following logic:

First, use base64 encode the URL.

Convert the base64 encoded result to unpadded base64url format by removing `=` characters from the end of the value, replacing `/` with `_` and `+` with `-`.

Append `u!` to be beginning of the string.
``` PowerShell
$url = "https://onedrive.live.com/redir?resid=1231244193912!12&authKey=1201919!12921!1"
$base64Value = [Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($url))
$encodedUrl = "u!" + $base64Value.TrimEnd('=').Replace('/','_').Replace('+','-')
```