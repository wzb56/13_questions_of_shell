# 13_questions_of_shell
shell十三问--shell教程（markdown 版本）

##shell十三问之1: 何为shell？
_______________________________________


 `shell`是什么东西之前，不妨让我们重新审视`使用者`和`计算机系统`的关系：
（此处为使用者和计算机系统的关系图）


我们知道计算机的运作不能离开硬件，但使用者却无法直接操作硬件，
硬件的驱动只能通过一种称为“`操作系统`(`OS`，`Opertating System`)”的软件来管控。
事实上，我们每天所谈的“`linux`”，严格来说只是一个`操作系统`(`OS`),
我们称之为“`内核`(`kernel`)”。

然而，从使用者的角度来说，使用者没有办法直接操作一个`kernel`，
而是通过`kernel`的“外壳”程序，也就是所谓的`shell`，来与`kernel`沟通。
这也正是`kernel`跟`shell`的形象命名的的关系。如图：
 （此处为kernel-->shell关系图；)


从技术的角度来说，`shell`是一个使用者与系统的`交互界面(interface)`,
只能让使用者通过`命令行`(`command line`)来使用系统来完成工作。
因此，`shell`最简单的定义就是----`命令解释器`( `Command Interpreter`):

- 将使用者的命令翻译给kernel来处理；
- 同时，将kernel的处理结果翻译给使用者。



每次当我们完成`系统登入`(`login`), 我们就取得一个交互模式的shell，
也称之为`login shell` 或者 `primary shell`。

若从`进程`(`process`)的角度来说，我们在shell所下达的命令，均是shell所产生的`子进程`。
这种现象，我暂可称之为`fork`。

如果是执行`shell脚本`(`shell script`)的话，脚本中命令则是由另一个非交互模式的
`子shell`(`sub shell`)来执行的。 也就是primary shell产生sub shell的进程，而该sub shell
进程再产生script中所有命令的进程。
 (关于进程，我们日后有机会在补充)

这里， 我们必须知道：`kernel` 与 `shell` 是不同的两套软件，而且都是可以被替换的：

- 不同的`OS`使用不同的`kernel`;
- 同一个kernel之上，也可以使用不同的`shell`;

在`Linux`的预设系统中，通常可以找到好几种不同的`shell`,
且通常会被记录在如下文件中：
```shell
/etc/shells
```

不同的`shell`有着不同的功能，且彼此各异，或者说“大同小异”。
常见的`shell`主要分为两大主流：

1. sh：
    - burne shell (sh)
    - burne again shell (bash)
2. csh：
    - c shell (csh)
    - tc shell (tcsh)
    - korn shell (ksh)
    (FIXME)


大部分的Linux操作系统的预设shell都是`bash`，其原因大致如下两种：
- 自由软件
- 功能强大
bash是gnu project最成功的产品之一，自推出以来深受广大`Unix`用户的喜爱，
且也逐渐成为不少组织的系统标准。


##shell十三问之2：shell prompt(PS1)与Carriage Return(CR)关系
_____________________________________________________________

当你成功登陆一个shell终端的文字界面之后，大部分的情形下，
你会在屏幕上看到一个不断闪烁的方块或者底线(视不同的版本而别)，
我们称之为`游标`(`cursor`).
`cursor`作用就是告诉你接下来你从键盘输入的按键所插入的位置，
且每输入一个键，`cursor`便向右移动一个格子，
如果连续输入太多的话，则自动接在下一行输入。

假如你刚完成登陆，还没有输入任何按键之前， 
你所看到的`cursor`所在的位置的同一行的左边部分，我们称之为`提示符`(`prompt`)。

`提示符`的格式或因不同的版本而各有不同，
在Linux上，只需留意最接近`游标`的一个提示符号，通常是如下两者之一：

- $: 给一般用户账号使用;
- \#: 给root(管理员)账号使用;


事实上，shell prompt的意思很简单：
告诉shell使用者，您现在可以输入命令行了。

我们可以说，使用者只有在得到shell prompt才能打命令行，
而`cursor`是指示键盘在命令行的输入位置，使用者每输入一个键，
`cursor`就往后移动一个格，直到碰到命令行读进`CR`(`Carriage Return`, 由`Enter`键产生)字符为止。
 `CR`的意思也很简单：
使用者告诉shell：老兄，你可以执行的我命令行了。
严格来说：
所谓的命令行， 就是在`shell prompt`与`CR`之间所输入的文字。

(**question：为何我们这里坚持使用`CR`字符而不说`Enter`按键呢？
答案在后面的学习中给出**)。

不同的命令可以接受的命令的格式各有不同，
一般情况下，一个标准的命令行格式为如下所列：
```shell
command-name options argument
```

若从技术的细节上来看， 
shell会依据`IFS`(`Internal Field Seperator`) 将 command line
所输入的文字给拆解为`字段`(`word`).
然后在针对特殊的字符(meta)先做处理，最后在重组整行command line。

