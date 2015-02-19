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



##shell十三问之4：""(双引号)与''(单引号)差在哪？
------------------------------------------------

还是回到我们的`command line`来吧...

经过前面两章的学习，应该很清楚当你在`shell prompt`后面敲打键盘,
直到按下`Enter`键的时候，你输入的文字就是`command line`了，
然后`shell`才会以进程的方式执行你所交给它的命令。
但是，你又可知道：你在`command line`中输入的每一个文字，
对`shell`来说，是有类别之分的呢？

简单而言，(我不敢说精确的定义，注1),
`command line`的每一个`charactor`, 分为如下两种：

- literal：也就是普通的纯文字，对`shell`来说没特殊功能；
- meta: 对`shell`来说，具有特定功能的特殊保留元字符。

> **Note:**

> 对于`bash shell`在处理`comamnd line`的顺序说明，
> 请参考O'Reilly出版社的**Learning the Bash Shell，2nd Edition**，
> 第177-180页的说明，尤其是178页的流程图：Figure 7-1 ...


`literal`没什么好谈的，
像abcd、123456这些"文字"都是literal...(so easy? ^_^)
但meta却常使我们困惑...(confused?)
事实上，前两章，我们在`command line`中已碰到两个
似乎每次都会碰到的meta：

- `IFS`：有`space`或者`tab`或者`Enter`三者之一组成(我们常用space)
- `CR`: 由`Enter`产生；

`IFS`是用来拆解`command line`中每一个词(word)用的，
因为`shell command line`是按词来处理的。
而`CR`则是用来结束`command line`用的，这也是为何我们敲`Enter`键，
命令就会跑的原因。

除了常用的`IFS`与`CR`, 常用的meta还有：

|meta字符| meta字符作用|
|--------|-------------|
|= |设定变量|
|$ | 作变量或运算替换(请不要与`shell prompt`混淆)|命令
|>| 输出重定向(重定向stdout)|
|<|输入重定向(重定向stdin)|
|\||命令管道|
|&|重定向file descriptor或将命令至于后台(bg)运行|
|()|将其内部的命令置于nested subshell执行，或用于运算或变量替换|
|{}|将期内的命令置于non-named function中执行，或用在变量替换的界定范围|
|;|在前一个命令执行结束时，而忽略其返回值，继续执行下一个命令|
|&&|在前一个命令执行结束时，若返回值为true，继续执行下一个命令|
|\|\||在前一个命令执行结束时，若返回值为false，继续执行下一个命令|
|!|执行histroy列表中的命令|
|... | ...|

    
假如我们需要在`command line`中将这些保留元字符的功能关闭的话，
就需要quoting处理了。

在`bash`中，常用的quoting有以下三种方法：

- hard quote：''(单引号)，凡在hard quote中的所有meta均被关闭；
- soft quote：""(双引号)，凡在soft quote中大部分meta都会被关闭，但某些会保留(如$);
- escape: \ (反斜杠)，只有在紧接在escape(跳脱字符)之后的单一meta才被关闭；

> **Note:**

>  在soft quote中被豁免的具体meta清单，我不完全知道，
>  有待大家补充，或通过实践来发现并理解。


下面的例子将有助于我们对quoting的了解：
```shell
$ A=B C #空白符未被关闭，作为IFS处理
$ C：command not found.
$ echo $A

$ A="B C" #空白符已被关掉，仅作为空白符
$ echo $A
B C
```
在第一个给A变量赋值时，由于空白符没有被关闭，
`command line` 将被解释为：
`A=B 然后碰到<IFS>，接着执行C命令`
在第二次给A变量赋值时，由于空白符被置于soft quote中，
因此被关闭，不在作为`IFS`；
`A=B<space>C`
事实上，空白符无论在soft quote还是在hard quote中，
均被关闭。Enter键字符亦然：

