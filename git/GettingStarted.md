# GIT COMMANDS
## Settings
### 1.Your Identity
```
git config --global user.name "zhu.wenjian"
git config --global user.email "zhu.wenjian@cesgroup.com.cn"
```
### 2.Your Editor
1. emacs
```
git config core.editor emacs
```
2. notepad++
```
git config --global core.editor "'/path/to/notepad++' -multiInst -notabbar -nosession -noPlugin"
```
### Your Default Branch Name
```
git config --global init.defaultBranch "branch name"
```
### Check you settings
```
git config --list
```
## Help
If you ever need help while using git, there are three equivalent ways to get conprehensive manual page help for any of the git commands.
- git  \<verb>  --help
- git help \<verb>
- man git-\<verb>

for example:
```
git config --help
git help config
man git-config
```

In addition, if you don't need a full-blown manpage help, but just need a quick refresher on the variable option for a git command. you can ask for the more concise "help" output with the option `-h`.such as:
```
git add -h 
```
## Create A Repository
### 1. Initializing a Repository
```
git init
```
### 2.Clone a exist Repository
```
git clone https://repository.path folder_name
```
note: Git has numbers of different transfer protocols you can use:
  - https:
  - git@
  - user@server:/path/to/repo.git

## Ignore file
### 1.create a .gitignore file
>Often, you’ll have a class of files that you don’t want Git to automatically add or even show you as being untracked. These are generally automatically generated files such as log files or files produced by your build system. In such cases, you can create a file listing patterns to match them named .gitignore. Here is an example `.gitignore` file:
### 2.add patterns to file
Here is another example .gitignore file:
```
# ignore all .a files
 *.a

# but do track lib.a, even though you're ignoring .a files above
 !lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
 /TODO

# ignore all files in any directory named build
 build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory and any of its subdirectories
 doc/**/*.pdf
 ```
 