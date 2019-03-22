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
    + [syslog](syslog)
    + [elastic search](#elastic#search)


## **Network utilities**

### netstat
######   
```bash
netstat -lntup
netstat -tulnp | grep "nginx"  # узнать какой порт слушает nginx
netstat -tulnp | grep ":80"  # узнать, какой сервис использует порт 80
```
* `-l` указывает netstat вывести все прослушивающие сокеты (сокет это ip + порт)
* `-t` показывает все TCP-соединения
* `-u` отображает все соединения UDP
* `-p` позволяет выводить имя приложения/программы, прослушивающее порт

### ss
###### **Анализ сетевых соединений (вместо устаревшей netstat)**
```bash
ss -tulnp
ss -tulnp | grep "8080"
ss -tulnp | grep "java"
```

### nmap
###### 
```bash
sudo apt install nmap  # On Debian/Ubuntu
sudo yum install nmap  # On CentOS/RHEL
sudo dnf install nmap  # On Fedora 22+
nmap -n -PN -sT -sU -p- localhost  # просканировать все доступные порты на localhost
```

### lsof
######
```bash
lsof -i  # вывести все интернет-файлы и сетевые файлы
lsof -i :80
```
