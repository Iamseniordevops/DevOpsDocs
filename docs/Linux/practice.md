# Практические примеры
## Устранение неполадок в Ansible (AWX) связанных с истечением срока жизни токен Vault
1. Необходимо подключиться к Hashi Vault b ввести следующую команду, чтобы посмотреть список активных политик: vault policy list
2. Создаем новую политику, где указываем ее имя и срок жизни: vault token create -policy="autodog" -ttl="365d" В данном примере значение autodog это имя политики, а 365d это срок ее действия. Нужно сохранить полученный ключ
3. Переходим в проект bork на Gitlab bork/group_vars/all/vault.yaml
4. Этот файл необходимо расшифровать. Для этого можно установить на локальном компьютере wsl:
``` linenums="1"
wsl --install
sudo apt update
sudo apt upgrade
```
Далее устанавливаем ansible: sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
Проверяю версию ansible: ansible --version
Далее в текстовом редакторе nano или vi и создаем файл nano encripted.yml и копируем в него содержимое файла
Расшифровываю его с помощью команды ansible-vault decrypt encripted.yml
Заменяю hashi_vault_token на значение полученное во втором пункте
Шифрую файл обратно ansible-vault encrypt encripted.yml
В гитлаб по пути нахожу файл с токеном bork/group_vars/all/vault.yaml. Либо можно загрузить сохранённый из локальной машины
Заменяю его значения, далее нужно сделать commit и merge
Проверяем работу задания в AWX путем повторного запуска