```shell
$ A=`B
> C
> '
$ echo "$A"
B
C
```
在上例中，由于`enter`被置于hard quote当中，因此不再作为`CR`字符来处理。
这里的`enter`单纯只是一个断行符号(new-line)而已，
由于`command line`并没得到`CR`字符，
因此进入第二个`shell prompt`(`PS2`, 以>符号表示)，
`command line`并不会结束，直到第三行，
我们输入的`enter`并不在hard quote里面，
因此没有被关闭，
此时，`command line`碰到`CR`字符，于是结束，交给shell来处理。

上例的`Enter`要是被置于soft quote中的话，`CR`字符也会同样被关闭：

```shell
$ A="B
> C
> "
$ echo $A
B C
```
然而，由于 `echo $A`时的变量没有置于soft quote中，
因此，当变量替换完成后，并作命令行重组时，`enter`被解释为`IFS`，
而不是new-line字符。

同样的，用escape亦可关闭CR字符：
```shell
$ A=B\
> C\
>
$ echo $A
BC
```
上例中的，第一个`enter`跟第二个`enter`均被escape字符关闭了，
因此也不作为`CR`来处理，但第三个`enter`由于没有被escape，
因此，作为`CR`结束`command line`。
但由于`enter`键本身在shell meta中特殊性，在 \ escape字符后面
仅仅取消其`CR`功能， 而不保留其IFS功能。

你或许发现光是一个`enter`键所产生的字符，就有可能是如下这些可能：

- CR
- IFS
- NL(New Line)
- FF(Form Feed)
- NULL
- ...

至于，什么时候解释为什么字符，这个我就没法去挖掘了，
或者留给读者君自行慢慢摸索了...^-^


至于soft quote跟hard quote的不同，主要是对于某些meta的关闭与否，以$来做说明：
```shell
$ A=B\ C
$ echo "$A"
B C
$ echo '$A'
$A
```
在第一个`echo`命令行中，$被置于soft quote中，将不被关闭，
因此继续处理变量替换，
因此，`echo`将A的变量值输出到屏幕，也就是"B C"的结果。

在第二个`echo`命令行中，$被置于hard quote中，则被关闭，
因此，$只是一个$符号，并不会用来做变量替换处理，
因此结果是$符号后面接一个A字母：$A.


>  **练习与思考:** 如下结果为何不同？

>  tips: 单引号和双引号，在quoting中均被关闭了。


```shell
$ A=B\ C
$ echo '"$A"'  #最外面的是单引号
"$A"
$ echo "'$A'"  #最外面的是双引号
'B C'
```

------------------------------------
在CU的shell版里，我发现很多初学者的问题，
都与quoting的理解有关。
比方说，若我们在awk或sed的命令参数中，
调用之前设定的一些变量时，常会问及为何不能的问题。

要解决这些问题，关键点就是：**区分出 shell meta 与 command meta**

前面我们提到的那些meta，都是在command line中有特殊用途的，
比方说{}就是将一系列的command line置于不具名的函数中执行(可简单视为command block)，
但是，awk却需要用{}来区分出awk的命令区段(BEGIN,MAIN,END).
若你在command line中如此输入：
```shell
$ awk {print $0} 1.txt
```
由于{}在shell中并没有关闭，那shell就将{print $0}视为command block，
但同时没有`;`符号作命令分隔，因此，就出现awk语法错误结果。

要解决之，可用hard quote:
```shell
awk '{print $0}'
```
上面的hard quote应好理解，就是将原来的
{、<space>、$、}这几个shell meta关闭，
避免掉在shell中遭到处理，而完整的成为awk的参数中command meta。

> **Note:**

> awk中使用的$0 是awk中内建的field nubmer，而非awk的变量，
> awk自身的变量无需使用$.

要是理解了hard quote的功能，在来理解soft quote与escape就不难：
```shell
awk "{print \$0}" 1.txt
awk \{print \$0\} 1.txt
```
然而，若要你改变awk的$0的0值是从另一个shell变量中读进呢？
比方说：已有变量$A的值是0， 那如何在`command line`中解决
awk的$$A呢？
你可以很直接否定掉hard quote的方案：
```shell
$ awk '{print $$A}' 1.txt
```
那是因为$A的$在hard quote中是不能替换变量的。

聪明的读者(如你！)，经过本章的学习，我想，你应该可以理解为
为何我们可以使用如下操作了吧：
```shell
A=0
awk "{print \$$A}" 1.txt
awk  \{print\ \$$A\} 1.txt
awk '{print $'$A'}' 1.txt
awk '{print $'"$A"'}' 1.txt
```
或许，你能给出更多方案... ^_^

更多练习：
- http://bbs.chinaunix.net/forum/viewtopic.php?t=207178
  一个关于read命令的小问题：
  很早以前觉得很奇怪：执行read命令，然后读取用户输入给变量赋值，
  但如果输入是以空格键开始的话，这空格会被忽略，比如：
```shell
read a  #输入：    abc
echo "$a" #只输出abc
```
原因:
    变量a的值，从终端输入的值是以IFS开头，而这些IFS将被shell解释器忽略(trim)。
    应该与shell解释器分词的规则有关；

```shell
read a  #输入：\ \ \ abc
echo "$a" #只输出abc
```
需要将空格字符转义


> **Note:** 

>  IFS   Internal field separators, normally  space,  tab,  and newline (see Blank Interpretation section).
>  ......
>  Blank Interpretation
>   After parameter and command  substitution,  the  results  of substitution  
>   are scanned for internal field separator characters (those found in IFS) 
>   and split  into  distinct  arguments  where  such characters are found. 
>   Explicit null arguments ("" or  '')  are  retained.  
>   Implicit  null  arguments(those  resulting  from  parameters that have no values) 
>   are removed. 
>   (refre to: man sh)

解决思路：

1. shell command line 主要是将整行line给分解(break down)为每一个单词(word);
2. 而词与词之间的分隔符就是IFS (Internal Field Seperator)。
3. shell会对command line作处理(如替换，quoting等), 然后再按词重组。(注：别忘了这个重组特性)
4. 当你用IFS来事开头一个变量值，那shell会先整理出这个词，然后在重组command line。
5.然而，你将IFS换成其他，那shell将视你哪些space/tab为“词”，而不是IFS。那在重组时，可以得到这些词。

若你还是不理解，那来验证一下下面这个例子：
```shell
$ A="  abc" 
$ echo $A
abc
$ echo "$A" #note1
   abc
$ old_IFS=$IFS
$ IFS=;
$ echo $A
   abc
$ IFS=$old_IFS
$ echo $A
abc
```
> **Note:**

>  1. 这里是用 soft quoting 将里面的 space 关闭，使之不是 meta(IFS)，
>  而是一个literal(white space);

>  2. IFS=; 意义是将IFS设置为空字符，因为;是shell的元字符(meta);


问题二：为什么多做了几个分号，我想知道为什么会出现空格呢？
```shell
$ a=";;;test"                              
$ IFS=";"                                  
$ echo $a                                  
   test                                                                         
$ a="   test"                              
$ echo $a                                  
   test                                                                         
$ IFS=" "                                  
$ echo $a                                  
test    
```

解答：

这个问题，出在`IFS=;`上。
因为这个`;`在问题一中的command line上是一个meta,
并非`";"`符号本身。
因此，`IFS=;`是将IFS设置为 null charactor
(不是space、tab、newline)。

要不是试试下面这个代码片段：
```shell
$ old_IFS=$IFS
$ read A
;a;b;c
$ echo $A
;a;b;c
$ IFS=";"  #Note2
$ echo $A
a b c
```
> **Note:**

> 要关闭`;`可用`";"`或者`';'`或者`\;`。


- http://bbs.chinaunix.net/forum/viewtopic.php?t=216729

思考问题二：文本处理：读文件时，如何保证原汁原味。
```shell
cat file | while read i
do
   echo $i
