# Перенос локальной GPO на другой компьютер с редактированием Firewall через LGPO


### Логика процесса

1. На исходном ПК сделать backup локальной GPO.
2. На исходном ПК разобрать `Machine\registry.pol` из этого backup в текст.
3. Отредактировать в тексте настройки firewall.
4. Собрать новый `registry.pol`.
5. Подменить им `Machine\registry.pol` внутри backup.
6. Скопировать backup на целевой ПК.
7. На целевом ПК сделать резервную копию текущей локальной политики.
8. Импортировать изменённый backup.

---

## Пошаговая инструкция

### Шаг 1. Выгрузить локальную политику с исходного компьютера

Откройте `cmd` **от имени администратора** на исходном ПК и выполните:

```cmd
mkdir C:\Temp\LGPO-Source
lgpo.exe /b C:\Temp\LGPO-Source
```

Эта команда создаст backup локальной политики в формате GPO backup.

После этого внутри `C:\Temp\LGPO-Source` появится папка с GUID, а в ней — структура backup, включая файл:

```text
DomainSysvol\GPO\Machine\registry.pol
```

Именно этот файл нужен для правки **machine-side policy**.

---

### Шаг 2. Найти `Machine\registry.pol`

Обычно путь будет таким:

```text
C:\Temp\LGPO-Source\{GUID}\DomainSysvol\GPO\Machine\registry.pol
```

Где `{GUID}` — имя созданной папки backup.

---

### Шаг 3. Разобрать `registry.pol` в текст

Выполните:

```cmd
lgpo.exe /parse /m "C:\Temp\LGPO-Source\{GUID}\DomainSysvol\GPO\Machine\registry.pol" > "C:\Temp\LGPO-Source\Machine.txt"
```

Эта команда **ничего не применяет**.  
Она только переводит `registry.pol` в читаемый текстовый формат **LGPO text**.

---

### Шаг 4. Отредактировать firewall в `Machine.txt`

Откройте файл:

```text
C:\Temp\LGPO-Source\Machine.txt
```

Редактировать нужно **только нужные строки firewall**.

Настройки Windows Firewall в локальной политике относятся к ветке:

```text
Computer Configuration > Policies > Windows Settings > Security Settings > Windows Firewall with Advanced Security
```

!!! note "Важное замечание"

    Ручная правка конкретных firewall rule blocks в `Machine.txt` требует аккуратности.

    - Простые policy-параметры обычно редактируются нормально.
    - Сложные правила с длинными строками, GUID и параметрами профилей чувствительны к ошибкам.
    - Если изменяется именно набор правил, а не только общие параметры профиля firewall, править нужно особенно внимательно.

---

### Шаг 5. Собрать новый `registry.pol`

После редактирования выполните:

```cmd
lgpo.exe /r "C:\Temp\LGPO-Source\Machine.txt" /w "C:\Temp\LGPO-Source\registry.pol"
```

Эта команда собирает новый бинарный `registry.pol` из текстового файла.

Она **пока ничего не импортирует в систему**.

---

### Шаг 6. Подменить `registry.pol` внутри backup

Теперь замените исходный `Machine\registry.pol` в backup на новый:

```cmd
copy /y "C:\Temp\LGPO-Source\registry.pol" "C:\Temp\LGPO-Source\{GUID}\DomainSysvol\GPO\Machine\registry.pol"
```

После этого backup уже будет содержать:

- все исходные локальные политики;
- ваши изменения по firewall.

---

### Шаг 7. Перенести backup на целевой компьютер

Скопируйте всю папку:

```text
C:\Temp\LGPO-Source
```

на целевой компьютер, например в:

```text
C:\Temp\LGPO-Import
```

---

### Шаг 8. На целевом компьютере снять backup текущей локальной политики

На целевом ПК откройте `cmd` **от имени администратора** и выполните:

```cmd
mkdir C:\Temp\LGPO-Target-Before
lgpo.exe /b C:\Temp\LGPO-Target-Before
```

Это rollback-копия на случай, если понадобится вернуть текущее состояние локальной политики.

---

### Шаг 9. Импортировать изменённый backup на целевой компьютер

На целевом ПК выполните:

```cmd
lgpo.exe /g C:\Temp\LGPO-Import
gpupdate /force
```

Что происходит:

- `lgpo.exe /g` импортирует настройки из GPO backup;
- `gpupdate /force` принудительно обновляет применение политики.

---

### Шаг 10. Проверить результат

После импорта проверьте:

1. локальные политики применились;
2. настройки firewall выглядят как ожидалось;
3. удалённый доступ и нужные сервисы не сломались;
4. если компьютер в домене — доменные GPO не переопределили локальные настройки.

---

## Готовый минимальный набор команд

### На исходном ПК

```cmd
mkdir C:\Temp\LGPO-Source
lgpo.exe /b C:\Temp\LGPO-Source

lgpo.exe /parse /m "C:\Temp\LGPO-Source\{GUID}\DomainSysvol\GPO\Machine\registry.pol" > "C:\Temp\LGPO-Source\Machine.txt"

lgpo.exe /r "C:\Temp\LGPO-Source\Machine.txt" /w "C:\Temp\LGPO-Source\registry.pol"

copy /y "C:\Temp\LGPO-Source\registry.pol" "C:\Temp\LGPO-Source\{GUID}\DomainSysvol\GPO\Machine\registry.pol"
```

После этого скопируйте папку `C:\Temp\LGPO-Source` на целевой ПК.

---

### На целевом ПК

```cmd
mkdir C:\Temp\LGPO-Target-Before
lgpo.exe /b C:\Temp\LGPO-Target-Before

lgpo.exe /g C:\Temp\LGPO-Import
gpupdate /force
```

---

## Краткий итог

Для переноса локальной политики достаточно следующей модели:

1. Сделать backup локальной GPO на исходном ПК.
2. Разобрать `Machine\registry.pol` в текст.
3. Внести нужные изменения в firewall.
4. Собрать новый `registry.pol`.
5. Подменить его в backup.
6. Импортировать этот backup на целевой ПК.

Это даёт перенос локальной политики **без промежуточной машины** и с возможностью точечно изменить firewall перед импортом.
