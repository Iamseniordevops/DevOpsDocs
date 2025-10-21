## Вывод в командной строке

``` powershell linenums="1"
Get-Content -Path C:\Windows\System32\LogFiles\Firewall\domainfw.log -Wait |  
Select-String -Pattern "10\.2\.2\.204|10\.2\.2\.205|10\.2\.2\.212|10\.2\.2\.213|10\.2\.2\214|10\.2\.2\.215" | 
Select-String -Pattern "2025-10-21"
```

## Вывод в HTML

``` powershell linenums="1"
 function Get-WindowsFirewallLog {
param(
[parameter(Position=0,Mandatory=$false)]
[ValidateScript({Test-Path $_})]
[string]$LogFilePath = "$env:SystemRoot\System32\LogFiles\Firewall\publicfw.log"
)
$headerFields = @("date","time", "action","protocol","src-ip","dst-ip","src-port","dst-port","size", "tcpflags","tcpsyn", "tcpack","tcpwin","icmptype","icmpcode", "info","path")
$firewallLogs = Get-Content $LogFilePath | ConvertFrom-Csv -Header $headerFields -Delimiter ' '
$firewallLogs | Out-GridView
}
Get-WindowsFirewallLog 
```