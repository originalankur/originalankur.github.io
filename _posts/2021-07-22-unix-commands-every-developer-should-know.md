---
title: "Unix commands every developer should know"
categories:
  - programming
tags:
  - unix
  - command-line
last_modified_at: 2020-07-22T14:25:52-05:00
---

If you are new to Linux or any Unix operating system here is a list of must know commands that can help you in every day task

## cat

Shows the content of a file. Run cat /etc/hosts

```
127.0.0.1	localhost
255.255.255.255	broadcasthost
::1             localhost
```

is the output on my computer. /etc/hosts is the path of the file.

## mkdir

Make/Create a directory. Run mkdir a mkdir a/b  mkdir a/b/c

this will create the directory a  and in it will be directory b and then c in b directory.

## pwd

print working directory. This commands shows the current directory you are in, output from my machine.

```
expanse@Ankurs-Mac-mini ~ % pwd 
/Users/expanse
```

I am in expanse directory which is in the Users directory.


## cd

cd allows you to navigate from one directory to another. You can give an absolute path or
relative path. Relative path is the location of the directory from your current folder.
Let us try to navigate the directory created above aka a b and c

Enters directory a and this is relative path from current directory.
```
cd a 
``` 

Enters directory b from current directory
```
cd b
``` 

Enter directory c using absolute path aka the complete path on the filesystem.
```
cd /Users/expanse/a/b/c
``` 

you can use 
```
cd ..
``` 
to go to parent directory from current directory.


```
cd -
``` 
to go back to previous directory before you entered this one.

```
cd
``` 
without any argument goes to the home directory of the user.


## free

free tells you how much of free memory is there on the system. e.g.

```
deploy@linux:~$ free
              total        used        free      shared  buff/cache   available
Mem:        8167616     3140156      795416        1156     4232044     4723404
Swap:             0           0           0
```

this show you how much of free Mem and swap space is there. To know the same in MB and GB do this 

```
free -mh
```

```
deploy@linux:~$ free -mh
total        used        free      shared  buff/cache   available
Mem:           7.8G        3.0G        776M        1.1M        4.0G        4.5G
Swap:            0B          0B          0B
```

## df

df tell you how much disk space is free in every partition.  e.g. 

```
df -h
```

```
deploy@deploy:~$ df -h 
Filesystem      Size  Used Avail Use% Mounted on
udev            3.9G     0  3.9G   0% /dev
tmpfs           798M  960K  797M   1% /run
/dev/vda1        78G   61G   17G  79% /
tmpfs           3.9G     0  3.9G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           3.9G     0  3.9G   0% /sys/fs/cgroup
/dev/loop0      100M  100M     0 100% /snap/core/11316
/dev/loop2      100M  100M     0 100% /snap/core/11187
/dev/loop1       43M   43M     0 100% /snap/certbot/1201
/dev/loop4       62M   62M     0 100% /snap/core20/1026
/dev/vda15      105M  8.8M   96M   9% /boot/efi
tmpfs           798M     0  798M   0% /run/user/1000
/dev/loop5       43M   43M     0 100% /snap/certbot/1280
```

## head

head allows you to display the first few lines of a file. I have a file with all rss feeds of tennis websites. 

```
deploy@linux:~$ head tennisnews.txt 
http://news.bbc.co.uk/rss/sportonline_world_edition/tennis/rss091.xml
http://sports.espn.go.com/espn/rss/tennis/news?null
http://www.tennis-x.com/tennisxnews.xml
http://www.atpworldtour.com/en/media/rss-feed/xml-feed
http://straightsets.blogs.nytimes.com/feed/
http://feeds.tennis.com/concrete-elbow-tignor
http://www.guardian.co.uk/sport/tennis/rss
http://feeds.feedburner.com/tennisx
http://bleacherreport.com/articles/feed?tag_id=12
http://rss.cnn.com/rss/edition_tennis.rss
```

how about if I want to display only the top 10 lines?. 

```
deploy@linux:~$ head -n 5 tennisnews.txt 
http://news.bbc.co.uk/rss/sportonline_world_edition/tennis/rss091.xml
http://sports.espn.go.com/espn/rss/tennis/news?null
http://www.tennis-x.com/tennisxnews.xml
http://www.atpworldtour.com/en/media/rss-feed/xml-feed
http://straightsets.blogs.nytimes.com/feed/
```

## watch

watch allows you to run a command again and again periodically. Say that your ram's is getting utilised completely and you want to monitor the change or growth every 3-4 seconds.

```
deploy@deploy:~$ watch -n 3 free -m
Every 3.0s: free -m                                                                                     total        used        free      shared  buff/cache   available
Mem:           7976        3068         722           1        4185        4610
Swap:             0           0           0
```

## screen

say you want to run a command and let it run even after you have existed the terminal aka run the program in background without the terminal running. Use the command screen

Run 
```
screen
``` 
and you will be greeted by a copyright message, 
```[Press Space or Return to end.]```

and you will see a normal shell prompt. Here you can simply run a script and exit screen by entering the command 

```
ctrl + ad
```

and to enter the screen again simply type  ```screen -r -d```


## | and grep

say you have a file with thousands of lines and want to find amongst that a word. 
We have seen that we can print the content of a file with cat. grep allows you to find the existence of a word in a line. `|` will allow you to take the output of one and make it the input of another. What the hell am I talking?.  e.g.

```
deploy@deploy:~$ cat tennisnews.txt | grep swordsmaster
http://blog.xuite.net/swordsmaster/tennis/rss.xml
```

so `|` made the output of tennisnews.txt as the input of grep.