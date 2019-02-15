# RVM

## To load rvm with login-bash shell
```sh
rvm get stable --auto-dotfiles
```

## Create and use gemset
```sh
rvm use ruby-x.x.x@gemset --create

# Examples
rvm use ruby-2.3.0-p0@gemset --create
rvm --rvmrc --create ruby-2.3.0-p0@gemset
```

## List
```sh
rvm list         # list rubies only
rvm list gemsets # list rubies and gemsets
rvm gemset list  # list gemsets for current ruby
```

## Check & install latest udpates
```sh
rvm requirements
```

## Temporarily selecting another Ruby or gemset
```sh
rvm 1.8.7 do gem install rspec       # in the given ruby
rvm 1.8.7, 1.9.2 do gem install haml # in this two rubies
rvm @global do gem install gist      # in @global gemset of current ruby
```
