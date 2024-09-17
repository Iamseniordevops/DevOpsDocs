# Практические примеры
## Устранение неполадок в Ansible (AWX) связанных с истечением срока жизни токен Vault
<div style="text-align: justify;">
Текст, который будет выровнен по ширине. Markdown позволяет использовать HTML внутри текста, что делает возможным реализацию кастомных стилей для отдельных блоков или параграфов.
</div>
1. Необходимо подключиться к Hashi Vault и ввести следующую команду, чтобы посмотреть список активных политик: vault policy list
2. Создаем новую политику, где указываем ее имя и срок жизни: vault token create -policy="autodog" -ttl="365d" В данном примере значение autodog это имя политики, а 365d это срок ее действия. Нужно сохранить полученный ключ
3. Переходим в проект bork на Gitlab bork внутри необоходимо найти файл с настройками, в формате YAML vault.yaml
4. Этот файл необходимо расшифровать. Для этого можно установить на локальном компьютере wsl:


```bash linenums="1"
wsl --install
sudo apt update
sudo apt upgrade
```
Далее устанавливаем ansible:
```bash linenums="1"
sudo apt install software-properties-common
```
```bash linenums="1"
sudo add-apt-repository --yes --update ppa:ansible/ansible
```
```bash linenums="1"
sudo apt install ansible
```
Проверяю версию ansible: 
```
ansible --version
```
<div style="text-align: justify;">
Далее в текстовом редакторе nano или vi и создаем файл nano encripted.yml и копируем в него содержимое файла.
Расшифровываю его с помощью команды
<div>
```
ansible-vault decrypt encripted.yml
```
Заменяю hashi_vault_token на значение полученное во втором пункте.
Шифрую файл обратно 
```
ansible-vault encrypt encripted.yml
```
В гитлаб по пути нахожу файл с токеном bork vault.yaml. Либо можно загрузить сохранённый из локальной машины
Заменяю его значения, далее нужно сделать commit и merge
Проверяем работу задания в AWX путем повторного запуска