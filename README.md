# devops-learning2
## table of contents
 - [Linux](#linux)
 - [Vagrant and Linux servers](#vagrant-and-linux-servers)
 - [Variables and yaml and json](#variables-and-yaml-json)
 - [VProfile project](#vprofile-project)
 - [Networking](#networking)
 - [Containers introduction](#containers-introduction)
 - [Bash scripting](#bash-scripting)
 - [AWS part 1](#aws-part-1)


## Linux
 - intro:
   - principles: everything is a file; small single purpose programs; able to chain programs together for complex operations; avoid captive user interface; configuration data stored in text file.
 - commands and file system
 - vim editor:
   - `:se nu`: set numbers for lines
   - `shift G`: last line
   - `gg`: first line
   - `(nums)yy`: copy line(s)
   - `p`: paste
   - `P`: paste above
   - `(nums)dd`: delete(cut) line(s)
   - `u`: undo
   - `/`: search
 - file types:
   - `file <filename>`
   - `l`: link
   - `b`: block (hard disk)
   - `c`: character
   - `s`: socket
   - `P`: pipeline
   - `ln -s`: create a soft link
   - `unlink`: delete the link
 - filter & IO redirection
   - grep -i
   - grep -i <firewall> * (not include directory)
   - grep -iR <firewall> *
   - grep -vi <firewall> (do not include any `firewall`)
   - less (a reader )
   - more
   - head -2 <filename>
   - tail -2 <filename>
   - tail -f <filename> (show the dynamic content if any changes made to the file)
   - cut -d<delimiter> -f1 /etc/passwd
   - awk -F':' '{print $1}' /etc/passwd  (an intelligent filter tool)
   - vim : %s/<pattern>/<replacement>/g ('g' means globally)
   - or using `sed`: sed 's/<pattern>/<replacement>' * (add `-i` to actually change the file)
   - I/O redirection:
     - `uptime > <filename>`
     - `uptime >> <filename>` (append the file)
     - `/dev/null` (nothing, like a black hole)
     - freeeee -m 2>> /tmp/error.log (`2` for standard error, `1` default value, standard output)
     - free -m &> /tmp/file.log (`&` for any output)
     - ls | wc -l < /etc/
     - find /etc -name host*
     - locate host (`updatedb` used to update the db for searching)
 - users & groups
   - /etc/passwd: users info, /etc/shadow: encrypted password , /etc/groups: groups info
   - id <userId>
   - useradd <username>
   - groupadd <groupname>
   - usermod -aG <groupname> <username>
   - passwd <username>
   - last (who logged into the system)
   - lsof -u <username> (find out what files opened by the user)
   - userdel <username> (delete the user, using `-r` to delete the directory)
   - groupdel <groupname>
 - file permissions:
   - <filetype><rwx--user><rwx--group><rwx--others>
   - chown -R <user>:<group>
   - chmod < u | g | o >< + | - >< r | w | x >  <filepath>
 - sudo:
   - visudo(root user) -- open sudo file in write mode in vim editor: `root ALL=(ALL)  ALL` (add a new line)--> `<username> ALL=(ALL) ALL` or `<username> ALL=(ALL) NOPASSWD: ALL` (no password asked when doing `sudo -i` with the user)
   - **note**: normally, we do not have root user and its password to edit the file /etc/sudoers, a better way is to edit or create the file in the directory /etc/sudoers.d/, for example: `%devops ALL=(ALL)  ALL`: % means group.
 - package management:
   - tree : shows the tree structure of a directory
   - `curl <link> -o <output file>` & then `rpm -iv <output file>`
   - using high-level package manager, like `yum`
 - services:
   - `systemctl`
 - processes:
   - `top`: showing all the dynamic processes based on their usage of cpu and mem.
   - `ps aux` similar to `top`
   - `ps -ef`
   - `kill <process id>` or `kill -9 <process id>`(only kill parent process)
   - for example: `ps -ef | grep httpd | grep -v 'grep' | awk '{print $2}' | xargs kill -9` (close orphan child processes)
 - archiving:
   - `tar -czvf <filename>.tar.gz <dir | file>`
   - `tar -xzvf <filename>.tar.gz` or `tar -xzvf <filename>.tar.gz -C <path>`
   - or `zip` command (needs to be installed first): (`zip`, `unzip`)
 - ubuntu commands:
   - instead of `useradd`, it is `adduser`
   - `visudo` --> will open nano editor, but can be changed by using `export EDITOR=vim`
   - package manager: `apt` : `apt remove <package>`, `<apt purge package>`

## Vagrant and Linux servers
 - so far concepts: vagrant cloud(VM images), vagrantfile(VM settings), vagrant commands: `vagrant <up|status|ssh|halt|destroy>`
 - vagrant networking, provisioning, RAM/cpu etc, multi-vm, documentation
 - vagrant sync directories
 - vagrant provisioning
 - vagrant website setup
 - vagrant wordpress setup
 - automate website setup
 - automate wordpress setup
 - multi-vm

## Variables and yaml and json
 - variables, (make sure using double quotes, otherwise it will print as-is if using single quotes)
 - python DS: string, integer, list, tuple and dictionary
 - json
 - yaml

## VProfile project
 - vm setup
 - db, cache & queue setup --> `continue...`
 - app setup
 - nginx setup
 - validation
 - automation

## Networking
 - components of network: computers, cables, network interfacing card, switches, routers, OS
 - ISO -> developed OSI model: (physical:bits, data link:frames, network:packets, transport:segments, session:data, presentation:data, application:data)
   - services: a set of actions that a layer offers to a higher layer
   - protocols: a set of rules that a layer uses to exchange information
   - interfaces: a interface is communication between the layers
 - networks & IP:
   - LAN: local area network
   - WAN: wide area network
   - MAN: metropolitan area network
   - CAN: campus area network
   - PAN: personal area network (hotspot)
   - switches: connect multiple computers together in a small network
   - routers: connect multiple networks together
   - private ip range in aws vpc:
     - class A 10.0.0.0/8
     - class B 172.16.0.0/12
     - class C 192.168.0.0/16
 - protocols and ports:
   - protocols: a formal spec that defines the procedures that must be followed when transmitting or receiving data. Protocols define the format, timing, sequence, and error checking used on the network.
   - ports: 22, 21, 80, 443, 53
 - network commands: `ifconfig`, `ping`,`tracert`, `netstat -antp`: showing all tcp open ports on the local machine, `ss -tunlp`, `nmap`, `dig`, `nslookup`, `route -n`, `arp`, `mtr`(live version of tracert), `telnet`,

## Containers introduction
 - microservices:  an architectural and organiztional approach to software development where software is composed of small independent services that communicate over well-defined apis. these services are owned by small, self-contained teams.

## Bash scripting
 - variables:  `SKILL="ASDF", echo $SKILL, or $ART_NAME.zip(with file extension)` : no space in-between !!!!
 - command line arguments: `$0 is the name of the script`, `$1 - $9 the first 9 arguments to the bash script`
 - system variables:
   - `$#`: how many arguments were passed to the script
   - `$@`: all arguments supplied to the script
   - `$?`: exit status of the most recently run process
   - `$$`: the process id of the current script
   - `$USER`: the username of the user who runs the script
   - `$HOSTNAME`: the hostname of the machine that script runs on
   - `$SECONDS`: the seconds since the script starts
   - `$RANDOM`: return a random number each time it is referred to
   - `$LINENO`: return the current line number of the script
 - quotes: referring to an argument in single quotes will lose its meaning:`'this is not okay $SKILL'`. to escape some special characters, like `$`, using `\` to escape.
 - **note**: exit code: `0` means success, `1` means fail
 - command substitution: using "`" to store the output to an argument, like "UP=`uptime`" or using "$(uptime)", such as "FREE_MEM=`free -m | grep -i mem | awk '{print $4}'`" 
 - exporting variables: exporting variables makes variables globally available for any child shell. the file `.bashrc` in home directory can be used to store `export <variable>="<value>"`, thus the variable will persists between user sessions. or if wanna make the variables globally for any users, then `vi /etc/profile`. **note**: `.bashrc` will override `/etc/profile`
 - user input: `read val1`, `read -p '(prompt)' val1`, `read -sp '(prompt)' val1 `(hide while typing)
 - decision-making / conditions:
   - `if [ $NUM -gt 100 ] then ... else ... fi` (**note**: be aware of the spaces!!!!!!!)
   - `if [ ] then ... elif [ ] ... else ... fi`
 - monitoring scripts:  using `crontab`(install cronie for fedora), then create a cron job
 - loops/ while loops:
   - `for var1 in item1 item2 item3 item4 ... do ... done`
   - `while [ ] do ... done` , **NOTE**: The double parentheses (( )) in shell scripting are used for arithmetic operations. 
 - remote command execution: since multiple virtual machine running on the same host, so on a virtual machine, we modify its `/etc/hosts` file to add other virtual machines so that we can refer to them later (vagrant user, vagrant password). **note**: if the login method is `public key` not password, then go to `/etc/ssh/sshd_config`: change `passwordauthentication` (no --> yes). lastly, doing `ssh <user>@host <command>` to execute the command remotely without logging onto another virtual machine.
 - ssh key exchange: `ssh-keygen`--> `id_rsa`, `id_rsa.pub`, then `ssh-copy-id <user>@host`, then save time typing password each time doing remote command execution.
 - using `scp` to securely copy files from one vm to another, `scp <file> <user>@host:<path>`

## AWS part 1
 - ec2 instance:
   - choose AMI
   - choose instance type
   - configure the instance
   - adding storage
   - adding tag
   - configure security group
 - ebs volume:
   - block based storage, runs ec2 os, store data from db, file data, etc. az-specific
   - general purpose(ssd), provisioned IOPS(large databases), throughput optimized HD, gold HDD (file servers), magnetic (backups & archives)
   - after attaching a block storage to ec2 instance, we need to create a partition and then formatting it. `fdisk -l` to list all disks on your linux virtual machine. `df -h`: Show information about the file system on which each FILE resides, or all file systems by default. then `fdisk /dev/xvdb` to go to the partition setttings. for formatting, we use `mkfs`, two taps then choose `mkfs.ext4 <partition path--/dev/xvdb1>`.  and then it is time to mount it, `mount <partition path -- /dev/xvdb1>  <the mount path -- /var/www/html/images>`, or unmount it using `umount <the mount path -- /var/www/html/images>`.  for permanent mount, vim the file `/etc/fstab`, and append `/dev/xvdf1 /var/www/html/images  ext4 defaults(settings) 0(no dumping) 0(no file system check)`. then `mount -a` to mount all entries.
   - snapshot: `unmount partition`(prevent data from being overwritten), `detach volume`, `create a new volume`, `attach the new volume`, `mount it back`
 - elb load balancer:
   - ALB: layer7, route traffic based on advanced app level info that includes in the content of the request. it is used for http or https requests.
   - NLB: layer4, static IP, handle millions of requests
   - target group
   - ami (image builder)
   - launch template
 - cloudwatch
   - monitoring performance of aws services using metrics
   - metrics: can set alarms on (integrated with SNS for notifications)
   - events
   - logs
   - `stress command`: `nohup stress -c 4 -t 300`
 - EFS
   - 










