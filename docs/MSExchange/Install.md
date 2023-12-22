# Установка Exchange Server

## Проверка уровня домена и леса

```powershell

# Импорт модуля Active Directory
Import-Module ActiveDirectory

# Получение информации о текущем домене
$domainInfo = Get-ADDomain

# Вывод уровня функциональности домена
Write-Host "Уровень функциональности домена: $($domainInfo.DomainMode)"


# Получение информации о текущем лесе
$forestInfo = Get-ADForest

# Вывод уровня функциональности леса
Write-Host "Уровень функциональности леса: $($forestInfo.ForestMode)"
```