# Настройка окружения

## настройки под себя

## Отключить звуковой БИП! динамика

**Удалить модуль на текущий сеанс**
`sudo modprobe -r pcspkr`
`sudo rmmod pcspkr`

**Включить модуль**
`sudo modprobe pcspkr`

**Запретить загрузку модуля**
добавить запись 
`blacklist pcspkr`
в файл
`/etc/modprobe.d/blacklist.conf`

*одной командой добавить в (или создать) файл*
`echo "blacklist pcspkr" | sudo tee -a /etc/modprobe.d/blacklist.conf`

