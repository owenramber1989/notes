# 字符串相关操作
***
#在文件中查找字符串
`grep printf path/*.c`
	如果只想看到出现的次数，可以加 -c 参数
	如果不想看次数，只想看到文件名，可以加 -l
`rm -i $(grep -l 'This file is obsolete' *)`
注意\*不会深入子目录, 只会匹配当前目录
#只关心搜索是否成功
`if grep -q findme foo.bar ; then echo yes ; else echo nope ; fi`
-q 指代 quiet , 如果在其后列出多个文件名，那么找到第一处匹配后grep就下班了
如果成功的话，第一处匹配的文件名储存在 `$?`这个shell 变量里面
	如果想要移植性更好或者搜索所有文件，可以
	`if grep findme foo.bar > /dev/null ; then echo yes ; else echo nope ; fi`
	/dev/null 是 位桶 ， 重定向到此处的输出将被丢弃
-i 忽略大小写
#在管道中进行搜索
	记得要将命令的输出重定向到标准输出
	`gcc foo.c 2>&1 | grep -i error`
***
grep 也可以嵌套使用
```shell
grep -i taylor mail/*
!! | grep -i swift
!! | grep -i "the female artist"
```
注意历史运算符 !! 的使用，它可以重复上一条命令
#缩写搜索结果 
`grep -i dec logfile | grep -vi decimal | grep -vi | decimate`
但是搜索12月的日志文件最好用下面的正则表达式
`grep Dec\ [0-9 ][0-9] logfile`
***
# 基本命令
#查找命令
	忘记了命令名的时候可以用 `apropos` 或 `man -k`
#查找文件
`ls -d .v*/` 是一个例子，-d表示筛选，文件名模式以斜线结尾可以只显示匹配模式的目录，而不会显示文件名
#输出
	'' 单引号可以让shell不处理字符串，双引号则还是会进行变量扩展、算术扩展、波浪号扩展和命令替换
#格式控制
```shell
printf '%-10.10s = %4.2f\n' 'Gigahertz' 1.92735
```
对于字符串s，第一个数字是字段的最大宽度，第二个数字是要输出的字符数量
#重定向
`>&`              `&>`              `2>&1` 都是把标准错误重定向到与标准输出一样的地方
#追加
使用 `>>`
#花括号的用法
1. 可以用来将重定向应用于多个命令的输出
	` { pwd; ls; cd ../somewhere; pwd; ls; } > /tmp/all.out`
	当然用圆括号也可以，用花括号则要注意花括号两端要有空格，圆括号是在子shell中运行命令，花括号更像一种便捷写法
		这种用法下最后一个命令必须用分号结尾
	上例如果用圆括号，wd不会改变，但若用花括号则会改变
2. 简化if-else分支结构
```shell
[ $result = 1 ] \
  && { echo "Result is 1." ; exit 0 ; } \
  || { echo "Result isn't 1." ; exit 120; } 
```
#T形接头
将输出作为输入，同时保留其副本
```shell
find / -name '*.c' -print 2>&1 | tee /tmp/all.my.sources
```
这样，输出保留在了指定的文件里，同时也会打印到屏幕上
#STDERR与STDOUT
	标准输出是缓冲式的，标准错误则不是。对于标准输出，只有缓冲区满了或者文件被关闭，才会写入输出，标准错误则是每个字符都单独写入，因次可以立刻看到错误信息
#标准输入
<< 允许我们创建一个临时输入源，然后指定一个终止符，用`<<-`  就可以在每行的开头用tab缩进here-document
```shell
grep $1 <<-'EOF'
	lots of data
	EOF
```
注意EOF后面不能有空白字符
#获取用户输入
1. read
```shell
read 
read -p "what's your name" ANSWER
read -t 3 -p "Tell me your name in 3 sec." ANSWER
read PRE MID POST
```
-p 可以先输出提示，-t设置时间限制
获取多个变量，输入少了后面的设为null，多了的则全部留给最后一个变量
2. 避免屏幕回显
	`read -s -p "password: " PASSWD ; printf "%b" "\n"`
***
#  执行命令
`$PATH` 包含了一个目录列表，bash 使用该变量定位可执行文件， 目录以冒号分隔
注意当前目录，也就是点号目录一定要放在最后面，最好不放
***因为bash是按照目录中列出的目录顺序依次查找命令行上指定的可执行文件，点号目录放在前面就容易被同名命令的恶意版本欺骗***
```shell
bash
cd
touch ls
chmod 755 ls
PATH=".:$PATH"
ls
```
这样的话ls就失效了，但因为第一行命令bash是一个副本，exit该shell就好了，不过删掉该ls亦可
```shell
cd
rm ls
exit
```
因此，***绝对不要把点号目录或可写目录放进root的$PATH变量里***
## 多个命令
```shell
a ; b ; c # 依次执行abc三个命令
a && b && c # 前一个命令成功了才执行下一个命令
# cd tmp ; && rm *
a & b & c # 同时执行三个命令，a b 转后台
```


