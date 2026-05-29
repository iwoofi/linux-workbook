# Установка и настройка SSH

**Если не был установлен SSH**

```bash
sudo apt install openssh-server
```

проверить что SSH работает

```bash
sudo systemctl status ssh
```

если работает фаервол надо разрешить работу ssh

```bash
sudo ufw allow ssh
```

теперь доступен для подключений по SSH

**Подключение по SSH-ключам**

Генерируем SSH-ключи на клиенте (кто будет подключаться к серверу), на linux и windows

```bash
ssh-keygen -t ed25519 -C "доп. инфа"
```

можно также на следующем этапе изменить имя файла ключей

далее надо скопировать публичный ключ на сервер

линукс
```bash
ssh-copy-id username@server_ip 
```

виндус (изменить путь до ключа на винде, пользователь и сервер для подключения по ssh 
```powershell
type ~\.ssh\id_ed25519.pub | ssh user@ip_server_адрес "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

Проверить и изменить при необходимости на сервере права на директорию и файл
```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys 
```

Далее смотрим в конфиге разрешение подключения по SSH-ключам 
сделаем копию конфига

```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak 
```

и изменим в конфиге настройки 

```ini
PubkeyAuthentication yes
```

после правки конфига проверяем корректность и перезапускаем сервис

```bash
sudo sshd -t
sudo systemctl restart ssh 
```

**далее пробуем подключиться к серверу через SSH-ключ**

Можно с указанием ключа подключаться (пока винда)

```powershell
ssh -i C:\Users\Имя_Вашего_Пользователя\.ssh\мой_ключ логин@ip_адрес_сервера
```

или с создание конфига для алиаса
создать файл без расширения

`C:\Users\%USERNAME%\.ssh\config`

Добавить туда:

```ini
# Это алиас (короткое имя), которое будем использовать для подключения.
# Например, "ssh mydebian"
Host mydebian
    # Реальный IP-адрес или доменное имя сервера.
    HostName 192.168.1.10
    # Имя пользователя на сервере.
    User your_username
    # ПУТЬ К ВАШЕМУ ПЕРЕИМЕНОВАННОМУ ПРИВАТНОМУ КЛЮЧУ.
    IdentityFile C:\Users\%USERNAME%\.ssh\my_custom_key
    # Порт, если вы его меняли со стандартного (22).
    # Port 2222
```

после это подключение будет просто через алиас

```powershell
ssh mydebian
```

**если все в порядке и подключение проходит можно донастроить конфиг SSH на сервере**
как пример (позже расписать)

PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
Port 2222  (если изменили порт, то не забыть настроить в фаерволе)
AllowUsers user1 user2
PermitEmptyPasswords no
MaxAuthTries 3
ClientAliveInterval 300
ClientAliveCountMax 2
X11Forwarding no


**Еще настроить Fail2Ban**

