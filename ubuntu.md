# Ubuntu

## List commands

```sh
# list commands
ls -lh   # permissions and file size
ls -lah  # permissions and file size on all files
ls -ltrh # permissions and file size, sorted on access time (newest is last)
ls -ld   # permissions, only directories

# directory size
du -sh directory # size in MB

# list recursive file sizes of files and directories in a directory
du --all --human-readable --apparent-size # size in Byte, KB, MB, GB ...

# archives cache size, used to cleaning disk
du -sh /var/cache/apt/archives

# command to get total disk space( free + available )
df -h
df -k
```

## Clean disk space
```sh
sudo apt-get clean
sudo apt-get autoclean
sudo apt-get autoremove
du -sh /var/cache/apt/archives
```

## System Information
```sh
sudo apt-get install landscape-common
landscape-sysinfo
```

## Build custom command
Make a bin directory in their home directory<br>
```sh 
mkdir ~/bin 
```
Put your custom scripts in there<br>
```sh 
mv command-script ~/bin 
```
Add this to the bottom of your ` ~/.bashrc`<br>
```sh
export PATH=$PATH:~/bin
```
Log back in and out of your terminal<br>
```sh 
bash --login
```

### Example
```sh
# create bin directory
mkdir ~/bin

# create executable file
sudo vi ~/bin/console

# add your commands to it
## cd /home/apps/vdate/current && bundle exec rails c production

# make the file executable
sudo chmod +x ~/bin/console

# add executable to $PATH
sudo vi ~/.bashrc
## export PATH=$PATH:~/bin
```

## Console Color Codes
```sh
# Black        0;30     Dark Gray     1;30
# Red          0;31     Light Red     1;31
# Green        0;32     Light Green   1;32
# Brown/Orange 0;33     Yellow        1;33
# Blue         0;34     Light Blue    1;34
# Purple       0;35     Light Purple  1;35
# Cyan         0;36     Light Cyan    1;36
# Light Gray   0;37     White         1;37


RED='\033[0;31m'
NC='\033[0m' # No Color
printf "I ${RED}love${NC} Stack Overflow\n"


echo -e '\033[0;32 logs cleared\033[0m'
```