# Содержание
+ [**GIT**](#GIT)
+ [**Virtualisation**](#Virtualisation)
    + [VirtualBox](#VirtualBox)
    + [Vagrand](#Vagrand)
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
+ [**Backup**](#Backup)
    + [bacula](bacula)


# Virtualisation

## Vagrand 

https://gist.github.com/wpscholar/a49594e2e2b918f4d0c4

Typing `vagrant` from the command line will display a list of all available commands.

Be sure that you are in the same directory as the Vagrantfile when running these commands!

### Creating a VM
- `vagrant init`           -- Initialize Vagrant with a Vagrantfile and ./.vagrant directory, using no specified base image. Before you can do vagrant up, you'll need to specify a base image in the Vagrantfile.
- `vagrant init <boxpath>` -- Initialize Vagrant with a specific box. To find a box, go to the [public Vagrant box catalog](https://app.vagrantup.com/boxes/search). When you find one you like, just replace it's name with boxpath. For example, `vagrant init ubuntu/trusty64`.

### Starting a VM
- `vagrant up`                  -- starts vagrant environment (also provisions only on the FIRST vagrant up)
- `vagrant resume`              -- resume a suspended machine (vagrant up works just fine for this as well)
- `vagrant provision`           -- forces reprovisioning of the vagrant machine
- `vagrant reload`              -- restarts vagrant machine, loads new Vagrantfile configuration
- `vagrant reload --provision`  -- restart the virtual machine and force provisioning

### Getting into a VM
- `vagrant ssh`           -- connects to machine via SSH
- `vagrant ssh <boxname>` -- If you give your box a name in your Vagrantfile, you can ssh into it with boxname. Works from any directory.

### Stopping a VM
- `vagrant halt`        -- stops the vagrant machine
- `vagrant suspend`     -- suspends a virtual machine (remembers state)

### Cleaning Up a VM
- `vagrant destroy`     -- stops and deletes all traces of the vagrant machine
- `vagrant destroy -f`  -- same as above, without confirmation

### Boxes
- `vagrant box list`              -- see a list of all installed boxes on your computer
- `vagrant box add <name> <url>`  -- download a box image to your computer
- `vagrant box outdated`          -- check for updates vagrant box update
- `vagrant boxes remove <name>`   -- deletes a box from the machine
- `vagrant package`               -- packages a running virtualbox env in a reusable box

### Saving Progress
-`vagrant snapshot save [options] [vm-name] <name>` -- vm-name is often `default`. Allows us to save so that we can rollback at a later time

### Tips
- `vagrant -v`                    -- get the vagrant version
- `vagrant status`                -- outputs status of the vagrant machine
- `vagrant global-status`         -- outputs status of all vagrant machines
- `vagrant global-status --prune` -- same as above, but prunes invalid entries
- `vagrant provision --debug`     -- use the debug flag to increase the verbosity of the output
- `vagrant push`                  -- yes, vagrant can be configured to [deploy code](http://docs.vagrantup.com/v2/push/index.html)!
- `vagrant up --provision | tee provision.log`  -- Runs `vagrant up`, forces provisioning and logs all output to a file

### Plugins
- [vagrant-hostsupdater](https://github.com/cogitatio/vagrant-hostsupdater) : `$ vagrant plugin install vagrant-hostsupdater` to update your `/etc/hosts` file automatically each time you start/stop your vagrant box.

### Notes
- If you are using [VVV](https://github.com/varying-vagrant-vagrants/vvv/), you can enable xdebug by running `vagrant ssh` and then `xdebug_on` from the virtual machine's CLI.

## **Network utilities**

### netstat
######   
```bash
netstat -lntup
netstat -tulnp | grep "nginx"  # show nginx port
netstat -tulnp | grep ":80"  # show service is used port 80
```
* `-l` указывает netstat вывести все прослушивающие сокеты (сокет это ip + порт)
* `-t` показывает все TCP-соединения
* `-u` отображает все соединения UDP
* `-p` позволяет выводить имя приложения/программы, прослушивающее порт

### ss
###### **ss is used to dump socket statistics. It allows showing information similar to netstat. It can display more TCP and state informations than other tools**. 
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
