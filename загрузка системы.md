# Загрузка системы
### Имеем две Ubuntu c адресами 10.129.0.29 и 10.129.0.26

### Делаем первую сервером (10.129.0.29)

```
sudo apt install nfs-kernel-server
```
### Создаём директорию, которую будем шарить
```
mkdir -p /srv/share/upload
```
```
chown -R nobody:nogroup /srv/share
```
```
chmod 0777 /srv/share/upload
```
### Добавляем в /etc/exports запись о расшаренной директории

```
cat << EOF > /etc/exports 
/srv/share 192.168.50.11/32(rw,sync,root_squash)
EOF
```
### Экспортируем  нашу директорию
```
exportfs -r
```
### Теперь настраиваем клиента (10.129.0.26)

```
sudo apt install nfs-common
```

### Стартуем сервис и получаем ошибку
```
sudo systemctl start nfs-common
```
![image](https://github.com/user-attachments/assets/95be9b2e-fce1-402f-9115-ebe29620c4a2)

### Сервис с маской. Проверяем куда идёт симлинк
```
sudo systemctl enable nfs-common
```
```
file /lib/systemd/system/nfs-common.service
```
![image](https://github.com/user-attachments/assets/b1fa230e-e43e-416d-9c12-bb3755fc94b9)

### Удаляем файл, чиним сервис и стартуем
![image](https://github.com/user-attachments/assets/1456c7a6-3f0f-4157-ab36-a51c5fe3bfb4)

### Монтируем на клиенте директорию и проверяем
```
sudo mount 10.129.0.29:/srv/share /mnt
```
![image](https://github.com/user-attachments/assets/12599e5b-3478-447d-aa48-daa97c32a685)



