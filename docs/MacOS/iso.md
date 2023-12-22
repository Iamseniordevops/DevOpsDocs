# Загрузочный ISO

Для создания загрузочного ISO нужно на Mac OS скачать нужную версию, например Sonoma. 

Создаем пустой диск в папке /tmp

``` zsh
/tmp % hdiutil create -o /tmp/Sonoma -size 15179m -volname Sonomaiso -layout SPUD -fs HFS+J -type UDTO -attach

```
Переходим в папку tmp

```zsh
cd /tmp
```

Создаем загрузочный диск

``` zsh
sudo /Applications/Install\ macOS\ Sonoma.app/Contents/Resources/createinstallmedia --volume /Volumes/Sonomaiso --nointeraction
```

Преобразуем  его в iso и перемещаем на рабочий стол

``` zsh
/tmp % mv /tmp/Sonoma.cdr ~/Desktop/Sonoma.iso
```