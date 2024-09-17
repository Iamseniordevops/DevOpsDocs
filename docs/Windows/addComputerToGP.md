#Добавление компьютеров в групповую политику с помощью Powershell

``` powershell linenums="1"
# Задаем имя групповой политики

$GPOName = "GLOBAL_VEEAMBR_Tempate"

# Список компьютеров, которые необходимо добавить в групповую политику

$Computers = @(
"EKT-VBP",
"EKT-VBR"
)

foreach ($Computer in $Computers) {
    Set-GPPermissions `
        -Name $GPOName `
        -PermissionLevel GpoApply `
        -TargetName $Computer `
        -TargetType Computer
}

```