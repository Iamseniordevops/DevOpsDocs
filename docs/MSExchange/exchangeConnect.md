
``` powershell
$UserCredential = Get-Credential
$Session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri http://mail.domain.com/PowerShell/ -Authentication Kerberos -Credential $UserCredential 
Import-PSSession $Session
```