## Задание № 8. Загрузка системы ##
- Попасть в систему без пароля несколькими способами.
- Установить систему с LVM, после чего переименовать VG.
- Добавить модуль в initrd.

### Попасть в систему без пароля несколькими способами ###
1. В конце строки, начинающейся с **linux**, добавляем **init=/bin/bash** и нажимаем **сtrl-x** для загрузки в систему.\
![alt text](https://github.com/Dormidont23/otus-task8/blob/master/screenshots/01.png)\
Смотрим, что корневая файловая система смотирована в режиме _только чтение_. Выполняем **mount -o remount,rw /** и смотрим ещё раз.\
![alt text](https://github.com/Dormidont23/otus-task8/blob/master/screenshots/02.png)
2. В конце строки, начинающейся с **linux**, добавляем **rd.break** и нажимаем **сtrl-x** для загрузки в систему.\
![alt text](https://github.com/Dormidont23/otus-task8/blob/master/screenshots/03.png)\
Попадаем в emergency mode. Перемонтируем файловую систему на запись, меняем пароль рута.\
![alt text](https://github.com/Dormidont23/otus-task8/blob/master/screenshots/04.png)
3. В строке, начинающейся с **linux**, заменяем **ro** на **rw init=/sysroot/bin/sh** и нажимаем **сtrl-x** для загрузки в систему.\
![alt text](https://github.com/Dormidont23/otus-task8/blob/master/screenshots/05.png)\
Файловая система монтируется сразу в режиме **rw**.\
![alt text](https://github.com/Dormidont23/otus-task8/blob/master/screenshots/06.png)
### Установить систему с LVM, после чего переименовать VG ###
Посмотрим текущее состояние системы:\
[root@otus-task8 ~]# **vgs**\
  VG       #PV #LV #SN Attr   VSize    VFree\
  centos9s   1   2   0 wz--n- <127.00g    0\
Переименуем VG:\
[root@otus-task8 ~]# **vgrename centos9s NewNameVG**\
  Volume group "centos9s" successfully renamed to "NewNameVG"\
Далее правим **/etc/fstab**, **/etc/default/grub**. Везде заменяем старое название на новое. Перезагружаемся. Перед загрузкой ОС выбираем первую строку, жмём **e** и правим загрузочную запись, указывая вместо **centos9s** - **NewNameVG**. Жмём **ctrl+x** для загрузки.\
Далее создаём grub.cfg: **grub2-mkconfig -o /boot/efi/EFI/centos/grub.cfg**\
Перезагружаемся и проверяем:\
[root@otus-task8 ~]# **vgs**\
  VG        #PV #LV #SN Attr   VSize    VFree\
  NewNameVG   1   2   0 wz--n- <127.00g    0
### Добавить модуль в initrd ###
Скрипты модулей хранятся в каталоге /usr/lib/dracut/modules.d/. Для того, чтобы добавить свой модуль, создаем там каталог с именем 01test:\
[root@otus-task8 modules.d]# **mkdir /usr/lib/dracut/modules.d/01test**\
Создаём два скрипта:\
- **module-setup.sh** - устанавливает модуль и вызывает скрипт **test.sh**.
- **test.sh** - собственно сам вызываемый скрипт, в нём и рисуется пингвинчик.

Скрипты здесь https://github.com/Dormidont23/otus-task8/tree/master/scripts
[root@otus-task8 modules.d]# **dracut -f -v**\
...\
dracut: *** Creating image file '/boot/initramfs-5.14.0-366.el9.x86_64.img' ***\
dracut: dracut: using auto-determined compression method 'pigz'\
dracut: *** Creating initramfs image file '/boot/initramfs-5.14.0-366.el9.x86_64.img' done ***\
Проверяем, что наш модуль загружен в образ:\
[root@otus-task8 modules.d]# **lsinitrd -m /boot/initramfs-$(uname -r).img | grep test**\
test\
Перезагружаемся и руками выключаем опции rghb и quiet и проверяем вывод:\
![alt text](https://github.com/Dormidont23/otus-task8/blob/master/screenshots/07.png)
