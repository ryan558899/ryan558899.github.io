---
title: OverTheWire - Bandit解題筆記
date: 2025-12-17 21:00:00 +0800
categories: [筆記]
tags: [資安, 解題]
permalink: /posts/OverTheWireBandit/
---

[OverTheWire - Bandit](https://overthewire.org/wargames/bandit/bandit0.html)
===
## Level0
### Level Goal
The password for the next level is stored in a file called readme located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.
### 解答
- `ssh -p 2220 bandit0@bandit.labs.overthewire.org`連進bandit0主機
- `ls`看到檔案`readme`
- `cat`檔案拿到flag

## Level1
### Level Goal
The password for the next level is stored in a file called - located in the home directory
### 解答
- 先`ls`發現有一個檔案`-`
- 嘗試用`cat`打開，但沒辦法
- 下`pwd`看絕對路徑`/home/bandit1` 
![image](https://hackmd.io/_uploads/rkUCtPwS6.png)

## Level2
### Level Goal
The password for the next level is stored in a file called spaces in this filename located in the home directory
### 解答
- `ls`看到檔案`spaces in this filename`直接`cat`取flag

## Level3
### Level Goal
The password for the next level is stored in a hidden file in the inhere directory.
### 解答
- 先進入`inhere`目錄
- `ls`顯示沒東西，加參數`ls -al`看到`.hidden`
- `cat`取flag
![image](https://hackmd.io/_uploads/By-3SuDra.png)

## Level4
### Level Goal
The password for the next level is stored in the only human-readable file in the inhere directory. Tip: if your terminal is messed up, try the “reset” command.
### 解答
- 先進入/inhere目錄
- `ls`顯示出有10個檔案
- `file ./*` 顯示副檔名
![](https://hackmd.io/_uploads/SkebTC22n.png)
- `cat ./-file07`打開檔案

## Level5
### Level Goal
The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:
### 解答
human-readable
1033 bytes in size
not executable

- 先進入/inhere目錄
- 按照題目題示查找檔案
![](https://hackmd.io/_uploads/rJtIZka33.png)

## Level6
### Level Goal
### 解答
The password for the next level is stored somewhere on the server and has all of the following properties:

owned by user bandit7
owned by group bandit6
33 bytes in size 
- 按照題目設條件
![](https://hackmd.io/_uploads/H1yPQka3n.png)

## Level7
### Level Goal
The password for the next level is stored in the file data.txt next to the word millionth
### 解答
- 按照題目條件在 data.txt 查找密碼
![](https://hackmd.io/_uploads/SJlXO81622.png)

## Level8
### Level Goal
The password for the next level is stored in the file data.txt and is the only line of text that occurs only once
### 解答
- 先用`sort`排列後再用`uniq -u`輸出沒有重複的字串
![](https://hackmd.io/_uploads/BkjWcypnh.png)

## Level9
### Level Goal
The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.
### 解答
- 用`strings`過濾可閱讀的文字,再用`grep`找出開頭是=的字串
![](https://hackmd.io/_uploads/rJlhhJpnh.png)

## Level10
### Level Goal
The password for the next level is stored in the file data.txt, which contains base64 encoded data
### 解答
- 用`base64 -d `解碼
- ![](https://hackmd.io/_uploads/rkJQ0J622.png)

## Level11
### Level Goal
The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions
### 解答
- 用` tr 'A-Za-z' 'N-ZA-Mn-za-m'`轉換字元 解密RO13
- ![](https://hackmd.io/_uploads/rycO1epnn.png)

## Level12
###  Level Goal
The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!)
### 解答
- 先將檔案移到myname123`cp /home/bandit12/data.txt /tmp/myname123`
- 用`xxd -r`轉換進位
- 再用`file`看壓縮類型
- 分別用`gzip -d` `bzip2 -d` `tar xvf`解壓縮

## Level13
### Level Goal
The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on
### 解答
- 先將 sshkey.private 中的key複製
- 到本機端`cat > 檔名`把key存入後提權`chmod 600 檔名`
- 再用`ssh  -i 檔名 bandit14@bandit.labs.overthewire.org -p 2220`用檔案中的key連線

## Level14
### Level Goal
The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.
### 解答
- 先從上一題給的路徑`/etc/bandit_pass/bandit14 `拿到進入localhost的密碼
- 拿到之後用`telnet`指令連線到127.0.0.1就可以拿到密碼
- ![](https://hackmd.io/_uploads/B1HnTTl6h.png)

## Level15
### Level Goal
The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL encryption.

Helpful note: Getting “HEARTBEATING” and “Read R BLOCK”? Use -ign_eof and read the “CONNECTED COMMANDS” section in the manpage. Next to ‘R’ and ‘Q’, the ‘B’ command also works in this version of that command…
### 解答
- 用`openssl s_client -connect ip:port`連線後輸入lv14的密碼

## Level16
### Level Goal
The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.
### 解答
- 用`nmap -sV 127.0.0.1 -p 31000-32000`搜尋127.0.0.1 port 31000~32000的通訊埠
- ![](https://hackmd.io/_uploads/HJbUeJZTh.png)
- 連線那兩個ssl的通訊埠
- 連線到正確的通訊埠後輸入上一題的密碼會得到一串金鑰
- 再用lv13的方式登入下一題

## Level17
### Level Goal
There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new

NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19
### 解答
- 用`diff -y 檔案1 檔案2`比較兩個檔案不一樣的字串
- 有 | 標記的是不一樣的
![](https://hackmd.io/_uploads/BJ4irJ-pn.png)
- 按題目取password.new 中的密碼

## Level18
### Level Goal
The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.
### 解答
- 先試著登入後發現會被踢出
- 照題目用ssh帶一個指令,打開readme`ssh  bandit18@bandit.labs.overthewire.org -p 2220 cat readme`
## Level19
### Level Goal
To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.
### 解答
- 先用`ls -l bandit20-do`看檔案權限跟可執行的使用者
- ![](https://hackmd.io/_uploads/H1vrkuXah.png)
- 打開檔案後按照提示用bandit20執行
- 在按照題目提示從`/etc/bandit_pass`拿bandit20的密碼
![](https://hackmd.io/_uploads/SJwsaD7T3.png)

## Level20
### Level Goal
There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

NOTE: Try connecting to your own network daemon to see if it works as you think
### 解答
- 用`echo`輸出lv20的密碼後面用`netcat -lp port`開起接聽模式
- 再用`jobs`查看正在執行的程式
- 在打開suconnect就可以得到密碼
![](https://hackmd.io/_uploads/rkgoLKX6h.png)

## Level21
### Level Goal
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.
### 解答
![](https://hackmd.io/_uploads/BkpDsY7p3.png)
- 按照題目給的路徑並打開`bandit22_root`
- 在按照檔案給的路徑走到`cronjob_bandit22.sh`
- 在按照裡面的路徑就可以得到密碼

## Level22
### Level Goal
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.
### 解答
- 按照題目給的路徑並打開`bandit23`
- 在按照檔案給的路徑走到`cronjob_bandit23.sh`
- 在按照檔案裡的程式碼執行`wcho I am bandit23 | md5sum | cut -d ' ' -f 1`
- 得到`$mytarget`後`cat /tmp/mytarget`就可以得到密碼
![](https://hackmd.io/_uploads/rJJ9uM563.png)

## Level23
### Level Goal
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

NOTE 2: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…
### 解答
- 按照題目給的路徑並打開`bandit24`
- 在按照檔案給的路徑走到`cronjob_bandit24.sh`
- 打開腳本後看到第三行說會執行並刪除所有在`/var/spool/$myname/foo`內的sh檔
- ![](https://hackmd.io/_uploads/SkvbOnh6n.png)
- 到`/tmp` 目錄下再創一個目錄
- 在目錄中開一個sh檔
- 腳本內的指令會將`/etc/bandit_pass/bandit24`複製到剛創的目錄bandit24中
- ![](https://hackmd.io/_uploads/SyD6YnnT2.png)
- 腳本寫完後用`touch`更改系統時間
- 寫完腳本後分別將`bandit_24.sh`和`bandit_pass.txt`提權
- ![](https://hackmd.io/_uploads/SJE0q22p3.png)
- 提權之後`ls -al`確認時間
- 再用`cp bandit_24.sh` 把腳本複製到`/var/spool/bandit24/foo`
- 複製之後再`ls -al`確認時間
- 然後打開`bandit_pass.txt`就可以拿到密碼了

## Level24
### Level Goal
A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.
You do not need to create new connections each time 
### 解答
![](https://hackmd.io/_uploads/ByXVdcu0n.png)
- 要用24題密碼搭配一個四位數子才能拿到25題密碼而且有時間限制
```shell=
for i in {0000..9999}; do echo VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar $i; done | nc localhost 30002 | grep -v Wrong!
```
- 用for迴圈從0000~9999搭配密碼輸出並不輸出錯誤答案
![](https://hackmd.io/_uploads/HyJJd9_Ch.png)

## Level25
### Level Goal
Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.
### 解答
- 先`ls`看到`bandit26.sshkey`
- 用`bandit26.sshkey`登入會被關掉
- 再用`cat /etc/passwd | grep bandit26`看使用者資訊
- ![](https://hackmd.io/_uploads/BJyvjFhCn.png)
- 發現他是用showtxt連線，要把它改成bash
- 用`more`的顯示特性把視窗縮小不讓它顯示全部
- 按v切到vim模式下指令`:set shell=/bin/bash`
- 再下`:shell`

## Level26
### Level Goal
Good job getting a shell! Now hurry and grab the password for bandit27!
### 解答
- 先`ls`看到`bandit27-do`
- 用`file bandit27-do`看到`bandit27-do`是執行檔
- 再用`./bandit27-do`執行`bandit27-do`
- ![](https://hackmd.io/_uploads/HJhleqnR3.png)
- 發現他會更改使用者
- 用`./bandit27-do cat /etc/bandit_pass/bandit27`直接看bandit27密碼

## Level27
### Level Goal
There is a git repository at ssh://bandit27-git@localhost/home/bandit27-git/repo via the port 2220. The password for the user bandit27-git is the same as for the user bandit27.

Clone the repository and find the password for the next level.
### 解答
- 先下`git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo`
- 發現權限不夠，`mktemp -d`創建暫時目錄
- 進到目錄後再下一次`git clone ssh://bandit27-git@localhost2220/home/bandit27-git/repo`
- 進到`repo`目錄後`cat README`就可以拿到密碼
![](https://hackmd.io/_uploads/S1QM2Qyya.png)

## Level28
### Level Goal
There is a git repository at ssh://bandit28-git@localhost/home/bandit28-git/repo via the port 2220. The password for the user bandit28-git is the same as for the user bandit28.

Clone the repository and find the password for the next level.
### 解答
- 先下`git clone ssh://bandit28-git@localhost:2220/home/bandit28-git/repo`
- 發現權限不夠，`mktemp -d`創建暫時目錄
- 進到目錄後再下一次`git clone ssh://bandit28-git@localhost2220/home/bandit28-git/repo`
- 進到`repo`目錄後`cat README.md`發現密碼被修改過
```
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: xxxxxxxxxx

```
- `git log`查看檔案修改紀錄
- ![](https://hackmd.io/_uploads/BJKZ74yJ6.png)
- `git show 899ba88df296331cc01f30d022c006775d467f28`看最近一次修復訊息洩漏(fix info leak)
![](https://hackmd.io/_uploads/rJgVV4Jyp.png)

## Level29
### Level Goal
There is a git repository at ssh://bandit28-git@localhost/home/bandit28-git/repo via the port 2220. The password for the user bandit28-git is the same as for the user bandit28.

Clone the repository and find the password for the next level.
### 解答
- 先下`git clone ssh://bandit29-git@localhost:2220/home/bandit29-git/repo`
- 發現權限不夠，`mktemp -d`創建暫時目錄
- 進到目錄後再下一次`git clone ssh://bandit29-git@localhost2220/home/bandit29-git/repo`
- 進到`repo`目錄後`cat README.md`發現密碼沒在這
```
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: <no passwords in production!>

```
- 用`git branch -a`查看其他分支
- ![](https://hackmd.io/_uploads/r1XCdEk16.png)
- `git checkout dev`查看`dev`分支
- 打開`RREADME.md`發現密碼再裡面
![](https://hackmd.io/_uploads/SJCVYVkyp.png)

## Level30
### Level Goal
There is a git repository at ssh://bandit28-git@localhost/home/bandit28-git/repo via the port 2220. The password for the user bandit28-git is the same as for the user bandit28.

Clone the repository and find the password for the next level.
### 解答
- 先下`git clone ssh://bandit30-git@localhost:2220/home/bandit30-git/repo`
- 發現權限不夠，`mktemp -d`創建暫時目錄
- 進到目錄後再下一次`git clone ssh://bandit30-git@localhost2220/home/bandit30-git/repo`
- 進到`repo`目錄後`cat README.md`發現密碼沒在這
- 用`git tag`查看標籤
- `git show secret`查看`secret`標籤，發現密碼再裡面
- ![](https://hackmd.io/_uploads/SJFyaVJJp.png)
## Level31
### Level Goal
There is a git repository at ssh://bandit28-git@localhost/home/bandit28-git/repo via the port 2220. The password for the user bandit28-git is the same as for the user bandit28.

Clone the repository and find the password for the next level.  
### 解答
- 先下`git clone ssh://bandit31-git@localhost:2220/home/bandit31-git/repo`
- 發現權限不夠，`mktemp -d`創建暫時目錄
- 進到目錄後再下一次`git clone ssh://bandit31-git@localhost2220/home/bandit31-git/repo`
- 進到`repo`目錄後`cat README.md`發現密碼沒在這
```
This time your task is to push a file to the remote repository.

Details:
    File name: key.txt
    Content: 'May I come in?'
    Branch: master
```
- 打開`gitignore`，發現他不會記錄txt檔
- 再依照題目條件建檔，先把檔案從工作區add到暫存區，這裡`-f`是要強制加入，再把檔案push到遠端
- `push`後密碼就出來了
![](https://hackmd.io/_uploads/ry847r11a.png)

## Level32
### Level Goal
After all this git stuff its time for another escape. Good luck!
### 解答
- 先下`ls`發現指令變大寫
- ![](https://hackmd.io/_uploads/HkyCb1iJT.png)
- `$0`引用shell
- 出來後下`ls /etc/bandit_pass/bandit33`取密碼
# 心得
OverTheWire - Bandit是我第一次解完一整系列的CTF題目，從0基礎開始研究linux指令，一開始還是會有指令記不住之類的問題，但打久後自然就記起來了，題目都是全英文的，下方都有附相關指令的用法。花了六周解完這系列題目有助於我對linux指令更進一步了解。





