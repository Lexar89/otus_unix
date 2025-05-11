# Дисковая подсистема
___
### Имеем в наличии два блочных устройства по 10G, sdb и sdc
### Делаем из них рейд 5
```
mdadm --create /dev/md42 -l 5 -n 2 /dev/sdb /dev/sdc
```
### Проверим, что рейд создался корректно
```
cat /proc/mdstat
```
```
mdadm -D /dev/md42
```
![image](https://github.com/user-attachments/assets/c6b7f9cc-1408-43e1-8864-7b18ba2609e5)

### Рейд собран, далее нужно создать файловую систему

```
mkfs.ext4 /dev/md42
```
### Создадим директорию и смонтируем в неё наш рейд 
```
sudo mkdir /mnt/01
```
```
mount /dev/md42 /mnt/01
```
![image](https://github.com/user-attachments/assets/61a32d39-4d60-4dd9-b010-ef4202436d10)


### Сымитируем вылет диска sdb
```
mdadm /dev/md42 --fail /dev/sdb
```
![image](https://github.com/user-attachments/assets/f6f800bd-f847-4f7c-9540-cffaeca20d07)

### Удалим "вылетевший" диск из рейда 
```
mdadm /dev/md42 --remove /dev/sdb
```
### Добавим диск обратно и дождёмся ребилда
```
sudo mdadm /dev/md42 --add /dev/sdb
```
![image](https://github.com/user-attachments/assets/ed9d090b-2661-48cc-80b5-2ef39c623173)


### Разберём рейд и грохнем суперблоки на дисках
```
sudo umount /mnt/01
```
```
sudo mdadm -S /dev/md42
```
```
sudo mdadm --zero-superblock /dev/sdb
```
```
sudo mdadm --zero-superblock /dev/sdc
```
### Создадим рейд заново, вместо монтирования директории создадим таблицу GPT разделов и 5 партишенов
```
sudo mdadm --create /dev/md42 -l 5 -n 2 /dev/sdb /dev/sdc
```
```
sudo mkfs.ext4 /dev/md42
```
```
sudo parted -s /dev/md42 mklabel gpt
```
```
sudo parted /dev/md42 mkpart primary ext4 0% 20%
```
```
sudo parted /dev/md42 mkpart primary ext4 20% 40%
```
```
sudo parted /dev/md42 mkpart primary ext4 40% 60%
```
```
sudo parted /dev/md42 mkpart primary ext4 60% 80%
```
```
sudo parted /dev/md42 mkpart primary ext4 80% 100%
```
### Создадим файловые системы в партишенах
```
for i in $(seq 1 5); do sudo mkfs.ext4 /dev/md42p$i; done
```

![image](https://github.com/user-attachments/assets/5e11ec0f-caa3-4e9b-891a-407729ed9b12)

### Смонтируем их по каталогам

```
sudo mkdir -p /raid/part{1,2,3,4,5}
```
```
for i in $(seq 1 5); do sudo mount /dev/md42p$i /raid/part$i; done
```
![image](https://github.com/user-attachments/assets/c3d75d79-983e-4c65-8782-950ce06ab205)

### Задание выполнено