第三条命令会打印出作业号和进程ID，杀死作业可以 `kill %1` or `kill 4592`
#执行结果
`$?` 变量保存命令的退出状态
	如果直接打印该变量，那么只有一次机会，最好用一个变量将退出状态保存
	`STAT=$? ; echo $STAT `
	注意注意***shell使用0代表真，非0代表假***
#命令失败时退出shell
```shell
set -e
cd mytmp
rm *
```
如果没有成功进入mytmp的话shell就直接退出了
#显示报错
`cmd || printf "%b" "cmd failed \n"`
这样cmd执行失败时就会出现后面的提示信息
	注意该用法和a or b一样是会短路的，所以如果 || 后面有多条指令记得用花括号
#执行所有的脚本
```shell
for SCRIPT in /path/*
do
	if [ -f "$SCRIPT" -a -x "SCRIPT" ] 
	then 
		$SCRIPT
	fi
done
```
-f 判断是不是文件，-x判断是否有执行权限
***
# 变量
	所有变量最好都用大写，赋值符号两侧不能有空白字符

#导出变量
```shell
export FNAME
export SIZE
export MAX
...
MAX=2048
SIZE=64
FNAME=/tmp/foo/bar
```
导出的变量是`pass by value` 的,因此传出去之后就可以随意修改了
#查看
`set`  查看当前shell中所有的变量值和函数定义
`env`   查看那些被导出的，可以用于子shell的变量
#处理包含空格的参数列表
```sh
for ARGS in "$@"
do
	chmod 0750 "$ARGS"
done
```
#参数数量
`if [ $# -lt 3 ] `
or
`if (( $# > 3 )) `
#默认值
`FILEDIR=${1:-/tmp}`
`:-`提供了一个缺省选项
而`:=` 会在指定参数为空时为其赋值
`cd ${BASE:="$(pwd)"}`
`$?`可以针对不存在的参数输出错误信息
```sh
FOOTYPE=${3:?"Error. $USAGE. $(rm $BAR)"}
```
甚至可以在后面额外下命令并打印命令的输出
#修改字符串
```sh
#!/usr/bin/env bash
# 将文件后缀.bad 改为 .bash

for FN in *.bad
do
	mv "${FN}" "${FN%bad}bash"
done 
```
当然上面的实例也可以用sed的斜线法
|${...}内|操作|
|---|---|
|name:num1:num2|从索引num1开始，返回长度为num2的子串|
|`#name`|返回字符串长度|
|name#pattern|从字符串起始位置开始，删除匹配pattern的最短子串|
|name##pattern|从字符串起始位置开始，删除匹配pattern的最长子串|
|name%pattern|从字符串结束位置开始，删除匹配pattern的最短子串|
|`name%%pattern`|从字符串结束位置开始，删除匹配pattern的最长子串|
|name/pattern/string|替换掉第一次出现的pattern|
|name//pattern/string|替换掉所有出现的pattern|
#求绝对值
`${MYNUM#-}`
#循环读值创建CSV
```sh
while read NEWVAL
do
	LIST="${LIST}${LIST:+,}${NEWVAL}"
done
echo $LIST
```
如果LIST为空或者不存在，那么两个关于LIST的表达式都为空
如果LIST不为空，那么第二个表达式会被替换成逗号
#数组
ARR=(first second third home)
$ARR 等同于 ${ARR[0]}
#大小写
```sh
${FN,,} # 全部转小写
${FN^^} # 大
${FN~~} # 颠倒
```
也可以用变量声明的方式
```sh
declare -u up # 大
declare -l dn # 小
declare -c Ca # 首字母大写
```
#驼峰命名法
```sh
while read TXT
do
	RA=($TXT) # 数组初始化
	echo ${RA[@]^}
done
```
`[@]` 一次性引用数组中的全部元素，^ 脱字符将每个元素的首字母转换为大写
***
# 算数
```sh
COUNT=$((COUNT + $2 + OFFSET))
let COUNT+=5
```
也可以用逗号形成级联
`echo $(( X+=5, Y*=3))`
```sh
if test $# -lt 3
then 
	echo try again.
fi
```
***
检查文件特性的单目运算符
|运算符|描述|
|---|---|
|-e file|文件是否存在(exist)|
|-f file|文件是否存在且为普通文件(file)  |
|-d file  |文件是否存在且为目录(directory)|
|-b file  |文件是否存在且为块设备block device  |
|-c file  |文件是否存在且为字符设备character device|
| -S file  |文件是否存在且为套接字文件Socket  |
|-p file  |文件是否存在且为命名管道文件FIFO(pipe)  |
|-L file|  文件是否存在且是一个链接文件(Link)|
|-N file|文件自上次读取后被修改过|
|-r file|可读文件(-w -x 类似)|
|-s file|文件大小不为空|
***
```sh
while read lineoftext
do
	process that line
done < file.input

# 另一种从文件中读取参数进行循环的方式

cat file.input |
while read lineoftext
do
	process that line
done
```