done
```
文件file的行中包含若干空，经过read只保留不重复的空格。
如何才能所见即所得。

```shell
cat file | while read i
do
   echo "X${i}X"
done
```
从上面的输出，可以看出read，读入是按整行读入的;
不能原汁原味的原因：

1.  如果行的起始部分有IFS之类的字符，将被忽略;
2.  `echo $i`的解析过程中，首先将$i替换为字符串，
 然后对echo 字符串中字符串分词，然后命令重组，输出结果;
 在分词，与命令重组时，可能导致多个相邻的IFS转化为一个;

```shell
cat file | while read i
do
  echo "$i"
done
```
以上代码可以解决原因2中的，command line的分词和重组导致meta字符丢失；
但仍然解决不了原因1中，read读取行时，忽略行起始的IFS meta字符。

回过头来看上面这个问题：为何要原汁原味呢？
cat命令就是原汁原味的，只是shell的read、echo导致了某些shell的meta字符丢失;

如果只是IFS meta的丢失，可以采用如下方式：
将IFS设置为null，即`IFS=;`, 
在此再次重申此处`;`是shell的meta字符,而不是literal字符;
因此要使用literal的	`;`应该是`\;`
或者关闭meta 的(soft/hard) quoting的`";"`或者`';'`。

因此上述的解决方案是：
```shell
old_IFS=$IFS
IFS=; #将IFS设置为null
cat file | while read i
do
  echo "$i"
done
IFS=old_IFS #恢复IFS的原始值
```
现在，回过头来看这个问题，为什么会有这个问题呢；
其本源的问题应该是没有找到解决原始问题的最合适的方法，
而是采取了一个迂回的方式来解决了问题；

因此，我们应该回到问题的本源，重新审视一下，问题的本质。
如果要精准的获取文件的内容，应该使用od或者hexdump会更好些。



##shell十三问之5:问var=value 在export前后的差在哪?
----------------------------------------------------


这次让我们暂时丢开`command line`, 
先了解一下bash变量(variable)吧...

所谓的变量，就是利用一个固定的"名称"(name),
来存取一段可以变化的"值"(value)。


###1. 变量设定(set)
在bash中， 你可以用"="来设定或者重新定义变量的内容：
```shell
name=value
```

在设定变量的时候，得遵守如下规则：
 
- 等号左右两边不能使用分隔符号(IFS),也应避免使用shell的保留元字符(meta charactor); 
- 变量的名称(name)不能使用$符号;
- 变量的名称(name)的首字符不能是数字(number)。
- 变量的名称(name)的长度不可超过256个字符。
- 变量的名称(name)及变量的值的大小写是有区别的、敏感的(case sensitive，) 

如下是一些变量设定时常见的错误：
```shell
A= B #=号前后不能有IFS
1A=B #变量名称不能以数字开头
$A=B #变量的名称里有$
a=B  #这跟a=b是不同的,(这不是错误，提醒windows用户)
```

如下则是可以接受的设定：
```shell
A=" B" #IFS被关闭，参考前面的quoting章节
A1=B   #并非以数字开头
A=$B   #$可用在变量的值内
This_Is_A_Long_Name=b #可用_连接较长的名称或值，且有大小区别；
```

###2. 变量替换(substitution)
shell 之所以强大，其中的一个因素是它可以在命令行中对变量作
替换(substitution)处理。
在命令行中使用者可以使用$符号加上变量名称(除了用=定义变量名称之外)，
将变量值给替换出来，然后再重新组建命令行。

比方:
```shell
$ A=ls
$ B=la
$ C=/tmp
$ $A -$B $C
```
以上命令行的第一个`$`是`shell prompt`, 并不在命令行之内。
必须强调的是，我们所提的变量替换，只发生在`command line`上面。
(是的，请让我们再次回到命令行吧！) 仔细分析,最后那行
`command line`,不难发现在被执行前(在输入`CR`字符之前)，
`$`符号对每一个变量作替换处理(将变量的值替换出来再重组命令行),
最后会得出如下命令行：
```shell
ls -la /tmp
```

还记得第二章，我请大家"务必理解"的那两句吗？
若你忘了，我这里重贴一遍：

> **Note:**

> 若从技术的细节来看，`shell`会依据`IFS`(Internal Field Seperator)
> 将`command line`所输入的文字拆解为"字段"(word/field)。
> 然后再针对特殊字符(meta)先作处理，最后重组整行`command line`。


这里的`$`就是`command line`中最经典的meta之一了，
就是作变量替换的。在日常的shell操作中，
我们常会使用`echo`命令来查看特定的变量的值，
例如：
```shell
$ echo $A -$B $C
```
我们已学过，`echo`命令只单纯将其argument送至"标准输出"(stdout, 通常是我们的屏幕)。
所以上面的命令会在屏幕上得到如下结果：
```shell
ls -al /tmp
```
这是由于`echo`命令在执行时，会先将`$A` (ls)、`$B` (la)跟`$C` (/tmp)给替换出来；
利用shell对变量的替换处理能力，我们在设定变量时就更为灵活了：
```shell
A=B
B=$A
```
这样，B的变量值就可继承A变量"当时"的变量值了。
不过，不要以"数学逻辑"来套用变量的设定，比方说：
```shell
A=B
B=C
```
这样，并不会让A的变量值变成C。再如：
```shell
A=B
B=$A
A=C
```
同样也不会让B的值变成C。

上面是单纯定义了两个不同名称的变量：
A 与 B, 它们的取值分别是C与B。

若变量被重复定义的话，则原有值为新值所取代。(这不正是"可变的量"吗？^_^)
当我们在设定变量的时候，请记住这点：**用一个名称存储一个数值**， 仅此而已。

此外， 我们也可以利用命令行的变量替换能力来"扩充"(append)变量的值：
```shell
A=B:C:D
A=$A:E