(**注意：请务必理解以上两句的意思，我们日后的学习中常回到这里思考**。)

其中`IFS`是shell预设使用的字段位分隔符号，可以由一个及多个如下按键组成：

- 空白键(White Space)
- 表格键(Tab)
- 回车键(Enter)

系统可以接受的命令的名称(command-name)可以从如下途径获得：

- 确的路径所指定的外部命令
- 命令的别名(alias)
- shell内建命令(built-in)
- $PATH之下的外部命令

每一个命令行均必须包含命令的名称，这是不能缺少的。

##shell十三问之3：别人echo、你也echo，是问echo知多少？
-----------------------------------------------------

承接上一章介绍的`command line`, 这里我们用`echo`这个命令加以进一步说明。

>  **温习** 
>  标准的`command line`三个组成部分：`command_name option argument`


`echo`是一个非常简单、直接的Linux命令：

```shell 
$echo argument
``` 
`echo`将argument送出到`标准输出`(`stdout`),通常是在监视器(monitor)上输出。


> **Note：**

> 在linux系统中任何一个进程默认打开三个文件：stdin、stdout、stderr.

> stdin  标准输入

> stdout 标准输出

> stderr 标准错误输出



为了更好理解，不如先让我们先跑一下`echo`命令好了：
```shell
$echo

$
```
你会发现只有一个空白行，然后又回到了`shell prompt`上了。
这是因为`echo`在预设上，在显示完argument之后，还会送出以一个换行符号
(`new-line charactor`). 
但是上面的command `echo`并没有任何argument，那结果就只剩一个换行符号。
若你要取消这个换行符号， 可以利用`echo`的`-n` 选项:
```shell
$echo -n
$
```
不妨让我们回到`command line`的概念上来讨论上例的echo命令好了：
`command line`只有command_name(`echo`)及option(`-n`),并没有显示任何`argument`。


要想看看`echo`的`argument`，那还不简单接下来，你可以试试如下的输入：
```shell
$echo first line
first line
$echo -n first line
first line $
```
以上两个`echo`命令中，你会发现`argument`的部分显示在你的屏幕，
而换行符则视 `-n` 选项的有无而别。
很明显的，第二个`echo`由于换行符被取消了，
接下来的`shell prompt`就接在输出结果的同一行了... ^_^。

事实上，`echo`除了`-n` 选项之外，常用选项有：

- -e: 启用反斜杠控制字符的转换(参考下表)
- -E: 关闭反斜杠控制字符的转换(预设如此)
- -n: 取消行末的换行符号(与-e选项下的\c字符同意)

关于`echo`命令所支持的反斜杠控制字符如下表：

|转义字符|字符的意义|
---------|----------|
|\a | ALERT / BELL(从系统的喇叭送出铃声)|
|\b | BACKSPACE, 也就是向左退格键|
|\c | 取消行末之换行符号
|\E | ESCAPE, 脱字符键|
|\f | FORMFEED, 换页字符|
|\n | NEWLINE,  换行字符 |
|\r | RETURN,  回车键|
|\t | TAB, 表格跳位键|
|\v | VERTICAL TAB, 垂直表格跳位键|
|\n | ASCII 八进制编码(以x开头的为十六进制)，此处的n为数字
|\\ | 反斜杠本身|

> **Note：**
> 上述表格的资料来自O'Reilly出版社的**Learning the Bash Shell, 2nd Ed**.

或许，我们可以通过实例来了解`echo`的选项及控制字符：

例一：
```shell
$ echo -e "a\tb\tc\n\d\te\tf"
a    b	c
d	e	f
$
```
上例中，用\t来分割abc还有def，及用\n将def换至下一行。

例二：
```shell
$echo -e "\141\011\142\011\143\012\144\011\145\011\146"
a	b	c
d	e	f
```
与例一中结果一样，只是使用ASCII八进制编码。

例三：
```shell
$echo -e "\x61\x09\x62\x09\x63\x0a\x64\x09\x65\x09\x66"
a	b	c
d	e	f
```
与例二差不多，只是这次换用ASCII的十六进制编码。


例四：
```shell
$echo -ne "a\tb\tc\nd\te\bf\a"
a       b       c
d       f $ 
```
因为e字母后面是退格键(\b)，因此输出结果就没有e了。
在结束的时听到一声铃响，是\a的杰作。
由于同时使用了-n选项，因此`shell prompt`紧接在第二行之后。
若你不用-n的话，那你在\a后再加个\c，也是同样的效果。

事实上，在日后的`shell`操作及`shell script`设计上，
`echo`命令是最常被使用的命令之一。
比方说，使用`echo`来检查变量值：
```shell
$ A=B
$ echo $A
B
$ echo $?
0
```

>  **Note:**
>  关于变量的概念，我们留到以下的两章跟大家说明。


好了，更多的关于`command line`的格式， 以及`echo`命令的选项，
请您自行多加练习、运用了...










