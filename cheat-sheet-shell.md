# cheat-sheet-shell

This is not a linux tutorial, but there are a few commands which you need often when using a shell.
That's my personal frequently used commands

## get basic host infos

    hostname -f
    uname -a
    cat /proc/version
    uptime
    ifconfig

## DNS and hostname resoultion

/etc/hosts - hosts files are checked before DNS, e.g.

    127.0.0.1       localhost.localdomain   localhost another.name
    100.0.100.12    username.example.com   username    

CNAME DNS records make it possible to redirect requests for one hostname or domain to another hostname or domain. 

## email

simpliest solution is postfix (will be installed at  /usr/sbin/sendmail)

## validate whether another host is reachable

    $ host machine_name_or_ip

    # domain information groper
    $ dig www.amazon.com
    $ dig -x 100.42.30.95

    $ ping google.com
    $ ping 100.0.100.12
    # Round Trip Time = rtt

    $ traceroute google.com
    # * * * are failed jumps

    $ tracepath machine_name_or_ip

    $ sudo mtr google.com

## monitoring

    # cpu info
    cat /proc/cpuinfo

    # memory
    free -m
    cat /proc/meminfo

    # memory and io
    vmstat 1 20
    #  wa = CPU spends waiting for I/O operations to complete, should be 0

    # Monitor Processes, Memory, and CPU Usage
    htop

    # processes
    ps -auxw
    pstree -p 

    # processors 
    mpstat -A

    # used ports
    netstat -a | more

    # Processes attached to open files or open network ports
    lsof

    # disk
    # report filesystem disk space usage
    df -k
    # space usage for a given director


## copy files to/from server

    # scp [source] [destination]
    #
    # copy to another host
    # scp [/path/to/local/file] [remote-username]@[remote-hostname]:[/path/to/remote/file]
    # e.g. 
    scp test.tar.gz username@hostname.example.com:/home/username/testfolder/
    #
    # copy from another host
    scp username@hostname.example.com:/home/username/testfolder/test.tar.gz test.tar.gz 


## symbolic links

    ln -s [/path/to/target/file] [/path/to/location/of/sym/link]
    
## package management

    # ubuntu / debian - list installed - search - details - install - remove
    sudo dpkg -l
    sudo apt-cache search [package-name]
    sudo apt-cache show [package-name]
    sudo apt-get install [package-name]
    sudo apt-get remove package-name

    # centos / fedora - list installed - search - details - install - remove
    sudo yum list installed
    sudo yum search [package-name]
    sudo yum info [package-name]
    sudo yum install [package-name]
    sudo yum remove package-name

## edit text


    # search for text in file
    grep "^Subject:.*HELP.*" /home/username/mbox
    grep -i "morris" ~/org/*.txt

    # replace
    # 's/[regex]/[replacement]/'
    sed -i `s/^old/new/` test.txt

    # view view from start 
    less filename.txt

    # view files page by page
    more filename.txt

    # edit file with vi
    vi /etc/hosts
    # :wq write and exit
    # i insert mode
    # back to default mode
    # dd delete line
    # x delete character

    # head / tail of a file 
    head file
    tail file
    tail -f file
    tail -n 200 file

    # Word count -w for words, -l for lines
    wc file.txt

    # diff files
    diff file1.txt file2.txt

    # text within files
    grep this_word this_file.txt 



## user management

    # logged in users
    users

    # logged in users and idle time
    who -uH
    
    
    # currently logged in users and processes they are running.
    w

    # create user
    useradd user_name 
    # -d HOME_DIR
    # -m add homedirectory
    # default group -g
    # -G grp1, grp2 additional groups
    # -s default shell
    # user can be created directly in file /etc/passwd as well

    # delete user
    userdel user_name

    # change passwd
    passwd user_name

    # set user limits
    /etc/security/limits.conf
    /etc/security/access.conf
    /etc/security/group.conf
    /etc/security/time.conf

    # Switch user account to root
    su -

    # switch to user
    su username

    # other stuff
    whoami
    groups
    env
    id
    last


## shell history    

    # history
    history
    history n

    # Searching through the Command History 
    CTRL-R

## find

    # Search and list all files from current directory and down for the string ABC: 
    find ./ -name "*" -exec grep -H ABC {} \; 
    find ./ -type f -print | xargs grep -H "ABC" /dev/null 
    egrep -r ABC *

    # Find all files of a given type from current directory on down: 
    find ./ -name "*.conf" -print

    # Find all user files larger than 5Mb: 
    find /home -size +5000000c -print

    # Find all files owned by a user (defined by user id number. see /etc/passwd) on the system: (could take a very long time) 
    find / -user 501 -print

    # Find all files created or updated in the last five minutes: (Great for finding effects of make install) 
    find / -cmin -5

    # Find all users in group 20 and change them to group 102: (execute as root) 
    find / -group 20 -exec chown :102 {} \;

    # Find all suid and setgid executables: 
    find / \( -perm -4000 -o -perm -2000 \) -type f -exec ls -ldb {} \; 
    find / -type f -perm +6000 -ls

    # Find location/list of files which contain a given partial name (refresh index with updatedb)
    locate/slocate

    # Find executable file location of command given. Command must be in path.
    which

    # Find executable file location of command given and related files
    whereis	

## files, owners and rights

    # List directory contents. List file information
    ls	

    # Change file access permissions
    chmod	 

    # Change file security so that the user, group and all others have read, write and execute privileges
    chmod ugo+rwx file-name

    # Remove file access so that the group and all others have write and execute privileges revoked/removed
    chmod go-wx file-name 

    # r (read) = 4 w (write) = 2 x (execute) = 1, user - group - other
    chmod 521 somefile

    # Change file owner and group
    chown

## tar

   # tar
   tar -z -cvf /.../...tar.gz -C /aFolder

   # list content
   tar -tzf /../...tar.gz   

   # untar
   tar -xzf /.../...tar.gz

   # -c --- create
   # -v --- verbose
   # -f --- file 
   # -z --- put the file though gzip or use gunzip on the file first.
   # -x --- extract the files from the tarball.

## se linux

Security Enhanced Linux (SELinux) enhancements developed by the US Federal National Security Agency (NSA)

    # config in file
    /etc/selinux/config
    
    # enable
    setenforce 1

    # disable
    setenforce 0

## Virtual Terminals and screen

    # switch between multiple virtual terminals on the one physical terminal
    screen

    # create new virtual terminal
    CTRL-A and C

    # next, previous terminal
    CTRL-N , CTRL-P

## stream pipes and so on

    # concatenate files together into a new one
    cat file1.txt file2.txt > new.txt

    # append to file
    >>

    # output of a program to a file and to standard output
    ls /home | tee directories.txt

    # execute command_2 and it's output will become the input to command_1
    command_1 $(command_2)
    command_2 | command_1

## cron


    # edit crontabs
    crontab -e
    # e.g. 5 4 * * sun echo "run at 5 after 4 every sunday"


    # list crontabs
    crontab -l  

## other stuff

    # Ssh pre-login Text
    /etc/issue

    # current dir
    pwd

    # services in /etc/init.d
    service x status / stop / start / restart
    
