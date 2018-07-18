
# 一、基础命令

## 1、man

Linux手册

`man cp`

## 2、cp

复制

```
[yaha@Yaha ~]$ cp -v php-7.2.0.tar.gz php.tar.gz
"php-7.2.0.tar.gz" -> "php.tar.gz"

[yaha@Yaha ~]$ cp -v test.c{,1}
"test.c" -> "test.c1"
```

## 3、mv

移动

```
[yaha@Yaha ~]$ mv -v test.c test.c1
"test.c" -> "test.c1"

[yaha@Yaha ~]$ mv -v test.c1{,test}
"test.c1" -> "test.c1test"
```

## 4、pwd

显示当前路径

```
[yaha@Yaha ~]$ pwd
/home/yaha

```

## 5、rm

删除

```
[yaha@Yaha ~]$ rm -v php-7.2.0.tar.gz
已删除"php-7.2.0.tar.gz"
```

## 6、touch

创建文件

```
[yaha@Yaha ~]$ touch test.c
```

## 7、echo

打印当前输入

```
[yaha@Yaha ~]$ echo test
test
```

## 8、设置临时环境变量

```
# 创建变量
[yaha@Yaha ~]$ test='test'

# 打印当前变量
[yaha@Yaha ~]$ echo $test
test

# 打印所有环境变量的列表
[yaha@Yaha ~]$ set|grep test
test=test

# 打印所有环境变量列表,但不包含test
[yaha@Yaha ~]$ env|grep test

# 使变量test可用于从当前 shell 执行的所有程序
[yaha@Yaha ~]$ export test

[yaha@Yaha ~]$ env | grep test
test=test

# 重新赋值
[yaha@Yaha ~]$ test='ls -la'

# 使用变量
[yaha@Yaha ~]$ $test
总用量 845632
drwx------   8 yaha yaha      4096 7月   6 13:45 .
drwxr-xr-x.  3 root root      4096 1月   3 2018 ..
-rw-------   1 yaha yaha     15758 6月   1 23:56 .bash_history
-rw-r--r--   1 yaha yaha        18 9月   7 2017 .bash_logout
-rw-r--r--   1 yaha yaha       193 9月   7 2017 .bash_profile
-rw-r--r--   1 yaha yaha       231 9月   7 2017 .bashrc
-rw-------   1 yaha yaha       870 4月  25 16:57 .mysql_history
drwxrwxr-x  17 yaha yaha      4096 1月   4 2018 php-7.2.0
drwxrw----   3 yaha yaha      4096 1月   3 2018 .pki
drwxrwxr-x   6 yaha yaha      4096 12月  5 2017 redis-4.0.6
-rw-rw-r--   1 yaha yaha   1723533 12月  5 2017 redis-4.0.6.tar.gz
-rw-------   1 yaha yaha        29 3月  25 00:12 .rediscli_history
drwx------   2 yaha yaha      4096 5月  22 22:59 .ssh
drwxrwxr-x   2 yaha yaha      4096 1月  28 22:48 test
-rw-rw-r--   1 yaha yaha         5 7月   6 13:45 test.c
-rw-rw-r--   1 yaha yaha         5 7月   6 13:45 test.c1test
-rwxrwxr-x   1 yaha yaha 864110719 4月  10 12:36 test.sql
drwxr-xr-x  12 yaha yaha      4096 5月  22 22:22 tools_admin
-rw-------   1 yaha yaha      5201 5月  25 23:38 .viminfo
```

## 9、diff

比对两个文件的差异

```
[yaha@Yaha ~]$ echo 'test'>>test.{c,c1}
[yaha@Yaha ~]$ echo '123'>>test.c1
[yaha@Yaha ~]$ diff test.{c,c1}
1a2
> 123
```

## 10、less

快速查看文件，类似vi、vim

- j - 向上移动
- k - 向下移动
- q - 退出less。
- `--chop-long-lines`或`--ch`
-  - 开启水平滚动。
-  / - 搜索。
-  &something - 只显示文件中包含某些内容的行。

## 11、history

查看历史运行命令（历史记录存在~/.bash_history中）

## 12、wc

统计行数

[yaha@Yaha ~]$ wc test.c
1 1 5 test.c

## 13、sed

替换字符串

```
[yaha@Yaha ~]$ echo SELECT+lesson_id+as+lessonId%2C+lesson_name+as+lessonName%2C+course_id+as+courseId%2C+abstract+as+abstract%2C+start_time+as+startTime%2C+stop_time+as+stopTime%2C+status+as+status%2C+create_time+as+createTime%2C+update_time+as+updateTime%2C+operator_uid+as+operatorUid%2C+operator+as+operator%2C+ext_data+as+extData%2C+class_essential+as+classEssential%2C+check_status+as+checkStatus%2C+ext_video+as+extVideo%2C+tag_bit_map+as+tagBitMap%2C+service_bit+as+serviceBit%2C+recheck_uid+as+recheckUid%2C+quality_status+as+qualityStatus+FROM+tblLesson+WHERE+%28course_id+in+%2866269%2C66271%2C66273%2C66275%2C66277%2C66279%2C66281%29%29+limit+0%2C+1000 | sed 's/%2C/,/g' | sed 's/+/ /g' |sed 's/%28/(/g'|sed 's/%29/)/g'
SELECT lesson_id as lessonId, lesson_name as lessonName, course_id as courseId, abstract as abstract, start_time as startTime, stop_time as stopTime, status as status, create_time as createTime, update_time as updateTime, operator_uid as operatorUid, operator as operator, ext_data as extData, class_essential as classEssential, check_status as checkStatus, ext_video as extVideo, tag_bit_map as tagBitMap, service_bit as serviceBit, recheck_uid as recheckUid, quality_status as qualityStatus FROM tblLesson WHERE (course_id in (66269,66271,66273,66275,66277,66279,66281)) limit 0, 1000
```

# 二、使用技巧

## echo

可以利用echo命令来进行文件追加，如跳板机无法直接使用vim、cat等命令，则无法配置快捷命令登陆机器

`echo "alias ls_4='ssh rd@192.168.240.4'" >> ~/.bash_profile`



