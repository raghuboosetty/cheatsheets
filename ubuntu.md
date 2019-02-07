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