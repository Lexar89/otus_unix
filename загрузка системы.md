# Загрузка системы

### Редактируем файл /etc/default/grub

![image](https://github.com/user-attachments/assets/7529f0f7-7611-4fc8-94b3-b3e5fe10e9fc)


### Обновляем загрузчик

```
sudo update-grub
```
![image](https://github.com/user-attachments/assets/e522fc2c-3e65-43e7-9ab3-2f21de567512)


### Перезагружаем систему 

![image](https://github.com/user-attachments/assets/a3790c79-575e-4fa8-aee8-2354a04564e6)

### В окошке загрузчика нажимаем E 

![image](https://github.com/user-attachments/assets/0326d7f7-17f8-416c-b783-d579fdde10a1)


### В конце строки, начинающейся с linux добавляем init=/bin/bash и нажимаем ctrl-x

![image](https://github.com/user-attachments/assets/e5d49104-b53b-499f-9e3e-8d544e04ef2b)


### Загружаемся в режиме ro, изменяем на rw командой
```
mount -o remount,rw /
```

### Пробуем внести изменения в /etc/apt/sources.list.d/ubuntu.sources

![image](https://github.com/user-attachments/assets/7b12ae92-c522-45f1-838d-0334cc01d003)


  ### Ребутаемся, проверяем vg

![image](https://github.com/user-attachments/assets/4e612dce-3967-4e64-8a94-5651738c9ea9)

### Переименовываем vgs

```
sudo vgrename ubuntu-vg ubuntu-otus
```

###  Вносим изменения в /boot/grub/grub.cfg

```
sudo nano /boot/grub/grub.cfg
```
```
ctrl+\
ubuntu--vg
enter
ubuntu--otus
enter
```

### Автопоиском заменило в трёх местах

![image](https://github.com/user-attachments/assets/848a553c-b1b6-4340-837e-72d67837f97e)


### Ребутаемся и проверяем 

![image](https://github.com/user-attachments/assets/3a10baad-99c4-4269-bbab-1a3f365bd5f3)

### Готово



