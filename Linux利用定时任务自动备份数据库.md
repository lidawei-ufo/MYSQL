# Linux利用定时任务自动备份数据库 各种折腾

```mysql
在实际生产环境中遇到需要备份数据库,防止服务器或数据库崩溃数据损坏时,无法挽救的情况,一种情况我们可以部署MySQL
主主互备来备份数据库,在机器资源不足以我们部署MySQL主主互备,又不能采取以人工手动定时备份数据库这种繁琐的操作
时,我们可以利用Linux的定时任务crontab来自动的备份数据库
```

- 1、我们先来看我们的需求,比如现在需要在每天晚上的1点30分自动备份MySQL数据库中的名字为mysql的库到一个指定的
目录,比如root目录下的mysql_backup文件夹,那么备份命令就是下面这样

```mysql
/usr/bin/mysqldump -uUsername -pPassword mysql > /root/mysql_backup/mysql_$(date +%Y%m%d_%H%M%S).sql
"Username"是我们数据库的用户

"Password"是这个用户的密码

"mysql"为我们需要备份的数据库中的某个数据库

因为我们每天晚上都会备份一下数据库,所以备份之后我们以当时的备份时间来命名备份文件
,即为"mysql_$(date +%Y%m%d_%H%M%S).sql",比如我2019年1月30号晚上1点30分整备份的数据库
即为"mysql_20190130_013000.sql".
```

- 2、我们的需求是在每天晚上都备份一下,那么每天都会生成一个文件,时间久了硬盘就会被塞满,而且很早之前的数据也
没有太大的保留意义,那么我们可以在每天备份时同时删除一段时间之前的备份数据,比如我们需要删除30天之前的备份

```mysql
find /root/mysql_backup/ -mtime +30 -type f | xargs rm -f
"/root/mysql_backup/"为我们备份文件的保存目录

"-mtime +30"是设置时间为30天前

"-type f"表明查找的类型是文件

这行命令完成的操作是:查找/root/mysql_backup/目录下30天之前的文件并且删除.
```

- 3、备份跟删除的命令我们都写好了,那么我们可以通过crontab来让系统每天自动的去执行这两个任务

**创建一个任务脚本mysql_autobackup.sh,写入我们刚才的两条命令并给于文件最高权限**

```mysql
#!/bin/bash
/usr/bin/mysqldump -uUsername -pPassword mysql > /root/mysql_backup/mysql_$(date +%Y%m%d_%H%M%S).sql
find /root/mysql_backup/ -mtime +30 -type f | xargs rm -f
crontab -e 写入计划任务并保存

30 01 * * * /root/mysql_autobackup.sh
表示每天晚上1点30分会执行root目录下的mysql_autobackup.sh脚本,就是我们上面编写的备份跟删除操作的脚本,
这样就可以完成系统每天自动备份数据库并且会自动的去查找超过30天的备份并删除
```


### crontab 的格式

**1、以我们刚才写的计划任务为例**

```mysql
30 01 * * * /root/mysql_autobackup.sh
格式简化之后是下面这样

* * * * * *
第一列的"*"为分钟 从1~59

第二列的"*"为小时 从0~23,0代表午夜12点

第三列的"*"为日 从1~31

第四列的"*"为月 从1~12

第五列的"*"为星期 从0~6,0代表星期天

第六列的"*"为要运行的命令

综合起来就是下面的格式

分 时 日 月 星期 要运行的命令
```

**2、举一些例子**


```mysql
30 21 * * * reboot

上面的例子表示每晚的21:30重启服务器.

45 4 1,10,22 * * reboot

上面的例子表示每月1、10、22号的4:45重启服务器

10 1 * * 6,0 reboot

上面的例子表示每周六、周日的1:10重启服务器

0,30 18-23 * * * reboot

上面的例子表示在每天18:00至23:00之间每隔30分钟重启服务器.

0 23-7/1 * * * reboot

晚上11点到早上7点之间,每隔一小时重启服务器
```

****

作者:UFO

原文:https://github.com/lidawei-ufo/MYSQL/blob/master/Linux%E5%88%A9%E7%94%A8%E5%AE%9A%E6%97%B6%E4%BB%BB%E5%8A%A1%E8%87%AA%E5%8A%A8%E5%A4%87%E4%BB%BD%E6%95%B0%E6%8D%AE%E5%BA%93.md

版权声明:本文为博主原创文章,转载请附上博文链接!


