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

  
    
      
Installing Wireguard
-------------------------

Обновляем сервер:  
```
apt update  
apt upgrade -y  
```
  
  
Ставим wireguard:  
```
apt install -y wireguard  
```
  
  
Генерим ключи сервера:  
  ```
wg genkey | tee /etc/wireguard/privatekey | wg pubkey | tee /etc/wireguard/publickey
```

  
Проставляем права на приватный ключ:  
```
chmod 600 /etc/wireguard/privatekey  
```
  
  
Проверим, как у вас называется сетевой интерфейс:  
  
  ```
ip a  
  ```
Название интерфейса нам нужно для команд PostUp и PostDown  
Скорее всего это будет eth0 или ens3 или как-то иначе  
  ```
cat privatekey  
  
nano /etc/wireguard/wg0.conf  
  ```
  ```
[Interface]  
PrivateKey = <privatekey>  
Address = 10.0.0.1/24  
ListenPort = 51830  
PostUp = iptables -A FORWARD -i %i -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE  
PostDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE  
```
  
Вместо <privatekey> вставляем приватный ключ сервера  
  
  
Включаем ip-форвардинг для прокидования сети через wireguard  
  
  ```
echo "net.ipv4.ip_forward=1" >> /etc/sysctl.conf  
sysctl -p  
```
  
  
Включаем systemd демон с wireguard:  
  
  ```
systemctl enable wg-quick@wg0.service  
systemctl start wg-quick@wg0.service  
systemctl status wg-quick@wg0.service  
```
  
  
Создаём ключи клиента:  
  
  ```
wg genkey | tee /etc/wireguard/Cardano_privatekey | wg pubkey | tee /etc/wireguard/Cardano_publickey  
```
  
Добавляем в конфиг сервера клиента:  
  ```
nano /etc/wireguard/wg0.conf  
```
```
[Peer]  
PublicKey = <Cardano_publickey>  
AllowedIPs = 10.0.0.2/32  
```
  
  
Вместо <Cardano_publickey> — заменяем на содержимое файла /etc/wireguard/Cardano_publickey  
  
Перезагружаем systemd сервис с wireguard:  

```
systemctl restart wg-quick@wg0  
systemctl status wg-quick@wg0  
  ```

  
На локальной машине (например, на ноутбуке) создаём текстовый файл с конфигом клиента:  

nano wgClient.conf  
  
  ```
[Interface]  
PrivateKey = <CLIENT-PRIVATE-KEY>  
Address = 10.0.0.2/32  
DNS = 8.8.8.8  
 
[Peer]  
PublicKey = <SERVER-PUBKEY>  
Endpoint = <SERVER-IP>:51830  
AllowedIPs = 0.0.0.0/0  
PersistentKeepalive = 20  
```
  
  
Здесь <CLIENT-PRIVATE-KEY> заменяем на приватный ключ клиента, то есть содержимое файла /etc/wireguard/goloburdin_privatekey на сервере. <SERVER-PUBKEY> заменяем на   публичный ключ сервера, то есть на содержимое файла /etc/wireguard/publickey на сервере. <SERVER-IP> заменяем на IP сервера.  
  
Этот файл открываем в Wireguard клиенте (есть для всех операционных систем, в том числе мобильных) — и жмем в клиенте кнопку подключения.  

[![imageup.ru](https://imageup.ru/img285/4012725/chrome_cupn7umhsa.jpg)](https://imageup.ru/img285/4012725/chrome_cupn7umhsa.jpg.html)
















