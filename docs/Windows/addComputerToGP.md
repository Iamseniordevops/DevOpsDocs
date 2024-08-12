#Добавление компьютеров в групповую политику с помощью Powershell

``` powershell
$GPOName = "GLOBAL_VEEAMBR_Tempate"
$Computers = @(
"EKT-VBP",
"EKT-VBR"
)

foreach ($Computer in $Computers) {
    Set-GPPermissions -Name $GPOName -PermissionLevel GpoApply -TargetName $Computer -TargetType Computer
}
```