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