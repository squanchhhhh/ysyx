shell脚本

1.常见参数
$0 - 脚本名
$1 到 $9 - 脚本的参数。 $1 是第一个参数，依此类推。
$@ - 所有参数
$# - 参数个数
$? - 前一个命令的返回值
$$ - 当前脚本的进程识别码
!! - 完整的上一条命令，包括参数。常见应用：当你因为权限不足执行命令失败时，可以使用 sudo !!再尝试一次。
$_ - 上一条命令的最后一个参数。如果你正在使用的是交互式 shell，你可以通过按下 Esc 之后键入 . 来获取这个值。

2.命令替换（command substitution）
$(CMD)

3.通配符
对于文件foo, foo1, foo2, foo10 和 bar, rm foo?这条命令会删除foo1 和 foo2 ，而rm foo* 则会删除除了bar之外的所有文件。
括号{} - 当你有一系列的指令，其中包含一段公共子串时，可以用花括号来自动展开这些命令。这在批量移动或转换文件时非常方便。
mv *{.py,.sh} folder


练习：
1.ls练习 
使用--color显示颜色

2.编写脚本实现目录切换
alias mark="export MARK=$(PWD)"
alias back="cd $MARK"

问题1：使用alias别名的时候会直接替换$MARK为空，所以alias back="cd $MARK"相当于alias back="cd "
解决方法：将cd $MARK 放在脚本中，这样可以保证获取到的是环境变量中的MARK

问题2: 如果使用alias back="sh back.sh"会导致shell创建子shell来执行back.sh
解决方法:将sh换成source让当前shell执行代码


3.
#!/bin/bash
temp=0

while true
do
	sh "$1" 2> log 1>/dev/null
	temp=$((temp + 1)) 
	if [ -s log ] ; then
		echo "error $temp"
		break   
	fi
done

将标准错误重定向到log文件，检测log文件，如果有输出说明出现了错误

要点1:不能使用temp = 1来赋值变量，需要使用 temp=1 shell脚本赋值不能加空格
要点2: 1>log 2>/dev/null分别重定向标准输出和标准错误
要点3: if 条件判断要加[]
要点4: -s file file存在且大小非0
要点5: $((A+B))用于计算


4.
 find . -name "*.html" | xargs tar -cf html_files.tar
找到全部的html文件并且打包为html_files.tar



vim
1.
在正常模式下输入d来输入命令
d$ 删除当前行到下一行
dw 删除当前单词

许多改变文本的命令都由一个操作符和一个动作构成。
使用删除操作符 d 的删除命令的格式如下：

d   motion

其中：
d      - 删除操作符。
motion - 操作符的操作对象(在下面列出)。

一个简短的动作列表：
w - 从当前光标当前位置直到下一个单词起始处，不包括它的第一个字符。
e - 从当前光标当前位置直到单词末尾，包括最后一个字符。
$ - 从当前光标当前位置直到当前行末。

因此输入 de 会从当前光标位置删除到单词末尾。


2.
u撤销命令
U恢复当前行到原始状态
Ctrl+R撤销撤销命令


3.
p将最近删除复制到最后一行
dd会将删除的行保存到寄存器中

4.
r替换当前字符
r+a -> 将当前字符替换为a

5.
ce 更改当前单词
c$更改当前行（和d命令类似）

6.
/用于搜索单词
n下一个 N上一个
:set ic 无视大小写

7.
%搜索当前()的配对()

8.
要替换两行之间出现的每个匹配串，请
输入   :#,#s/old/new/g   其中 #,# 代表的是替换操作的若干行中
                      首尾两行的行号。
输入   :%s/old/new/g     则是替换整个文件中的每个匹配串。
输入   :%s/old/new/gc    会找到整个文件中的每个匹配串，并且对每个匹配串
                      提示是否进行替换。

9.
:!执行shell命令
:w filename 以filename这个文件名保存当前文件
v motion :w filename 将选择的部分内容保存到filename

10.
y复制
p粘贴



