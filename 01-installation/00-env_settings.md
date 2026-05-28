# Настройка окружения

## Отключить звуковой БИП! динамика

**Удалить модуль на текущий сеанс**

```bash
sudo modprobe -r pcspkr
```
```bash
sudo rmmod pcspkr
```

**Включить модуль если надо**

```bash
sudo modprobe pcspkr
```

**Запретить загрузку модуля**

добавить запись 
`blacklist pcspkr`
в файл
`/etc/modprobe.d/blacklist.conf`

*одной командой добавить в (или создать) файл*

```bash
echo "blacklist pcspkr" | sudo tee -a /etc/modprobe.d/blacklist.conf
```

## Изменить шрифт в консоли, размер

```bash
sudo dpkg-reconfigure console-setup
```