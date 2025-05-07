# Обновление ядра системы
sudo apt update
sudo apt upgrade  #проверяем версию ядра
wget https://raw.githubusercontent.com/pimlie/ubuntu-mainline-kernel.sh/master/ubuntu-mainline-kernel.sh
chmod +x ubuntu-mainline-kernel.sh
sudo mv ubuntu-mainline-kernel.sh /usr/local/bin/
ubuntu-mainline-kernel.sh -c
sudo ubuntu-mainline-kernel.sh -i
sudo reboot /t:0
