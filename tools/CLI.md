
du -s * | sort -nr 可以把所有文件占用了多少内存块按降序打印

ls -l /usr/local | cut -c1 | grep d | wc -l 可以统计出/usr/local下面有多少个目录

dir=$HOME/proj 可以用来方便的使用常用目录

printenv HOME

双引号中$之类的特殊字符还是会被计算，单引号的就不会

`\cmd` 会直接运行cmd，而无视别名的覆盖

echo $PATH | tr : "\n" 可以把PATH的冒号分隔符替换为换行符

![[Pasted image 20230612084327.png]]

history | grep -w wd

!?grep? 可以调用历史命令中含有grep的内容

按下ctrl-r以后就可以增量搜索历史命令了

输入错了不要紧，两个脱字符就可以改过来了

![[Pasted image 20230612091325.png]]
![[Pasted image 20230612091724.png]]

可以把最常用的目录加入到CDPATH里面，这样的话无论身处哪个位置，都能够直接用到该目录及其子目录

`ls -d */`可以把当前目录下的所有目录打印出来


目录也是可以进栈的

![[Pasted image 20230612094025.png]]


![[Pasted image 20230612094338.png]]

表示将栈顶的四个目录换到底部，并进去新的顶部目录

![[Pasted image 20230612094841.png]]

好用还是pushd -n 最好用

![[Pasted image 20230612095843.png]]![[Pasted image 20230612095844.png]]
列出当前目录下有多少个目录

date可以指定格式

![[Pasted image 20230612101753.png]]
