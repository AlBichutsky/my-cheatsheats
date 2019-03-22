## **netstat**
аргумент      | Описание
------------- | -------------
-l            | указывает netstat вывести все прослушивающие сокеты (сокет это ip + порт)
-t            | показывает все TCP-соединения
-u            | отображает все соединения UDP
-p            | позволяет выводить имя приложения/программы, прослушивающее порт


```
$ netstat -lntup
$ netstat -lntup | grep "nginx"
$ netstat -lntup | grep ":80"
```

## **ss** 

```
$ ss -lntu
```

## **nmap** 
```
$ sudo apt install nmap  # On Debian/Ubuntu
$ sudo yum install nmap  # On CentOS/RHEL
$ sudo dnf install nmap  # On Fedora 22+
$ nmap -n -PN -sT -sU -p- localhost  # просканировать все доступные порты на localhost
```

## **lsof**
```
$ lsof -i  # вывести все интернет-файлы и сетевые файлы
$ lsof -i :80
```
