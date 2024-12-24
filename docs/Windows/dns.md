## Экспорт DNS
I was in the process of enabling DNS scavenging following the steps in this blog post. I’m used to it already being on but going into an existing environment with it disabled & enabling it could be bad. I was at the “enable” phase mentioned in the blog when I thought that it would be nice to have an export of all the zones just in case. While I’m sure there are other solutions, this is mine.

Browse to 
```
C:\Windows\System32\dns
```
Create a folder called export.
Open a command prompt.
Browse to C:\Temp.
Run the following commands:
```
dnscmd /enumzones > AllZones.txt
```
```
for /f %a in (AllZones.txt) do dnscmd /ZoneExport %a export\%a.txt
```

In the export folder you will now have a text file with an export for every zone (forward & reverse) in your DNS server.
Now create a subfolder in the YYYYMMDD format under the export folder created earlier.
Move all the text files to that folder.

[Original](https://patrickhoban.wordpress.com/2012/06/29/900-2/)

## Сохранение результатов команд в файл
```
w32tm /query /configuration > C:\Logs\time_config.txt
```
## Экспорт Conditional Forwarders
```
Get-DnsServerZone | Where-Object {$_.ZoneType -eq 'Forwarder'} | Export-Csv -Path "C:\temp\DNS_Conditional_Forwarders.csv" -NoTypeInformation
```

## Экспорт всех DNS zones records в CSV с помощью PowerShell

``` powershell linenums="1"
# Очищаем консоль
Clear-Host

# Создаем новый список для хранения записей
$Report = [System.Collections.Generic.List[Object]]::new()

# Получаем список зон DNS с сервера
$zones = Get-DnsServerZone

# Проходим по каждой зоне DNS
foreach ($zone in $zones) {
    # Получаем записи ресурса для текущей зоны
    $zoneInfo = Get-DnsServerResourceRecord -ZoneName $zone.ZoneName
    
    # Проходим по каждой записи ресурса в текущей зоне
    foreach ($info in $zoneInfo) {
        # Получаем временную метку, если она есть, иначе устанавливаем значение "static"
        $timestamp = if ($info.Timestamp) { $info.Timestamp } else { "static" }

        # Получаем время жизни записи в секундах
        $timetolive = $info.TimeToLive.TotalSeconds
        
        # Получаем данные записи в зависимости от ее типа
        $recordData = switch ($info.RecordType) {
            'A' { $info.RecordData.IPv4Address }  # Адрес IPv4
            'CNAME' { $info.RecordData.HostnameAlias }  # Псевдоним
            'NS' { $info.RecordData.NameServer }  # Сервер имен
            'SOA' { "[$($info.RecordData.SerialNumber)] $($info.RecordData.PrimaryServer), $($info.RecordData.ResponsiblePerson)" }  # SOA запись
            'SRV' { $info.RecordData.DomainName }  # SRV запись
            'PTR' { $info.RecordData.PtrDomainName }  # PTR запись
            'MX' { $info.RecordData.MailExchange }  # MX запись
            'AAAA' { $info.RecordData.IPv6Address }  # Адрес IPv6
            'TXT' { $info.RecordData.DescriptiveText }  # TXT запись
            default { $null }  # Если тип записи не распознан
        }

        # Создаем объект для записи и добавляем его в список отчетов
        $ReportLine = [PSCustomObject]@{
            Name       = $zone.ZoneName       # Название зоны
            Hostname   = $info.Hostname       # Имя хоста
            Type       = $info.RecordType     # Тип записи
            Data       = $recordData          # Данные записи
            Timestamp  = $timestamp           # Временная метка
            TimeToLive = $timetolive          # Время жизни записи
        }
        # Добавляем объект записи в отчет
        $Report.Add($ReportLine)
    }
}

# Указываем путь для экспорта отчета в формате CSV
$exportPath = "C:\temp\All_DNS_Zones_Records.csv"

# Экспортируем данные в CSV файл без типа информации и с кодировкой UTF-8
$Report | Export-Csv $exportPath -NoTypeInformation -Encoding utf8

# Проверяем, был ли файл успешно создан, и выводим соответствующее сообщение
if (Test-Path $exportPath) {
    Write-Host "Скрипт успешно выполнен. Файл создан: $exportPath"
} else {
    Write-Host "Произошла ошибка при создании файла."
}
```
[Оригинал](https://www.alitajran.com/export-all-dns-records-to-csv-powershell/)
