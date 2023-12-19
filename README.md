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
  Volume group "centos9s" successfully renamed to "NewNameVG"

### Добавить модуль в initrd ###

