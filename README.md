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