```
这样， 第一行我们设定A的值为"B:C:D", 
然后,第二行再将值扩充为"B:C:D:E"。

上面的扩充的范例，我们使用分隔符号(:)来达到扩充的目的，
要是没有分隔符的话，如下是有问题的：
```shell
A=BCD
B=$AE
```
因为第二次是将A的值继承$AE的替换结果，而非$A再加E。
要解决此问题，我们可用更严谨的替换处理：
```shell
A=BCD
A=${A}E
```

上例中，我们使用{}将变量名称范围给明确定义出来，
如此一来， 我们就可以将A的变量值从BCD给扩充为BCDE。

> **Tips:**
> 关于${name}事实上还可以做到更多的变量处理能力，
> 这些均属于比较进阶阶段的变量处理，现阶段暂不介绍了，
> 请大家自行参考资料。

###3. export 变量

严格来说，我们在当前shell中所定义的变量，均属于
"本地变量"(local variable), 只有经过`export`命令的
"输出"处理，才能成为"环境变量"(environment variable)：
```shell
$ A=B
$ export A
```
或者
```shell
$ export A=B
```
经过`export`输出处理之后，变量A就能成为一个环境变量
供其后的命令使用。在使用`export`的时候，请别忘记
shell在命令行对变量的"替换"(substitution)处理。
比方说：
```shell
$ A=B
$ B=C
$ export $A
```
上面的命令并未将A输出为"环境变量"，而是将B导出
这是因为在这个命令行中，$A会首先被替换为B,然后在"塞回"
作`export`的参数。

要理解这个`export`，事实上需要从process(进程)的角度来理解才能透彻。
我们将于下一章为大家说明process(进程)的概念，敬请留意。


####4. 取消变量(unset)
要取消一个变量，在bash中可使用`unset`命令来处理：
```shell
    unset A
```
与`export`一样，`unset`命令行，也同样会作
变量替换(这其实是shell的功能之一)，
因此:
```shell
$ A=B
$ B=C
$ unset $A
```
事实上，所取消的是变量B而不是A。

此外，变量一旦经过unset取消之后，
其结果是将整个变量拿掉，而不是取消变量的值。

如下两行其实是很不一样的：
```shell
$ A=
$ unset A
```
第一行只是将变量A设定为"空值"(null value),
但第二行则是让变量A不存在。
虽然用眼睛来看，
这两种变量的状态在如下的命令结果中都是一样的：
```shell
$ A=
$ echo $A

$ unset A
$ echo $A
```
请学员务必能识别null value 与 unset的本质区别，
这在一些进阶的变量处理上是很严格的。

比方说：
```shell
$ str=    #设为null
$ var=${str=expr} #定义var
$ echo $var

$ echo $str

$ unset str #取消str
$ var=${str=expr} #定义var
$ echo $var
expr
$ echo $str
expr
```
聪明的读者(yes, you!)，稍加思考的话，
应该不难发现为何同样的var=${str=expr}
在str为null与unset之下的不同吧？
若你看不出来，那可能是如下原因之一：

- 你太笨了
- 不了解 var=${str=expr} 这个进阶处理
- 对本篇说明还没有来得及消化吸收
- 我讲得不好

不知，您选哪个呢？...... ^_^.


##shell十三问之6：exec跟source差在哪？
-------------------------------------

这次让我们从CU  shell版的一个实例帖子来谈起吧：
(论坛改版后，原链接已经失效)

例中的提问原文如下：

> **帖子提问:**

> cd /etc/aa/bb/cc可以执行
> 但是把这条命令放入shell脚本后，shell脚本不执行！
> 这是什么原因？

意思是：运行shell脚本，并没有移动到/etc/aa/bb/cc目录。

我当时如何回答暂时别去深究，先让我们了解一下进程
(process)的概念好了。

首先，我们所执行的任何程序，都是父进程(parent process)产生的一个
子进程(child process),子进程在结束后，将返回到父进程去。
此现象在Linux中被称为`fork`。

(为何要称为fork呢？ 嗯，画一下图或许比较好理解...^_^)

当子进程被产生的时候，将会从父进程那里获得一定的资源分配、及
(更重要的是)继承父进程的环境。

让我们回到上一章所谈到的"环境变量"吧：
**所谓环境变量其实就是那些会传给子进程的变量**。
简单而言, "遗传性"就是区分本地变量与环境变量的决定性指标。
然而，从遗传的角度来看，我们不难发现环境变量的另一个重要特征：
**环境变量只能从父进程到子进程单向传递。
换句话说：在子进程中环境如何变更，均不会影响父进程的环境。**

接下来，在让我们了解一下shell脚本(shell script)的概念.
所谓shell script 讲起来很简单，就是将你平时在shell prompt输入的多行
`command line`, 依序输入到一个文件文件而已。


再结合以上两个概念(process + script)，那应该不难理解如下的这句话的意思了：
正常来说，当我们执行一个shell script时，其实是先产生一个sub-shell的子进程，
然后sub-shell再去产生命令行的子进程。
然则，那让我们回到本章开始时，所提到的例子在重新思考：

> **帖子提问:**

> cd /etc/aa/bb/cc可以执行
> 但是把这条命令放入shell脚本后，shell脚本不执行！
> 这是什么原因？

意思是：运行shell脚本，并没有移动到/etc/aa/bb/cc目录。

我当时的答案是这样的：

> 因为，我们一般跑的shell script是用sub-shell去执行的。
> 从process的概念来看，是 parent process产生一个child process去执行，
> 当child结束后，返回parent, 但parent的环境是不会因child的改变而改变的。
> 所谓的环境变量元数很多，如effective id(euid)，variable, working dir等等...
> 其中的working dir($PWD) 正是楼主的疑问所在：
> 当用sub-shell来跑script的话，sub-shell的$pwd会因为cd而变更，
> 但返回primary shell时，$PWD是不会变更的。


能够了解问题的原因及其原理是很好的，但是？
如何解决问题，恐怕是我们更应该感兴趣的是吧？

那好，接下来，再让我们了解一下`source`命令好了。
当你有了`fork`的概念之后，要理解`soruce`就不难：

所谓`source`，就是让script在当前shell内执行、
而不是产生一个sub-shell来执行。
由于所有执行结果均在当前shell内执行、而不是产生一个sub-shell来执行。

因此, 只要我们原本单独输入的script命令行，变成`source`命令的参数，
就可轻而易举地解决前面提到的问题了。

比方说，原本我们是如此执行script的：
```shell
$ ./my_script.sh
```
现在改成这样既可：
```shell
$ source ./my_script.sh
```
或者：
```shell
$ . ./my_script.sh
```

说到这里，我想，各位有兴趣看看`/etc`底下的众多设定的文件，
应该不难理解它们被定义后，如何让其他script读取并继承了吧？

若然，日后，你有机会写自己的script，
应也不难专门指定一个设定的文件以供不同的script一起"共用"了... ^_^

okay,到这里，若你搞懂`fork`与`source`的不同，
那接下来再接受一个挑战：
> 那`exec`又与`source`/`fork`有何不同呢？

哦...要了解`exec`或许较为复杂，尤其是扯上`File Decscriptor`的话...
不过，简单来说：
> `exec` 也是让script在同一个进程上执行，但是原有进程则被结束了。
> 简言之，原有进程能否终止，就是`exec`与`source`/`fork`的最大差异了。

嗯，光是从理论去理解，或许没那么好消化，
不如动手"实践+思考"来得印象深刻哦。

下面让我们为两个简单的script，分别命名为1.sh以及2.sh

1.sh

```shell
#!/bin/bash 

