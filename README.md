# WireGuard-ShadowSocks_Profile

Installing
-------------------------
### Server
```no-highlight
* apt update  
* apt upgrade -y  
* apt-get install shadowsocks-libev, нажать y  
* apt-get update
```
Если на сервере не установлен текстовый редактор, то его нужно установить
```no-highlight
* apt-get install nano  
```

Открываем конфигурационный файл и устанавливаем ip  
нашего сервера, порт подключения,  
пароль от сервера и метод шифрования  

```no-highlight
* nano /etc/shadowsocks-libev/config.json  
```
* после коррекции жмем Ctrl+O Enter Ctrl+Х

```no-highlight
* apt-get install python3-nacl 
* service shadowsocks-libev restart  
* service shadowsocks-libev status  
```

### Client ShadowSocks
Для переадресации трафика на устройстве необходимо использовать ShadowSocks клиент  

[Windows](https://github.com/shadowsocks/shadowsocks-windows/releases/)  
[Android Google Play](https://play.google.com/store/apps/details?id=com.github.shadowsocks&hl=ru&gl=US)  
[Android F-droid](https://f-droid.org/ru/packages/com.gitlab.mahc9kez.shadowsocks.foss/)  
[IOS](https://apps.apple.com/ru/app/sockswitch-shadowsocks-client/id1453207024)  
[MacOS](https://universonic.github.io/shadowsocks-macos/)  

Пример конфигурации в windwos клиенте  
![](https://imageup.ru/img256/3966154/shadowsocks_fafjl4rvly.png?nc)

Далее, в трее: Системный прокси сервер(System proxy-server) выбираем нужный нам вариант переодрисации и проверяем соединение 












