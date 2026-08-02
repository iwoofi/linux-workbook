# Установка и настройка KVM

## 1. Проверка поддержки аппаратной виртуализации

Сначала убедитесь, что процессор поддерживает виртуализацию:

```bash
grep -E -c '(vmx|svm)' /proc/cpuinfo
```

- Если число **больше 0** - виртуализация поддерживается, можно продолжать.
- Если число **0** - включите в BIOS/UEFI настройки **Intel VT-x** (для Intel) или **AMD-V** (для AMD).

Дополнительная проверка флагов:

```bash
# Intel
grep -E 'vmx' /proc/cpuinfo

# AMD
grep -E 'svm' /proc/cpuinfo
```

## 2. Установка пакетов

```bash
sudo apt update
sudo apt install qemu-system libvirt-daemon-system libvirt-clients virtinst virt-manager bridge-utils
```

### Что устанавливается

| Пакет | Назначение |
|-------|-----------|
| `qemu-system` | Эмуляция виртуального оборудования |
| `libvirt-daemon-system` | Демон управления виртуализацией |
| `libvirt-clients` | Утилиты (`virsh`, `virt-install`) |
| `virtinst` | Утилита для создания ВМ из командной строки |
| `virt-manager` | Графический интерфейс |
| `bridge-utils` | Управление мостовыми сетями |

## 3. Добавление пользователя в группы

```bash
sudo adduser $USER libvirt
sudo adduser $USER kvm
```

> **Важно:** После добавления в группы выйдите из системы и зайдите заново (или перезагрузитесь), чтобы изменения вступили в силу.

## 4. Проверка загрузки модулей ядра

Убедитесь, что модули KVM загружены:

```bash
lsmod | grep kvm
```

Ожидаемый вывод (для AMD):

```text
kvm_amd
kvm
```

Для Intel будет `kvm_intel` вместо `kvm_amd`.

## 5. Проверка устройства /dev/kvm

```bash
ls -l /dev/kvm
```

Ожидаемый вывод:

```text
crw-rw----+ 1 root kvm 10, 232 ... /dev/kvm
```

Если устройство отсутствует - модуль KVM не загружен.

## 6. Проверка статуса служб libvirt

```bash
sudo systemctl status libvirtd
```

Должен быть статус **active (running)**.

> **Примечание:** Начиная с Ubuntu 22.04+, libvirt может использовать модульную архитектуру с отдельными сервисами (`libvirtd`, `virtlockd`, `virtlogd`). В классической конфигурации используется монолитный `libvirtd`. Обе архитектуры работают корректно.

Проверить активные юниты:

```bash
systemctl list-unit-files | grep -E 'virt|libvirt'
```

## 7. Проверка работы virsh

```bash
virsh list --all
```

Ожидаемый вывод (пустой список ВМ):

```text
 Id   Name   State
------------------
```

Проверить URI подключения:

```bash
virsh uri
```

Ожидаемый вывод:

```text
qemu:///system
```

## 8. Готово!

Если все проверки прошли успешно, система готова к созданию виртуальных машин.

Используйте `virt-manager` для графического управления или `virt-install` / `virsh` для работы из командной строки.
