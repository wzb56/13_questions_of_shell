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





