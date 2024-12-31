# Отключение энергосберегающего режима для Wi-Fi на Fedora
Открываем с помощью nano файл:
```bash linenums="1"
sudo nano /etc/NetworkManager/conf.d/default-wifi-powersave-on.conf
```
Если в нем, что есть изменяем или добавляем строку
```bash linenums="1"
[connection]
wifi.powersave = 2
```
После этого перезагружаем службу NetworkManager

```bash linenums="1"
sudo systemctl restart NetworkManager
```
Не помогло 