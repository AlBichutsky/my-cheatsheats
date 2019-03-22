# Содержание

+ [**Network utilities**](#Network#utilities)
    + [netstat](#netstat)
    + [ss](#ss)
    + [nmap](#nmap)
    + [lsof](#lsof)
+ [**Logging**](#Logging)
    + [lnav](lnav)
    + [tail](tail)
    + [cat](cat)
    + [less](less)
    + [zcat](zcat)


## **Network utilities**

#### netstat
######   
```bash
netstat -lntup
netstat -lntup | grep "nginx"  # узнать какой порт слушает nginx
netstat -lntup | grep ":80"  # узнать, какой сервис использует порт 80
```
* -l указывает netstat вывести все прослушивающие сокеты (сокет это ip + порт)
* -t показывает все TCP-соединения
* -u отображает все соединения UDP
* -p позволяет выводить имя приложения/программы, прослушивающее порт

#### ss
######
```bash
ss -lntu
```

##### nmap
###### 
```bash
sudo apt install nmap  # On Debian/Ubuntu
sudo yum install nmap  # On CentOS/RHEL
sudo dnf install nmap  # On Fedora 22+
nmap -n -PN -sT -sU -p- localhost  # просканировать все доступные порты на localhost
```

#### lsof
######
```bash
lsof -i  # вывести все интернет-файлы и сетевые файлы
lsof -i :80
```
