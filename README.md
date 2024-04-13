# devops-learning2
## table of contents
 - [Linux](#linux)
 - [Vagrant and Linux servers](vagrant-and-linux-servers)
 - [Variables and yaml and json](variables-and-yaml-json)
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










