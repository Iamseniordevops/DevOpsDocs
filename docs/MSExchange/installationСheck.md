# Проверка установки

Проверка состояния служб
```powershell
Get-Service -DisplayName "Microsoft Exchange*"| Sort-Object -Property status -Descending | format-table -GroupBy status -AutoSize -Wrap displayname
```

После запуска сервера службы Exchange должны быть в следующем состоянии
``` pwsh-session
Status: Running 

DisplayName 
----------- 
Microsoft Exchange Replication 
Microsoft Exchange RPC Client Access 
Microsoft Exchange Service Host 
Microsoft Exchange Information Store 
Microsoft Exchange Mailbox Assistants 
Microsoft Exchange Mailbox Replication 
Microsoft Exchange Transport Log Search 
Microsoft Exchange Unified Messaging 
Microsoft Exchange Unified Messaging Call Router 
Microsoft Exchange Mailbox Transport Submission 
Microsoft Exchange Throttling 
Microsoft Exchange Transport 
Microsoft Exchange Health Manager Recovery 
Microsoft Exchange Diagnostics 
Microsoft Exchange Anti-spam Update 
Microsoft Exchange Active Directory Topology 
Microsoft Exchange Compliance Service 
Microsoft Exchange DAG Management 
Microsoft Exchange Mailbox Transport Delivery 
Microsoft Exchange Compliance Audit 
Microsoft Exchange Health Manager 
Microsoft Exchange EdgeSync 
Microsoft Exchange Frontend Transport 
Microsoft Exchange Search Host Controller 
Microsoft Exchange Search 


Status: Stopped 

DisplayName 
----------- 
Microsoft Exchange Server Extension for Windows Server Backup 
Microsoft Exchange Notifications Broker 
Microsoft Exchange IMAP4 Backend 
Microsoft Exchange POP3 
Microsoft Exchange IMAP4 
Microsoft Exchange POP3 Backend
```

Проверяем наличие базы данных
```powershell
Get-MailboxDatabase
```
Должна быть подключена одна база.

``` pwsh-session
Name                           Server          Recovery        ReplicationType
----                           ------          --------        ---------------
Mailbox Database 0639043316    T16MAIL1        False           None
```

Проверяем работу базы и подключение 
```powershell
Test-MAPIConnectivity
```
Результатом должно быть успешное подключение
```pwsh-session
MailboxServer      Database           Result    Error
-------------      --------           ------    -----
T16MAIL1           Mailbox Database   Success
                   0639043316
```

Проверяем появились ли почтовые ящики. Должны быть два ящика, администратор который устанавливал Exchange Server, а также ящик DiscoverySearchMailbox
```powershell
Get-Mailbox
```

```pwsh-console
Name                      Alias                ServerName       ProhibitSendQuota
----                      -----                ----------       -----------------
mail                      mail                 t16mail1         Unlimited
DiscoverySearchMailbox... DiscoverySearchMa... t16mail1         50 GB (53,687,091,200 bytes)
```

Проверяем служебные почтовые ящики. 

```powershell
Get-Mailbox -Arbitration
```

Результат 6 почтовых ящиков
```pwsh-console
Name                      Alias                ServerName       ProhibitSendQuota
----                      -----                ----------       -----------------
SystemMailbox{1f05a927... SystemMailbox{1f0... t16mail1         Unlimited
SystemMailbox{bb558c35... SystemMailbox{bb5... t16mail1         Unlimited
SystemMailbox{e0dc1c29... SystemMailbox{e0d... t16mail1         Unlimited
Migration.8f3e7716-201... Migration.8f3e771... t16mail1         300 MB (314,572,800 bytes)
FederatedEmail.4c1f4d8... FederatedEmail.4c... t16mail1         1 MB (1,048,576 bytes)
SystemMailbox{D0E409A0... SystemMailbox{D0E... t16mail1         Unlimited
SystemMailbox{2CE34405... SystemMailbox{2CE... t16mail1         Unlimited

```

Необходимо зайти на сервер через браузер и проверить работу ecp и owa. И также отправить письмо самому себе.