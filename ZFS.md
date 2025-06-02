# ZFS

### Имеем в наличии 8 дисков по 512 Мб

```
sudo lsblk
```
![image](https://github.com/user-attachments/assets/846751bb-0c89-4a23-97d1-10aa2e4425d8)


### Делаем z-pool-ы  raid 1

```
sudo zpool create otus1 mirror /dev/sdb /dev/sdc -f
sudo zpool create otus2 mirror /dev/sdd /dev/sde -f
sudo zpool create otus3 mirror /dev/sdf /dev/sdg -f
sudo zpool create otus4 mirror /dev/sdh /dev/sdi -f
sudo zpool list
```

![image](https://github.com/user-attachments/assets/feaabee0-3a92-4ba0-8418-385c1e5e73e6)


### Добавляем алгоритмы сжатия на каждый пул

```
sudo zfs set compression=lzjb otus1
sudo zfs set compression=lz4 otus2
sudo zfs set compression=gzip-9 otus3
sudo zfs set compression=zle otus4
zfs get all | grep compression
```
![image](https://github.com/user-attachments/assets/08890838-b156-43e6-b05b-43fa1b2be0f6)


### Скачиваем один файл во все пулы

```
for i in {1..4}; do sudo wget -P /otus$i https://gutenberg.org/cache/epub/2600/pg2600.converter.log; done
```
![image](https://github.com/user-attachments/assets/8adc09a4-09f4-4812-b548-28d9f79580be)


### Проверяем, что файл есть во всех пулах

```
sudo ls -l /otus*
```

![image](https://github.com/user-attachments/assets/7dce9010-fc0c-44eb-aa7b-20085843505a)

```
sudo zfs list
```
![image](https://github.com/user-attachments/assets/6023408e-f68a-41ad-aba6-f3cffa935903)

### Оптимальный метод сжатия - gzip-9

# Import

### Скачиваем через wget и разархивируем в /home файл zpoolexport, указанный в задании.
```
wget -O otus_task2.file --no-check-certificate https://drive.usercontent.google.com/download?id=1wgxjih8YZ-cqLqaZVa0lA3h3Y029c3oI&export=download
```

### Проверяем возможность импорта данного каталога в пул

```
sudo zpool import -d zpoolexport/
```

### Делаем импорт данного пула

```
sudo zpool import -d zpoolexport otus
```

### Проверяем пулы

```
sudo zpool status
```

![image](https://github.com/user-attachments/assets/6b14bd11-5efb-43e2-b407-babd24e1a46b)



# Snapshot


### Скачиваем файл, указанный в задании


### Восстанавливаем ФС из снэпшота

sudo zfs receive otus/test@today < otus_task2.file

### Ищем в каталоге файл "secret_message"

```
sudo find /otus/test -name "secret*"
```
```
cat /otus/test/task1/file_mess/secret_message
```
![image](https://github.com/user-attachments/assets/4c1c0709-2679-4f45-9fa9-73e581d1e522)



