# Запуск оснасток mmc

Для удаленного запуска оснасток с недоменного компьютера в доменной сети нужно использовать команду: 

``` doscon
runas /netonly /user:Domain\User "mmc dnsmgmt.msc"
```
DNS
``` doscon  
dnsmgmt.msc
```
Active Directory домены и доверие
``` doscon  
domain.msc
```
Консоль управления GPO (Group Policy Management Console)
```
gpmc.msc
```
Управление сертификатами
```
certmgr.msc
```