A=B 
echo "PID for 1.sh before exec/source/fork:$$"

export A
echo "1.sh: \$A is $A"

case $1 in
        exec)
                echo "using exec..."
                exec ./2.sh ;;
        source)
                echo "using source..."
                . ./2.sh ;;
        *)
                echo "using fork by default..."
                ./2.sh ;;
esac

echo "PID for 1.sh after exec/source/fork:$$"
echo "1.sh: \$A is $A"
```
2.sh

```shell
#!/bin/bash

echo "PID for 2.sh: $$"
echo "2.sh get \$A=$A from 1.sh"

A=C
export A
echo "2.sh: \$A is $A"
```

然后分别跑如下参数来观察结果：
```shell
$ ./1.sh fork
$ ./1.sh source
$ ./1.sh exec
```
好了，别忘了仔细比较输出结果的不同及背后的原因哦...
若有疑问，欢迎提出来一起讨论讨论~~~~

happy scripting！ ^_^

##shell十三问之7：()与{}差在哪？
--------------------------------


嗯，这次轻松一下，不讲太多... ^_^

先说一下，为何要用()或者{}好了。

许多时候，我们在shell操作上，需要在
一定的条件下执行多个命令，也就是说，
要么不执行，要么就全执行，而不是每次
依序的判断是否要执行下一个命令。

或者，要从一些命令执行的先后次序中得到结果，
如算术运算的2*(3+4)那样...

这时候，我们就可以引入"`命令群组`"(`command group`)
的概念：将许多命令集中处理。

在shell `command line`中，一般人或许不太计较`()`与
`{}`这两对符号的差异，虽然两者都可以将多个命令当作群组处理，
但若从技术细节上，却是很不一样的：

- `()` 将`command group`置于`sub-shell`(`子shell`)中去执行，也称 `nested sub-shell`。
- `{}` 则是在同一个`shell`内完成，也称`non-named command group`。

若你对上一章的fork与source的概念还记得的话，
那就不难理解两者的差异了。

要是在 `command group`中扯上变量及其他环境的修改，
我们可以根据不同的需求来使用`()`或`{}`。
通常而言, 若所作的修改是临时的，且不想影响原有或以后的设定，
那我们就使用`nested sub-shell`, 即`()`;
反之，则用`non-named command group`, 即`{}`。

是的，光从`command line`来看，`()` 与 `{}`差别就讲完了，够轻松吧~~~, ^_^

然而，这两个`meta`用在其他`command meta`或领域中(如Regular Expression)，
还是有很多差别的。
只是，我不打算再去说明了，留给读者慢慢发掘好了...

我这里只想补充一个概念，就是`function`。
所谓`function`，就是用一个名字去命名一个`command group`, 
然后再调用这个名字去执行`command group`。

从`non-named command group`来推断，
大概你也可以推测到我要说的是`{}`了吧？(yes! 你真聪明 ^_^)

在bash中，function的定义方式有两种：

- 方式一：
```shell
function function_name {
    command1
	command2
	command3
	.....
}
```
- 方式二：
```shell
function_name () {
	command1
	command2
	command3
	......
}
```

用哪一种方式无所谓，
只是碰到所定义的名称与现有的命令或者别名冲突的话，
方式二或许会失败。
但方式二起码可以少打个`function`这一串英文字符，
对懒人来说(如我)，有何乐而不为呢？...^_^

`function` 在一定程度上来说，也可以称为"函数"，
但请不要与传统编程所使用的"函数"(library)搞混了，
毕竟两者差异很大。
唯一相同的是，我们都可以随时用"已定义的名称"来调用它们...

若我们在shell操作中，需要不断地重复某些命令，
我们首先想到的，或许是将命令写成shell脚本(shell script)。
不过，我们也可以写成function, 
然后在command line中打上function_name就可当一般的shell script使用了。

若只是你在shell中定义的`function`, 
除了用`unset` function_name取消外，
一旦你退出shell， function也跟着消失。
然而，在script中使用function却有许多好处，
除了提高整体script的执行性能外(因为已经载入)，
还可以节省许多重复的代码......

简单而言，若你会将多个命令写成script以供调用的话，
那你可以将function看成script中script。... ^_^

而且通过上一章节介绍的`source`命令， 
我们可以自行定义许许多多好用的function，
在集中写在特定文件中，
然后，在其他的script中用`source`将它们载入，并反复执行。

若你是`RedHat Linux`的使用者，
或许，已经猜出 `/etc/rc.d/init.d/functions`这个文件时啥作用了~~~ ^_^

okay，说要轻松点的嘛，那这次就暂时写到这吧。
祝大家学习愉快，^_^

##shell十三问之8: $(())与$()还有${}差在哪？
--------------------------------------------

我们上一章介绍了()与{}的不同，
这次让我们扩展一下，看看更多的变化：
$()与${}又是啥玩意儿呢？

在bash shell中, `$()`与\`\`(反引号)都是用来做
`命令替换`(command substitution)的。

所谓的`命令替换`与我们第五章学过的变量替换差不多，
都是用来`重组命令行`：
完成 \`\` 或者`$()`里面的
命令，将其结果替换出来，
再重组命令行。

例如：
```shell
$ echo the last sunday is $(date -d "last sunday" +%Y-%m-%d)
```
如此便可方便得到上一个星期天的日期了...^_^

在操作上， 用$()或\`\`都无所谓,
只是我个人比较喜欢用$(),理由是：

1. \`\`(反引号)很容易与''(单引号)搞混乱，尤其对初学者来说。
    有时在一些奇怪的字形显示中，两种符号是一模一样的(只取两点)。
	当然了有经验的朋友还是一眼就能分辨两者。只是，若能更好的避免混乱，
	又何乐而不为呢？ ^_^

2.  在多次的复合替换中， \`\`需要额外的转义(escape, \)处理，而$()则比较直观。
例如，一个错误的使用的例子：
```shell
	command1 `command2 `command3` `
