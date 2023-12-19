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
### Установить систему с LVM, после чего переименовать VG ###

### Добавить модуль в initrd ###

