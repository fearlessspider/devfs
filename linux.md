# Linux
## Size of a directory
```
$ du -sh /tmp
```
## Size of a file
```
$ du -h /tmp/xyz
```
## Python on ubuntu
```
sudo dd if=/dev/zero of=/swapfile count=4096 bs=1MiB
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
sudo apt-get update
sudo apt-get upgrade
sudo reboot
sudo apt-get install nginx php5 php5-fpm php5-gd mysql-client libmysqlclient-dev git libxml2-dev libxslt-dev python-dev python-setuptools libjpeg-dev libfreetype6-dev zlib1g-dev build-essential supervisor
sudo easy_install pip
sudo pip install virtualenv
```
## Add user with home
For command line, these should work:
```
useradd -m USERNAME
```
You have to use -m, otherwise no home directory will be created. If you want to specify the path of the home directory, use -d and specify the path:
```
useradd -m -d /PATH/TO/FOLDER USERNAME
```
You can then set the password with:
```
passwd USERNAME
```