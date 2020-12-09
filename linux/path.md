# HOW TO SET THE PATH VARIABLE IN LINUX
```
when 
```

## Typical file

### 1. /etc/profile
 >This is the system wide initialization file executed during login. It usually contains enviroment variables, Including an  initail PATH and startup programs.
 
 >It not an good idea to change this file unless you know what you are doing. It's much better to create a `custom.sh` shell script in /etc/profile.d/ to make custom change to you enviroment. as this will prevent the need for merging in future updates. 

### 2. /etc/bashrc
>This is another system wide initailization file that may executed by user's `.bashrc` for each bash launched. It usually contains functions and aliases.

### 3. /etc/profile.d/

### 4. ~/bash_profile
>If this file exist,  it will automatically executed after `/etc/profile` during login.

### 5. ~/bash_login
>If `~/bash_login` not exist, it will automatically executed after `/etc/profile` during login.

### 6. ~/.profile
>If neither `~/.bash_profile` or `~/.bash_login` exist,  It will automatically executed during login. This is default file in debian-based distribution , such as ubuntu.

### 7. ~/.bashrc
>This file executed when a non-interactive shell starts, i.e, a new  terminal window in X. This  file is offen refered to in the bash interactive script. such as `~/.bashrc`


## What is the order of  execution of typical files.
- `/etc/profile`
> When bash is invoked as an interactive login shell, or as  an non-interacitve shell with a '--login' option. It first read and execut the command in `/etc/profile` , if that file exist.
- `~/.bash_profile` or `~/.bash_login` or `~/.profile`
>After read that file, It looks for `~/.bash_profile`, `~/.bash_login` ,`~/.profile` and read and executes command from the first one that exist and is readable
- `~/.bash_profile`
> if there is an file named `~/.bash_profile` and it's readable, `~/.profile` will be ignore.

## Which file should we modify
> As an general rule, to add directories to you `PATH` or define additional enviroment variables. palce those change in `~/bash_profile` (ubuntu use `~/.profile`) 
> for everything else, place change in `~/.bashrc`