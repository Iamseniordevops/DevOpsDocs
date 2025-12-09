
пароль можно сменить с мак через сочетание клавиш Fn + Control + Option (Alt) + Delete (Backspace)

На windows Пароль можно изменить самостоятельно на rds сервере. Для этого нужно нажать CTRL + ALT + END
и выбрать [change password](https://winitpro.ru/index.php/2014/06/23/kak-izmenit-parol-v-rdp-sessii-windows-server-2012/)

В доменной среде можно использовать скрипт
``` powershell linenums="1"
Set-ADAccountPassword -Identity $env:USERNAME -OldPassword (Read-Host -AsSecureString "Введите текущий пароль") -NewPassword (Read-Host -AsSecureString "Введите новый пароль")
```
В локальной среде следующий скрипт
``` powershell linenums="1"
 # Необходимо запускать с правами Администратора

# 1. Запрашиваем новый пароль
$NewPassword = Read-Host -AsSecureString "Введите новый пароль для локальной учетной записи $env:USERNAME"

# 2. Меняем пароль текущего пользователя
# Идентификатор пользователя берется из переменной среды $env:USERNAME
try {
    Set-LocalUser -Name $env:USERNAME -Password $NewPassword
    Write-Host "Пароль локальной учетной записи '$env:USERNAME' успешно изменен." -ForegroundColor Green
}
catch {
    Write-Host "Ошибка при смене пароля: $($_.Exception.Message)" -ForegroundColor Red
    Write-Host "Убедитесь, что вы запустили скрипт от имени Администратора." -ForegroundColor Yellow
} 

```