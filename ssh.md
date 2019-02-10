# SSH

## See known hosts
```sh
sudo vi ~/.ssh/known_hosts
```
## SSH to server without password [how to add your public key]
```sh
# copy the public key
cat ~/.ssh/id_rsa.pub

# ssh to server
ssh username@domain.com

# paste the public key in the known hosts
sudo vi ~/.ssh/authorized_keys

# further check with SSH shortcuts(last section) to ease the process of SSH to server
```

## Generate new default/root SSH keys
```sh
# create ssh-key pair for git
ssh-keygen -t rsa -b 4096 -C "raghuboosetty@gmail.com"

# add the public key to github
cat ~/.ssh/id_rsa.pub
# copy the public key and paste it in the SSH of your SVN(Github)

# clone the app
git clone git@github.com:raghuboosetty/sample.git

# further
git config user.name "raghu"
git config user.email "raghuboosetty@gmail.com"
```

## Managing multiple SSH keys for cloning applications using Git
```sh
# create another ssh-key pair
ssh-keygen -t rsa -b 4096 -C "raghuboosetty@gmail.com"

# add the file name e.g: "id_rsa_raghu"
ssh-add ~/.ssh/id_rsa_raghu
ssh-add -D
ssh-add -l

# add the public key to github
cat ~/.ssh/id_rsa_raghu.pub
# copy the public key and paste it in the SSH of your SVN(Github)

# config keys with host
cd ~/.ssh/
sudo touch config
sudo vi config

# Then add following(github name can be anything its upto you)
Host github.com-raghu
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_raghu

# Note the name at the end of github its pointing towards the key
git clone git@github.com-raghu:raghuboosetty/sample.git


# further
git config user.name "raghu"
git config user.email "raghuboosetty@gmail.com"

# Source: https://gist.github.com/jexchan/2351996
```
### My detailed blog post: 
https://raghuror.wordpress.com/2015/10/10/managing-multiple-ssh-keys/

## SSH shortcuts [how to]
```sh
mate ~/.ssh/config
# OR
sudo vi ~/.ssh/config

# Sample Settings
# 
# Host localbiz
#    HostName 54.244.249.44
#    User ubuntu
# 
# Host localbiz-crawler
#    HostName 54.203.250.94
#    User ubuntu
# 
# Host jobsin
#    HostName 54.227.236.62
#    User ubuntu
# 
# Host jobsin-crawler
#    HostName ec2-54-166-74-90.compute-1.amazonaws.com
#    User ubuntu
#    IdentityFile /Users/reonios/Desktop/jobsin/jobsin_imp/jobsin_east.pem
```