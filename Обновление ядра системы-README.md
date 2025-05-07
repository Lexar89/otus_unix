# Обновление ядра системы
```
sudo apt update
```
```
sudo apt upgrade
```

### проверяем версию ядра
### установлено ядро 6.8.0, оно далеко не последнее.
___
```
wget https://raw.githubusercontent.com/pimlie/ubuntu-mainline-kernel.sh/master/ubuntu-mainline-kernel.sh
```
```
chmod +x ubuntu-mainline-kernel.sh
```
```
sudo mv ubuntu-mainline-kernel.sh /usr/local/bin/
```
```
ubuntu-mainline-kernel.sh -c
```
```
sudo ubuntu-mainline-kernel.sh -i
```
```
sudo reboot /t:0
```
### установлена более новая версия ядра

![image](https://github.com/user-attachments/assets/677b3b6f-b87c-46c6-bc33-520e1f188965)

___

### Попробуем поставить более новую

```
wget https://kernel.ubuntu.com/mainline/daily/current/amd64/linux-headers-6.15.0-061500rc5daily20250507-generic_6.15.0-061500rc5daily20250507.202505070223_amd64.deb
```
```
wget https://kernel.ubuntu.com/mainline/daily/current/amd64/linux-image-unsigned-6.15.0-061500rc5daily20250507-generic_6.15.0-061500rc5daily20250507.202505070223_amd64.deb
```
```
wget https://kernel.ubuntu.com/mainline/daily/current/amd64/linux-modules-6.15.0-061500rc5daily20250507-generic_6.15.0-061500rc5daily20250507.202505070223_amd64.deb
```
```
sudo dpkg -i *.deb
```
```
update-grub
```
### установлена ещё более новая версия ядра
![image](https://github.com/user-attachments/assets/b3a4792b-032c-45f8-a392-25f2bca7bb1e)

___

### Настроим возможность выбора ядра при загрузке виртуалки.

```
sudo apt install nano
```
```
nano /etc/default/grub
```
### Вносим изменения в файл чтоб он выглядел таким образом:
![image](https://github.com/user-attachments/assets/86b28e96-bc5e-4e90-875c-ce03ccdfb622)

### Перезагружаемся
![image](https://github.com/user-attachments/assets/efb17e7b-eb39-4721-b619-53803bff35a1)