```
原来的本意是要在command2 \`command3\` ,
先将command3替换出来给command2处理，
然后再将command2的处理结果，给command1来处理。
然而真正的结果在命令行中却是分成了\`command2\`与 \`\`。

正确的输入应该如下：
```shell
	command1 `command2 \`command3\` `
```
 要不然换成$()就没有问题了：

```shell
 	command1 $(commmand2 $(command3))
```

只要你喜欢，做多少层的替换都没有问题~~~^_^

不过，$()并不是没有弊端的...
首先，\`\`基本上可用在所有的unix shell中使用，
若写成 shell script，其移植性比较高。
而$()并不是每一种shell都能使用，我只能说，
若你用bash2的话，肯定没问题... ^_^


接下来，再让我们看看${}吧...它其实就是用来做
变量替换用的啦。
一般情况下，$var与${var}并没有啥不一样。
但是用${}会比较精准的界定变量名称的范围，
比方说:
```shell
$ A=B
$ echo $AB
```
原本是打算先将$A的结果替换出来，
然后在其后补一个字母B；
但命令行上，
真正的结果却是替换变量名称为AB的值出来...
若使用${}就没有问题了：
```shell
$ A=B
$ echo ${A}B
$ BB
```
不过，假如你只看到`${}`只能用来界定变量名称的话，
那你就实在太小看bash了。

为了完整起见，我这里再用一些例子加以说明`${}`的一些
特异功能：
假设我们定义了一个变量file为：
```shell 
file=/dir1/dir2/dir3/my.file.txt
```
我们可以用`${}`分别替换获得不同的值：

####1. shell字符串的非贪婪(最小匹配)左删除
-----------

```shell
${file#*/}  #其值为：dir1/dir2/dir3/my.file.txt
```
拿掉第一个`/`及其左边的字符串，其结果为：
`dir1/dir2/dir3/my.file.txt` 。
```shell
${file#*.}  #其值为：file.txt
```
拿掉第一个`.`及其左边的字符串，其结果为：
`file.txt` 。


####2. shell字符串的贪婪(最大匹配)左删除：
------
```shell
${file##*/} #其值为：my.file.txt
```
拿掉最后一个`/`及其左边的字符串，其结果为：
`my.file.txt`

```shell
${file##*.} #其值为：txt
```
拿掉最后一个`.`及其左边的字符串，其结果为：
`txt`

####3. shell字符串的非贪婪(最小匹配)右删除：
----------
```shell
${file%/*}  #其值为：/dir1/dir2/dir3
```
拿掉最后一个`/`及其右边的字符串，其结果为：
`/dir1/dir2/dir3`。

```shell
${file%.*}  #其值为：/dir1/dir2/dir3/my.file
```
拿掉最后一个`.`及其右边的字符串，其结果为：
`/dir1/dir2/dir3/my.file`。

####4. shell字符串的贪婪(最大匹配)右删除：
-----
```shell
${file%%/*}  #其值为：其值为空。
```
拿掉第一个`/`及其右边的字符串，其结果为：
空串。

```shell
${file%%.*}  #其值为：/dir1/dir2/dir3/my。
```
拿掉第一个`.`及其右边的字符串，其结果为：
/dir1/dir2/dir3/my。

> **Tips:**

> 记忆方法：

> `#`是去掉左边(在键盘上`#`在`$`的左边);

> `%`是去掉右边(在键盘上`%`在`$`的右边);

>  单个符号是最小匹配;

>  两个符号是最大匹配;


####5. shell字符串取子串：
----

```shell
 ${file:0:5} #提取最左边的5个字符：/dir1
 ${file:5:5} #提取第5个字符及其右边的5个字符:/dir2
```
shell字符串取子串的格式：`${s:pos:length}`,
取字符串s的子串：从pos位置开始的字符(包括该字符)的长度为length的的子串;
其中pos为子串的首字符，在s中位置；
length为子串的长度;

> **Note:** 字符串中字符的起始编号为0.


####6. shell字符串变量值的替换：
-----

```shell
${file/dir/path}  #将第一个dir替换为path：/path1/dir2/dir3/my.file.txt
${file//dir/path} #将全部的dir替换为path：/path1/path2/path3/my.file.txt
```

shell字符串变量值的替换格式：

- 首次替换：
`${s/src_pattern/dst_pattern}` 将字符串s中的第一个src_pattern替换为dst_pattern。

- 全部替换：
`${s//src_pattern/dst_pattern}` 将字符串s中的所有出现的src_pattern替换为dst_pattern.


####7. ${}还可针对变量的不同状态(没设定、空值、非空值)进行赋值：
------

- `${file-my.file.txt}` #如果file没有设定，则使用
使用my.file.txt作为返回值, 否则返回${file};(空值及非空值时，不作处理。);

- `${file:-my.file.txt}` #如果file没有设定或者${file}为空值, 均使用my.file.txt作为其返回值，否则，返回${file}.(${file} 为非空值时，不作处理);


- `${file+my.file.txt}` #如果file已设定(为空值或非空值), 则使用my.file.txt作为其返回值，否则不作处理。(未设定时，不作处理);

- `${file:+my.file.txt}` #如果${file}为非空值, 则使用my.file.txt作为其返回值，否则，(未设定或者为空值时)不作处理。

- `${file=my.file.txt}` #如果file为设定，则将file赋值为my.file.txt，同时将${file}作为其返回值；否则，file已设定(为空值或非空值)，则返回${file}。

- `${file:=my.file.txt}` #如果file未设定或者${file}为空值, 则my.file.txt作为其返回值，
同时，将${file}赋值为my.file.txt，否则，(非空值时)不作处理。

- `${file?my.file.txt}` #如果file没有设定，则将my.file.txt输出至STDERR, 否侧，
已设定(空值与非空值时)，不作处理。

- `${file:?my.file.txt}` #若果file未设定或者为空值，则将my.file.txt输出至STDERR，否则，
非空值时，不作任何处理。

> **Tips:**

> 以上的理解在于，你一定要分清楚，`unset`与`null`以及non-null这三种状态的赋值；
>一般而言，与null有关，若不带`:`, null不受影响；
> 若带 `:`, 则连null值也受影响。


####8. 计算shell字符串变量的长度：`${#var}`
--------------
```shell
 ${#file}  #其值为27, 因为/dir1/dir2/dir3/my.file.txt刚好为27个字符。
```

####9. bash数组(array)的处理方法
-------------------

接下来，为大家介绍一下bash的数组(array)的处理方法。
一般而言, `A="a b c def"`
这样的变量只是将`$A`替换为一个字符串，
但是改为 `A=(a b c def)`,
则是将`$A`定义为数组....

#####1). 数组替换方法可参考如下方法：
	
```shell
${A[@]} #方法一
${A[*]} #方法二
```
以上两种方法均可以得到：a b c def, 即数组的全部元素。

#####2). 访问数组的成员:
```shell
${A[0]}
```
其中，`${A[0]}`可得到a, 即数组A的第一个元素，
而 `${A[1]}`则为数组A的第二元素，依次类推。

#####3). 数组的length：
```shell
${#A[@]} #方法一
${#A[*]} #方法二
```	
以上两种方法均可以得到数组的长度: 4, 即数组的所有元素的个数。
 	
回忆一下，针对字符串的长度计算，使用`${#str_var}`; 
我们同样可以将该方法应用于数组的成员:
```shell
${#A[0]}
```
其中，`${#A[0]}`可以得到：1，即数组A的第一个元素(a)的长度;
同理，`${#A[3]}`可以得到: 3, 即数组A的第4个元素(def)的长度。

#####4). 数组元素的重新赋值：
```shell
A[3]=xyz
```
将数组A的第四个元素重新定义为xyz。

> **Tips:**

> 诸如此类的...

> 能够善用bash的$()与${}可以大大提高及
> 简化shell在变量上的处理能力哦~~~^_^


####10. $(())作用:
----
好了，最后为大家介绍`$(())`的用途吧：
**`$(())`是用来作整数运算的**。

在bash中， `$(())`的整数运算符号大致有这些：

-  \+\- *  /    #分别为"加、减、乘、除"。
-  %            #余数运算,(模数运算)
-  & | ^ !      #分别为"AND、OR、XOR、NOT"运算。

例如：
```shell
$ a=5; b=7; c=2;
$ echo $(( a + b * c ))
19
$ echo $(( (a + b)/c ))
6
$ echo $(( (a * b) % c ))
1
```
在`$(())`中的变量名称, 
可以在其前面加 `$`符号来替换，
也可以不用，如：
`$(( $a + $b * $c ))` 也可以得到19的结果。

此外，`$(())`还可作不同进制(如二进制、八进制、十六进制)的运算，
只是输出结果均为十进制的。
```shell
echo $(( 16#2a )) #输出结果为：42，(16进制的2a)
```
以一个实用的例子来看看吧 :
假如当前的umask是022,那么新建文件的权限即为：
```shell
$ umask 022
$ echo "obase=8; $(( 8#666 & (8#777 ^ 8#$(umask)) ))" | bc
644
```
事实上，单纯用`(())`也可以重定义变量值，或作testing：
```shell
a=5; ((a++)) #可将$a 重定义为6
a=5; ((a--)) #可将$a 重定义为4
a=5; b=7; ((a< b)) #会得到0 (true)返回值。
```
常见的用于`(())`的测试符号有如下这些：

|符号|符号名称|
|----|--------|
| <  | 小于号 |
| >  | 大于号 |
| <= | 小于或等于|
| >= | 大于或等于|
| == | 等于|
| != | 不等于|

> **Note:**

> 使用`(())`作整数测试时，
> 请不要跟`[]`的整数测试搞混乱了。

> 更多的测试，我们将于第10章为大家介绍。

怎样？ 好玩吧... ^_^ 

okay,这次暂时说这么多...

上面的介绍，并没有详列每一种可用的状态，
更多的，就请读者参考手册文件(man)吧...




##shell十三问之9：$@与$*差在哪？
------

要说$@与$*之前，
需得先从shell script的positional parameter谈起...

我们都已经知道变量(variable)是如何定义和替换的，
这个不再多讲了。

#### 1. shell script的positional parameter
--------------

但是，我们还需要知道有些变量是shell内定的，
且其名称是我们不能随意修改的。
其中，就有positional parameter在内。

在shell script中，我们可用$0, $1, $2, $3 ...
这样的变量分别提取命令行中的如下部分:
```shell
script_name parameter1 parameter2 parameter3 ...
```

我们很容易就能猜出, `$0`就是代表 shell script名称(路径)本身，
而`$1`就是其后的第一个参数，如此类推...

须得留意的是`IFS`的作用, 也就是`IFS`被quoting处理后，
那么positional parameter也会改变。

如下例：
```shell
my.sh p1 "p2 p3" p4
```
由于p2与p3之间的空白键被soft quoting所关闭了，
因此，my.sh的中$2是"p2 p3",而$3则是p4...

还记得前两章，我们提到function时，
我们不是说过，它是script中的script吗？^_^

是的，function一样可以读取自己的(有别于script的)
positional parameter, 唯一例外的是$0而已。

举例而言：
假设my.sh里有一个函数(function)叫my_fun,
若在script中跑`my_fun fp1 fp2 fp3`,
那么，function内的$0就是my.sh，而$1是fp1而不是p1了...

不如写个简单的my.sh script 看看吧：
```shell
#!/bin/bash

my_fun() {
    echo '$0 inside function is '$0
	echo '$1 inside function is '$1
	echo '$2 inside function is '$2
}

echo '$0 outside function is '$0
echo '$1 outside function is '$1
echo '$2 outside function is '$2

my_fun fp1 "fp2 fp3" 
```
然后在command line中跑一下 script就知道了：
```shell
chmod 755 my.sh

./my.sh p1 "p2 p3"
$0 outside function is ./my.sh
$1 outside function is p1
$2 outside function is p2 p3
$0 inside function is ./my.sh
$1 inside function is fp1
$2 inside function is fp2 fp3
```

然而，在使用positional parameter的时候，
我们要注意一些陷阱哦：

**$10不是替换第10个参数，
而是替换第一个参数，然后在补一个0于其后;**

也就是说， `my.sh one two three four five six seven eight nine ten`
这样的command line, my.sh里的$10不是ten而是one0 哦...小心小心
要抓到ten的话，有两种方法：

- 方法一：使用我们上一章介绍的${}, 也就是用${10}即可。

- 方法二：就是shift了。

用通俗的说法来说，
**所谓的shift就是取消positional parameter中最左边的参数($0不受影响)**。
其预设值为1，也就是shift 或shift 1 都是取消$1,
而原本的$2则变成$1, $3则变成$2...
那亲爱的读者，你说要shift掉多少个参数，
才可用$1取得到${10} 呢？ ^_^

okay，当我们对positional parameter有了基本的概念之后，
那再让我们看看其他相关变量吧。


#### 2. shell script的positional parameter的number
--------------

先是$#, 它可抓出positional parameter的数量。
以前面的`my.sh p1 "p2 p3"`为例：
由于"p2 p3"之间的`IFS`是在soft quote中，
因此，$#就可得到的值是2.
但如果p2与p3没有置于quoting中话，
那$#就可得到3的值了。
同样的规则，在function中也是一样。

因此，我们常在shell script里用如下方法，
测试script是否有读进参数：
```shell
[ $# = 0 ]
```
假如为0, 那就表示script没有参数，否则就是带有参数...

#### 3. shell script中的$@与$*
-------------

接下来就是**$@与$*:
精确来讲，两者只有在soft quote中才有差异，
否则，都表示“全部参数” ($0除外)**。

若在comamnd line上， 跑`my.sh p1 "p2 p3" p4`的话，
不管$@还是$\*, 都可得到 p1 p2 p3 p4就是了。

但是，**如果置于soft quote中的话：**

- **"$@"则可得到 "p1" "p2 p3" "p4" 这三个不同字段(word);**
- **"$*"则可得到 "p1 p2 p3 p4" 这一整个单一的字段。**

我们修改一下前面的my.sh，使之内容如下：
```shell
#!/bin/bash

my_fun() {
	echo "$#"
}

echo 'the number of parameter in "$@" is ' $(my_fun "$@")
echo 'the number of parameter in "$*" is ' $(my_fun "$*")

```
然后再执行:
```shell
./my.sh p1 "p2 p3" p4
```
就知道，$@与$*差在哪了... ^_^






























