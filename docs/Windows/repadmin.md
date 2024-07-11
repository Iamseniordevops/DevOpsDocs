# Принудильная репликация всех контроллеров домена

С помощью Powershell
``` powershell 
Get-ADDomainController -Filter * | ForEach-Object { Repadmin /syncall $_.hostname }
```

Через repadmin

``` BatchLexer
repadmin /syncall /e /d /A /P /q
```

/e: Запускает репликацию всех разделов.

/d: Подробный режим (детальная информация).

/A: Все контексты имен (Application partitions).

/P: Ожидание завершения (Push репликация).

/q: Тихий режим (минимизация вывода).