1. Задание 1. Kernel and Module Inspection (15 баллов).

    - Продемонстрировать версию ядра вашей ОС

      ![](./screenshots/kernel_info.png)

    - Показать все загруженные модули ядра

      ![](./screenshots/lsmod_1.png)

      ![](./screenshots/lsmod_2.png)

    - Отключить автозагрузку модуля `cdrom`

      ![](./screenshots/blacklist_cdrom.png)

      После перезагрузки наблюдаем отсутствие модулей

      ![](./screenshots/cdrom_blacklisted.png)

    - Найти и описать конфигурацию ядра (файл конфигурации, параметр `CONFIG_XFS_FS`)

      ![](./screenshots/kernel_config_xfs.png)

      Значение параметра `m` означает, что файловая система `XFS` поддерживается, но через отдельный модуль

2. Задание 2. Наблюдение за VFS (20 баллов)

    - Используйте `strace` для анализа команды `cat /etc/os-release > /dev/null`. Для этого
      запустите `strace -e trace=openat,read,write,close cat /etc/os-release > /dev/null`

      ![](./screenshots/strace.png)

    - Описать открываемый и читаемый файл, объяснить отсутствие записывающих вызовов в выводе

      Можно наблюдать один `write` в выводе `strace`, однако непосредственно вывода в `stdout` нет, поскольку он
      перенаправлен в `/dev/null`

3. Задание 3. LVM Management (40 баллов)

    - Добавить к своей виртуальной машине диск `/dev/sdb` размером `2GB`

   ![](./screenshots/lsblk_before.png)

    - Создать раздел на `/dev/sdb`, используя `fdisk` или `parted`

   ![](./screenshots/fdisk.png)

    - Создать `Physical Volume (PV)` на этом разделе

   ![](./screenshots/pvcreate.png)

   ![](./screenshots/pvdisplay.png)

    - Создать `Volume Group (VG)` с именем `vg_highload`

   ![](./screenshots/vgcreate.png)

   ![](./screenshots/vgdisplay.png)

    - Создать два `Logical Volume (LV)`: `data_lv (1200MB)` и `logs_lv (оставшееся место)`

   ![](./screenshots/lvcreate_data.png)

   ![](./screenshots/lvcreate_logs.png)

   ![](./screenshots/lvdisplay.png)

    - Отформатировать `data_lv` как `ext4` и примонтировать в `/mnt/app_data`

   ![](./screenshots/mkfs_ext4_mount.png)

    - Отформатировать `logs_lv` как `xfs` и примонтировать в `/mnt/app_logs`

   ![](./screenshots/mkfs_xfs_mount.png)

4. Задание 4. Использование pseudo filesystem (25 баллов)

    - Извлечь из `/proc` модель CPU и объем памяти (KiB)

   К сожалению, `arm64` версия Debian не отдает подробную информацию о процессоре из `/proc/cpuinfo` (что замечали и
   пользователи производных от него систем, например Raspberry OS https://github.com/raspberrypi/linux/issues/3991)

   ![](./screenshots/proc_cpuinfo_fail.png)

   Поэтому данная команда была повторно выполнена на другом устройстве в WSL

   ![](./screenshots/cpuinfo.jpg)

   С `/proc/meminfo` же проблем никаких нет 

   ![](./screenshots/meminfo.png)

    - Используя `/proc/$$/status`, найдите `Parent Process ID (PPid)` вашего текущего shell. Что означает `$$`?

   ![](./screenshots/ppid.png)

    - Определить настройки `I/O Scheduler` для основного диска `/dev/sda`

   ![](./screenshots/io_scheduler.png)

   ![](./screenshots/is_hdd.png)

    - Определить размер `MTU` для основного сетевого интерфейса

   ![](./screenshots/mtu.png)

   `MTU = 1500`
