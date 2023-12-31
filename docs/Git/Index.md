# GIT


## Начальная конфигурация

Перед добавлением коммитов нужно задать имя пользователя и почту, которые будут публиковаться в репозитории git

```bash
git config user.name "Ivan Ivanov"
```

```bash
git config user.email mail@gmail.com
```
!!! note "Примечание"

    Если нужно задать имя  для всех проектов, необходимо добавить параметр 
    ```bash
    --global
    ```

Далее сбрасываем автора, если он изначально не был задан для данного репозитория

```bash
git commit --amend --reset-author
```

## Создание коммитов

 добавление файлов в индекс GIT
```bash
git add .
```
 показывает список добавляемых файлов в индекс, но не добавляет их
```bash
git add . -n
```
Добавление коммита с комментарием
```bash
git commit -m $'Adding initial documentation files'
```

Отправка коммитов на удаленный репозиторий

```bash
git push origin main
```