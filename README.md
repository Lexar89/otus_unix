#Файловые системы и LVM-1

### Проверим наши диски

```
sudo lsblk
```
![image](https://github.com/user-attachments/assets/4d5ec35f-a175-4eaf-a054-8dba43438936)

### Имеем два тестовых блочных устройства sdb и sdc по 10 Гб каждый.
### Создадим PV на диске sdb:
```
sudo pvcreate /dev/sdb
```
### Создадим VG и добавим в неё диск sdb:
```
sudo vgcreate otus_test /dev/sdb
```
### Далее создаем LV размером 8 Гб:
```
sudo lvcreate -l+80%FREE -n otus1 otus_test
```
### Проверяем Volume Group:
```
sudo vgdisplay otus_test
```
![image](https://github.com/user-attachments/assets/013e774b-e691-4826-8f4c-c159ab474422)

### Видим, что 2 Гб свободно, создадим ещё один LV:
```
sudo lvcreate -L2000M -n otus1_small otus_test
```

![image](https://github.com/user-attachments/assets/a3517f6d-d96f-4314-a647-57db734213e0)


### Создадим на LV файловые системы и смонтируем директорию
```
sudo mkfs.ext4 /dev/otus_test/otus1
```
```
sudo mkfs.ext4 /dev/otus_test/otus1_small
```
```
sudo mkdir /data
```
```
sudo mount /dev/otus_test/otus1 /data
```
### Займёмся расширением LVM
### Добавим PV на диск sdc
```
sudo pvcreate /dev/sdc
```
### Добавим диск sdc в VG otus_test
```
sudo vgextend otus_test /dev/sdc
```
### Проверим, что новый диск добавился в нужную VG
```
sudo vgdisplay -v otus_test
```
![image](https://github.com/user-attachments/assets/34784b83-2e30-4e77-8722-8a08fc0fb42d)


```
sudo vgdisplay -v otus_test | grep 'PV Name'
```
![image](https://github.com/user-attachments/assets/28b34526-df3f-4020-b8a1-9966684de2a9)

```
sudo vgs
```
![image](https://github.com/user-attachments/assets/2d0801a8-3cf8-4a0f-81be-fdd9e6b2a0e9)

### Увеличим LV otus1 за счёт появившегося свободного места
```
sudo lvextend -l+70%FREE /dev/otus_test/otus1
```
```
sudo lvs /dev/otus_test/otus1
```
![image](https://github.com/user-attachments/assets/5d40062d-7c1a-4358-9a3f-5fae12240a9f)


### Уменьшим LV otus1 до 10 Гб
### Отмонтируем диск от директории /data
```
sudo umount /data
```
### Проверим фс на ошибки
```
sudo e2fsck -fy /dev/otus_test/otus1
```
![image](https://github.com/user-attachments/assets/55e882b4-1fac-48da-a1ab-7ab18a9ff801)

### Выполним ресайз фс до 10 Гб
```
sudo resize2fs /dev/otus_test/otus1 10G
```

### Уменьшим размер LVM otus1 до 10 Гб
```
sudo lvreduce /dev/otus_test/otus1
```
### Маунтим lvm otus1 обратно в директорию /data
### Проверим размеры ФС и LVM
```
sudo df -Th /data
```
![image](https://github.com/user-attachments/assets/6d0a7825-a0b8-4e9f-90b2-142954dcc2bf)

```
sudo lvs /dev/otus_test/otus1
```
![image](https://github.com/user-attachments/assets/c5fa5086-2097-4607-b418-0c5a39bdd1f8)

