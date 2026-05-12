---
tags: [common]
aliases: [navigation]
---

* Windows -> only for **Powershell** (some flags aren't working)
* Linux -> by default
# navigation
* `.` - is current dir
* `~` - home dir
	* Windows -> `C:/users/<CurrentUserName>/` (if OS is installed on C)
	* Linux -> `/home/<CurrentUserName>`
* `/` - is single symbol, only for Linux, it's a root dir

| intention                    | command      | examples                                                              | detailed link |
| ---------------------------- | ------------ | --------------------------------------------------------------------- | ------------- |
| list of files in current dir | `ls`         | `ls`<br>`ls .`<br>`ls ..`<br>`ls existing_dir`                        |               |
| move to folder               | `cd <where>` | `cd existing_dir`<br>`cd existing_dir/`<br>`cd ./existing_dir/`       |               |
| move "up"                    | `cd ..`      | `cd ..`<br>`cd ../`<br>`cd ../existing_dir/`<br>`cd ../../../../`<br> |               |
| move to root                 | `cd ~`       | `cd ~/.gitconfig`                                                     |               |
# file modification

| intention   | command                                            | examples                                                 | detailed link |
| ----------- | -------------------------------------------------- | -------------------------------------------------------- | ------------- |
| move file   | `mv <file> <where>/`<br>* trailing slash for dirs* | `mv my-script.py new-existing-folder/`                   |               |
| rename file | `mv <old name> <new name>`                         | `mv HELP.md README.md`<br>`mv old-dir-name new-dir-name` |               |
