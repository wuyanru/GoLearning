
# 应用场景

```json lines
自己使用过的

 cat dp.invoker.metadata_dev.log |grep -c 'RegionDataSourceCheckReq'
 grep -l 'GetRegionDsCheck' ./*  在文件夹下搜索包含关键字文件



```




## 应用场景一：按行号查看---过滤出关键字附近的日志

     1）cat -n test.log |grep "debug"  得到关键日志的行号

     2）cat -n test.log |tail -n +92|head -n 20  选择关键字所在的中间一行. 然后查看这个关键字前10行和后10行的日志:

            tail -n +92表示查询92行之后的日志

            head -n 20 则表示在前面的查询结果里再查前20条记录

##  应用场景二：根据日期查询日志

      sed -n '/2014-12-17 16:17:20/,/2014-12-17 16:17:36/p'  test.log

      特别说明:上面的两个日期必须是日志中打印出来的日志,否则无效；

                      先 grep '2014-12-17 16:17:20' test.log 来确定日志中是否有该 时间点

## 应用场景三：日志内容特别多，打印在屏幕上不方便查看

    (1)使用more和less命令,

           如： cat -n test.log |grep "debug" |more     这样就分页打印了,通过点击空格键翻页

    (2)使用 >xxx.txt 将其保存到文件中,到时可以拉下这个文件分析

            如：cat -n test.log |grep "debug"  >debug.txt

## 应用场景四：统计关键字出现次数

grep -o "关键字"  文件名称|wc -l



# 常用命令

tail

参数： tail [ -f ] [ -c Number | -n Number | -m Number | -b Number | -k Number ] [ File ]

参数说明：

-f 该参数用于监视File文件增长。
-c Number 从 Number 字节位置读取指定文件
-n Number 从 Number 行位置读取指定文件。
-m Number 从 Number 多字节字符位置读取指定文件，比方你的文件假设包括中文字，假设指定-c参数，可能导致截断，但使用-m则会避免该问题。
-b Number 从 Number 表示的512字节块位置读取指定文件。
-k Number 从 Number 表示的1KB块位置读取指定文件。
File 指定操作的目标文件名称

上述命令中，都涉及到number，假设不指定，默认显示10行。Number前面可使用正负号，表示该偏移从顶部还是从尾部開始计算。

tail可运行文件一般在/usr/bin/以下。

head
head 仅仅显示前面几行

示例：
head -n 10 test.log 查询日志文件中的头10行日志;
head -n -10 test.log 查询日志文件除了最后10行的其他所有日志;

grep

grep（global search regular expression(RE) and print out the line，全面搜索正则表达式并把行打印出来）是一种强大的文本搜索工具，它能使用正则表达式搜索文本，并把匹配的行打印出来。

grep [options] 主要参数:

－c：只输出匹配行的计数。

－I：不区分大 小写(只适用于单字符)。

－h：查询多文件时不显示文件名。

－l：查询多文件时只输出包含匹配字符的文件名。

－n：显示匹配行及 行号。

－s：不显示不存在或无匹配文本的错误信息。

－v：显示不包含匹配文本的所有行。

pattern正则表达式主要参数

：忽略正则表达式中特殊字符的原有含义。

^：匹配正则表达式的开始行。

$: 匹配正则表达式的结束行。

<：从匹配正则表达 式的行开始。

>：到匹配正则表达式的行结束。

[ ]：单个字符，如[A]即A符合要求 。

[-]：范围，如[A-Z]，即A、B、C一直到Z都符合要求 。

。：所有的单个字符。

- ：有字符，长度可以为0。

sed
sed -n '5,10p' filename 说明：只查看文件的第5行到第10行。

cat
cat主要有三大功能：

1.一次显示整个文件。$ cat filename
2.从键盘创建一个文件。$ cat > filename 只能创建新文件,不能编辑已有文件.
3.将几个文件合并为一个文件： $cat file1 file2 > file

参数：

-n 或 --number 由 1 开始对所有输出的行数编号

-b 或 --number-nonblank 和 -n 相似，只不过对于空白行不编号

-s 或 --squeeze-blank 当遇到有连续两行以上的空白行，就代换为一行的空白行

-v 或 --show-nonprinting

tac 是将 cat 反写过来，所以他的功能就跟 cat 相反， cat 是由第一行到最后一行连续显示在萤幕上， 而 tac 则是由最后一行到第一行反向在萤幕上显示出来！

more

more test.log

常用按键：

空格键：查看下一屏；
回车键：往下滚动一行；
b 键：往前查看一屏；
q 键：退出。
more +100 test.log 从100行开始显示

more -10 test.log 限制每页显示的行数

-d 显示 more 命令的一些提示信息

日志最常使用的三种操作命令

第一种:查看实时变化的日志(比较吃内存)

最常用的:

tail -f filename (默认最后10行,相当于增加参数 -n 10)

Ctrl+c 是退出tail命令

其他情况:

tail -n 20 filename (显示filename最后20行)

tail -n +5 filename (从第5行开始显示文件)

第二种:搜索关键字附近的日志

最常用的:cat -n filename |grep "关键字"

其他情况:

cat filename | grep -C 5 '关键字' (显示日志里匹配字串那行以及前后5行)
cat filename | grep -B 5 '关键字' (显示匹配字串及前5行)
cat filename | grep -A 5 '关键字' (显示匹配字串及后5行)
第三种:进入编辑查找:vi(vim)

1、进入vim编辑模式:vim filename

2、输入“/关键字”,按enter键查找

3、查找下一个,按“n”即可

退出:按ESC键后,接着再输入 :号 时,vi会在屏幕的最下方等待我们输入命令

wq! 保存退出;

q! 不保存退出;

其他情况:

/关键字   注:正向查找,按n键把光标移动到下一个符合条件的地方
?关键字   注:反向查找,按shift+n 键,把光标移动到下一个符合条件的




