# Полезные скрипты на PowerShell

## Проверка хеш (Hash) файлов
В первую строку после $knownHash нужно внести свое значение хеш суммы, во второй строке после -Path внести путь к проверяемому файлу. 
``` powershell linenums="1"
$knownHash = "171B2EE89B4DD93FF151D8850B8A5AA7"
$fileHash = (Get-FileHash -Path "C:\VM.iso" -Algorithm MD5).Hash

if ($fileHash -eq $knownHash) {
    Write-Host "Хеш соответствует известному значению"
} else {
    Write-Host "Хеш не соответствует известному значению"
}

```
Проверка с помощью certutil
```
certutil -hashfile SQLServer2019-KB5049235-x64.exe SHA256
```
## Вывод списка процессов отфильтрованных по пользователю
``` powershell linenums="1"
Get-Process -IncludeUserName | Where-Object {$_.Username -eq "ssaas\prtg"}
```
## Вывод имени, CPU, RAM, User, и времени старта процессов по пользователю
``` powershell linenums="1"
Get-Process -IncludeUserName | Where-Object {$_.Username -eq "ssaas\prtg"} | Select-Object name, CPU, WS, Username, StartTime | ft -a
```
## Завершение всех процессов пользователя
``` powershell linenums="1"
# Определяем имя пользователя
$username = "ssaas\prtg"

# Получаем список процессов, запущенных указанным пользователем
$processes = Get-Process -IncludeUserName | Where-Object { $_.UserName -eq $username }

# Завершаем каждый процесс
foreach ($process in $processes) {
    try {
        $process.Kill()  # Быстрое завершение процесса
        Write-Host "Процесс с ID $($process.Id) ($($process.Name)) завершён."
    } catch {
        Write-Warning "Не удалось завершить процесс с ID $($process.Id) ($($process.Name)): $_"
    }
}
```
Фильтрация в командной строке
```
arp -a | findstr 10.7.28.174
```

Очистка всех корзин на терминальном сервере
``` powershell linenums="1"
Get-ChildItem -Path 'C:\$Recycle.Bin' -Force | Remove-Item -Recurse -ErrorAction SilentlyContinue
```