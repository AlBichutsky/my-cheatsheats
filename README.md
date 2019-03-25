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
+ [**RAID**](#RAID)
    + [Creating a new RAID-array](#RAID-array)
    + [/etc/mdadm.conf](#mdadm.conf)
    + [Verifying the status of the RAID-array](#arrays)
    + [Remove and add a disk in array](#Remove)
+ [**LVM**](#LVM)
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

# **RAID**

### Creating a new RAID-array
- `yum install -y mdadm`  -- Installing mdmadm
- `mdadm --create --verbose /dev/md0 --level=1 /dev/sda1 /dev/sdb2`   -- Create new RAID-array level 1
- `mdadm --create --verbose /dev/md0 -l 10 -n 4 /dev/sd{b,c,d,e}`   -- Create new RAID-array level 10 

### /etc/mdadm.conf
###### /etc/mdadm.conf or /etc/mdadm/mdadm.conf (on debian) is the main configuration file for mdadm in old versions fo Linux. After we create our RAID arrays we can add configuration of RAID in mdadm.conf.
- `mdadm --detail --scan >> /etc/mdadm.conf` -- use on Centos
- `mdadm --detail --scan >> /etc/mdadm/mdadm.conf` -- use on Debian 

### Verifying the status of the RAID arrays
- `cat /proc/mdstat`
- `mdadm --detail /dev/md0`
- `watch cat /proc/mdstat`   -- While monitoring the status of a RAID rebuild operation using `watch` can be useful

### Remove and add a disk from an array
````bash
# We can’t remove a disk directly from the array, unless it is failed. 
# so we first have to fail it (if the drive it is already failed this step is not needed):
mdadm --fail /dev/md0 /dev/sda1

# And now we can remove it:
mdadm --remove /dev/md0 /dev/sda1

# Add a disk to an existing array
# We can add a new disk to an array (replacing a failed one probably):

mdadm --add /dev/md0 /dev/sdb1
````

# **Network utilities**

### netstat
######   

- `netstat -lntup`
- `netstat -tulnp | grep "nginx"`  -- show nginx port
- `netstat -tulnp | grep ":80"`  -- show service is used port 80

* `-l` указывает netstat вывести все прослушивающие сокеты (сокет это ip + порт)
* `-t` показывает все TCP-соединения
* `-u` отображает все соединения UDP
* `-p` позволяет выводить имя приложения/программы, прослушивающее порт

### ss
###### **ss is used to dump socket statistics. It allows showing information similar to netstat. It can display more TCP and state informations than other tools**. 
`ss -tulnp`
`ss -tulnp | grep "8080"`
`ss -tulnp | grep "java"`

### nmap
###### 

`apt install nmap`  -- On Debian/Ubuntu
`yum install nmap`  -- On CentOS/RHEL
`dnf install nmap`  -- On Fedora 22+
`nmap -n -PN -sT -sU -p- localhost`

### lsof
######
`lsof -i`
`lsof -i :80`
