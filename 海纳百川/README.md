（一）基本命令

命令格式： 命令 参数

1.ls 显示文件名，等同于dos下dir命令

命令格式：ls [option] file

option：

-l 显示详细列表

域1 ：文件类型和文件权限

域2 ：文件连接数

域3 ：文件所有者名字

域4 ：文件用户组名字

域5 ：文件长度

域6-8 ：最近修改日期

域9 ：文件名

-a 显示所有文件，包含隐藏文件（以. 起头的文件名）

-R 显示文件及所有子目录

-F 显示文件（后跟*）和目录（后跟/）

-d 与l选项合用，显示目录名而非其内容


2.cd 目录转换，等同于dos下cd命令

注意目录分隔符为“/”，与dos相反

命令格式：cd dirname

3.pwd 显示当前路径

4.cat 显示文件内容,等同于dos下type命令

命令格式：cat filename

5.more 以分页方式查看文件内容.

命令格式：more filename 

6.rm 删除文件

命令格式： rm [-r] filename (filename 可为档名，或档名缩写符号.)

例子 ：

rm file1 删除档名为 file1 之文档.

rm file? 删除档名中有五个字元，前四个字元为file 之所有文档.

rm f* 删除档名中，以 f 为字首之所有文档.

rm -r dir1 删除目录 dir1，及其下所有文档及子目录.

7.mkdir 创建目录

命令格式： mkdir [-p] directory-name

Exmaple ：

mkdir dir1 建立一新目录 dir1.

mkdir -p dir/subdir 直接创建多级目录

8.rmdir 删除目录

目录必须首先为空

命令格式: rmdir directory


9.cp 文档复制

命令格式: cp [-r] source destination

例子:

cp file1 file2 将文档 file1 复制成 file2

cp file1 dir1 将文档 file1 复制到目录 dir1 下，文件名仍为 file1.

cp /tmp/file1 . 将目录 /tmp 下的文档 file1复制到现行目录下，

档名仍为 file1.

cp /tmp/file1 file2 将目录 /tmp 下的文档 file1现行目录下，档名

为file2

cp -r dir1 dir2 (recursive copy) 复制整个目录.

若目录 dir2 存在，则将目录dir1，及其所有文档和子目录，

复制到目录 dir2 下，新目录名称为dir1.若目录dir2不存在，

则将dir1，及其所有文档和子目录，复制为目录 dir2.


　 
10.mv 文件移动 

命令格式： mv source destination

例子:

mv file1 file2 将文档 file1，更改档名为 file2.

mv file1 dir1 将文档 file1，移到目录 dir1 下，档名仍为 file1.

mv dir1 dir2 若目录 dir2 不存在，则将目录 dir1，及其所有档

案和子目录，移到目录 dir2 下，新目录名称为 dir1.

若目录 dir2 不存在，则将dir1，及其所有文档和子

目录，更改为目录 dir2.

11.du 查看目录所占磁碟容量

命令格式: du [-sk] directory

例子 :

du dir1 显示目录 dir1 的总容量及其次目录的容量

du -sk dir1 显示目录 dir1 的总容量,以k bytes为计量

12.find 文件查找 

命令格式: find dir -name filename command

例子:

find . -name hello -print 寻找目前目录及所有的子目录内叫

hello的文档.

find . -ctime +7 -print 找出七天内未被更动的文档

find . -size +2000m -print 找出大小超过2000 bytes的文档

find /tmp -user b1234567 -print 在/tmp下属於b1234567的文档

find . -name '*.c' -exec rm {} 删除所有的.c档

find . -name test\* -print 显示当前目录及其子目录文件名前4

位为test的文件名


13.vi 编辑器

命令状态：

j,k,h,l:上下左右

0： 行首

$: 行尾

i,I :插入命令，i 在当前光标处插入 I 行首插入

a,A:追加命令，a 在当前光标后追加，A 在行末追加

o,O:打开命令，o 在当前行下打开一行，O在当前行上插入一行

r,R :替换命令，r 替换当前光标处字符，R从光标处开始替换

数字s: 替换指定数量字符

x: 删除光标处字符

dd: 删除当前行

d0: 删除光标前半行

d$: 删除光标后半行

ctrl+f :后翻页

ctrl+b:前翻页

G : 文件尾

数字G: 数字所指定行

/string 查找字符串

n 继续查找

N 反向继续查找

% 查找对应括号

u 取消上次操作

ex命令状态 

：set number 显示行号

：set smd 显示显示状态

：0 文件首

：1,5 copy 7 块拷贝

：1，5 del 块删除

：1，5 move 7 块移动

：1，$s/string1/string2/g 全文件查找string1并替换为string2

：wq! 存盘退出


（二） 增强命令

1. ln 文档连结

命令格式：ln -s oldname newname ( Hard link )

同一文档，可拥有一个以上之名称，可将文档做数个连结.

例子 ：

ln -s file1 file2 　　将名称 file2，连结至文档 file1.

2.grep 搜索字符串

命令格式: 

grep string filename


寻找字串的方法很多，比如说我想找所有以M开头的行.此时必须引进pattern的观

念.以下是一些简单的□例，以及说明：


^M 以M开头的行，^表示开始的意思

M$ 以M结尾的行，$表示结束的意思

^[0-9] 以数字开始的行，[]内可列举字母

^[124ab] 以1,2,4,a,或b开头的行

^b.503 句点表示任一字母

* 星号表示0个以上的字母(可以没有)

+ 加号表示1个以上的字母

\. 斜线可以去掉特殊意义


<eg> cat passwd | grep ^b 列出大学部有申请帐号者名单

cat passwd | grep ^s 列出交换学生申请帐号者名单

cat passwd | grep '^b.503' 列出电机系各年级...

grep '^\.' myfile.txt 列出所有以句点开头的行

3.fgrep 搜索字符串

命令格式：fgrep string file

4.file 显示文件类型

命令格式：file fileall

文件类型为shell script,ELF 32bit,ASCII text,data or tar file

5.diff 比较文档或目录之不同内容

命令格式：diff [-r] name1 name2 ( name1 name2 可同时为档名，或目录名称.)

例子 :

%diff file1 file2

比较文档 file1 与 file2 内，各行之不同处.

%diff -r dir1 dir2

比较目录 dir1 与 dir2 内，各文档之不同处.

6.cmp 比较文档相同部分

命令格式：cmp file1 file2


7.ftp 远程文件传输

命令格式： ftp [hostname|IP address]

在进入 ftp 之後，如果与 remote host 连接上了，它将会询问你 username 

与密码，如果输入对了就可以开始进行文档传输.

注意：如用户无密码，无法注册

(1) ftp 命令 

ascii 将传输模式设为 ascii 模式.通常用於传送文字档.

binary 将传输模式设为 binary 模式，通常用於传送执行档，压缩档与影像

档等.

cd remote-directory 将远程主机上的工作目录改变.

lcd [ directory ] 更改本地主机的工作目录.

ls [ remote-directory ] [ local-file ] 列出远程主机上的文档.

get remote-file [ local-file ] 取得远方的文档.

mget remote-files 可使用通用字元一次取得多个文档.

put local-file [ remote-file] 将本地主机的文档送到远程主机.

mput local-files 可使用通用字元一次将多个文档放到远程主机上.

help [ command ] 线上辅助指令.

mkdir directory-name 在远程主机创建一个目录.

prompt 更改交谈模式，若为 on 则在 mput 与 mget 时每作一个文档之传

输时均会询问.

quit/bye 离开ftp .

(2) 后台执行ftp

1.首先，将过程所用到的指令依顺序放入文档中，如下：

%cat ftp_command

!mkdir test

lcd test

cd test

prompt

binary

mget *.*

bye

2.其次，建一个.netrc档，属性为400，让ftp 自动到此读取Username

与Password，方可顺利login 到的主机，如下：

%cat .netrc

machine remote login anonymous password guest 

3.最後再执行下面指令即可.

%nohup ftp remote < ftp_command > message &


8.telnet 远程终端访问

命令格式：

telnet [hostname|IP address]


9.IO 重新导向

UNIX所有的程式执行时，均需要资料的输入以及输出资料.一般而言，资料是

从键盘输入，并将资料输出到萤幕上，这就叫做标准输入及标准输出，而我们

可以更改标准出输出入.

A. 更改标准输入 —— 在命令後方加"< <filename>" ，即可从<filename>这

个文档输入资料.

B. 更改标准输出 —— 在命令後方加"> <filename>" ，即可将萤幕输出的资

料导向到<filename>这个文档上.

C. 更改标准输出 —— 在命令後方加">> <filename>"，功能与B.相似，只不

过这会将资料加在文档後方.

D. 管道 —— 在两个命令中间加上'|'，即可将前方指令的输出当成後方指令


D. 管道 —— 在两个命令中间加上'|'，即可将前方指令的输出当成後方指令

的输入.


例:

cd /tmp

ls -l > /tmp/ls.out

cat /tmp/ls.out

more /tmp/ls.out

rm /tmp/ls.out

ls | more

ls | wc -l (word count, count line number,算出文档数目)

Aix系统培训
一、 Aix简介

Aix是Unix操作系统的版本之一，主要应用在IBM RISC 6000系列小型机上。其他的Unix ，如SUN的Solaris ,HP Unix以及Linux等等。目前Aix系统的版本有Aix 4.x Aix5.x等。它们之间只是版本不同，但内核都是一样的，操作命令机本上也相同。版本高的会新增一些命令，都是向下兼容的。

二、 Aix使用入门

对于大多数用户来说，对于Aix的访问都是通过telnet的方法来登录到RS6000上，当然是以不同的身份了。把本地机器作为RS6000的一个终端，来完成对R S6000的操作。这实际上是Unix的一大特点，Unix本身就是设计成一个多任务、多用户的并发系统。
2.1登录 （login）
2.1.1 RS6000的登录

许多用户可以同时使用Unix系统，为了让系统知道用户是谁以及可以使用什么资源，用户在使用之前必须向系统表明自己的身份。另外，U nix认为用户是通过终端或在PC上运行方针软件与其通讯。Unix系统和用户终端之间必须建立传输信息的连接。通常把建立通讯连接和表明用户身份的过程称为登录( login) .
2.1.1.1 RS6000的开机

直接按Power键。RS6000启动、自检。包括对硬件的自检，系统的初始化。
2.1.1.2登录

默认的情况下，系统会进入Xwindows环境下，也就是所谓的CDE环境（Common Desktop Environment）即公共桌面环境。这样Aix启动后将直接显示CDE注册界面，输入用户名和口令后即进入CDE操作环境。通常在RISC6000端都是以r oot身份（管理员身份）登录的。 
2.1.2客户端的登录

客户端主要是通过执行telnet命令，并且输入合法的用户名和密码，登录到服务器端。比如：在客户端执行telnet 192.168.0.161 (假设为RS6000的IP) ，会出现如下的登录提示：
AIX Version 4
(C) Copyrights by IBM and by others 1982, 1996.
login:
然后输入用户名，如：long ，回车后，会出现输入密码的提示：
long's Password: 
2.1.3成功登录之后

成功登录之后，系统将显示一些消息，如上一次用户登录的时间、有关系统信息的消息（称为当日消息），以及一些通知用户是否有mail 的消息。其中，当热消息十分重要，它是系统管理员与用户通讯的方法之一，比如系统管理员通过当日消息通知用户下一次系统关闭的时间。如图：
AIX Version 4
(C) Copyrights by IBM and by others 1982, 1996.
login: long
long's Password:
********************************************************************** 
* 
* Welcome to AIX Version 4.3! 
* 
* 
* Please see the README file in /usr/lpp/bos for information pertinent to 
* this release of the AIX Operating System. 
* 
* 
*********************************************************************
Last unsuccessful login: Fri Dec 28 14:59:34 BEIST 2001 on /dev/pts/0 from 192.3
Last login: Sat Dec 29 10:13:50 BEIST 2001 on /dev/pts/6 from 192.168.0.133

[YOU HAVE NEW MAIL]
$
显示以上消息后，系统将显示一个命令提示符。这表明系统将等待用户的输入，非root用户登录后提示符一般为$ ，root用户登录后，提示符为# .出现不同命令提示符与shell有关，B shell和K shell的提示符使用$ ，Aix中为K shell .
2.1.4用户的权限

如果Unix系统只有用户自己使用的话，除了系统提供的系统管理员帐号以外，用户还要给自己建立一个帐号。因为使用系统管理员的帐号( root)要十分小心，该帐号具有特殊的权限。Unix有内嵌的安全机制，一般的用户没有权限创建新的帐户或进行其他系统管理的操作。roo t用户使用系统管理员帐号，又称为超级用户，具有系统管理员的权限。但root 拥护不小心的错误可能会导致系统故障。因此用户在作一般的工作时，应进入自己的帐号，由于没有权限，故不会给系统带来故障。在多用户环境中，超级用户的工作应更加小心。因为他的错误不但会影响他本人，还会影响他人以及整个系统。
Unix还保证在多用户环境中，不同用户之间不相互干扰，每个用户有自己的工作权限，并且可以选择组内或其他用户对自己工作的访问权限。如果把访问权限制为o wner ，那么自己的数据别人是无法访问的。如果用户参加一个组，共同进行一项任务，那么该用户可以把访问权限定为owner和组内的其他成员。如果用户的数据允许其他任何用户看的话，可以把权限定义为任何人。有关定义权限的部分，我们将在介绍c homd命令时讨论。
2.2 logout

工作结束后，用户需要退出系统，这就防止别人有意无意的通过该用户的帐号访问他的文件或者使用系统。不论在什么shell中，退出系统的方法是使用e xit命令，该命令使shell退出。当用户从login的shell退出时，就会自动退出系统。也可敲入logout命令。$logout ，或按ctrl + d .如果是图形界面的话，退出系统方法请参见在线帮助。
2.3使用基本命令

telnet命令
telnet命令用于使客户端登录到RS6000上的Aix 系统。语法为：
telnet + ip (服务器的ip地址)。例如：登录到ip 地址为192.168.0.161的服务器，在Windows 的命令提示符下敲入telnet 192.168.0.161 ，当然，如果使用其它的telnet软件，只需设好ip ，然后执行登录就可以了。

ls命令
ls命令用于显示指定路径下的文件。具体格式为：
ls + 路径。比如，要显示/home下的文件，用 $ls /home .这种方式只显示文件名，而不包含其他的信息。
$ls –l + 路径 ，除了显示文件名之外，还显示文件的属性、创建时间，以及所属的组 。
$ls –a +路径，显示隐含文件。

man命令
是Aix的帮助命令，可以显示某个命令的详细使用说明。这个命令比较有用处，具体语法为：$man + 命令 ，比如，要查看tar命令的使用方法，可用：
$man tar

cd命令
改变当前的工作目录，类似于Dos方式下的cd命令。具体语法为：
$cd +路径 比如，进入/home/user目录中，用$cd /home/user命令。
cd..是退回到上一级目录。cd是回到上一步所在的目录。

who命令
who命令用于显示当前在线的用户，使用非常简单，直接在shell提示符下输入who就可以了。$who

cp命令
用于拷贝文件，类似于Dos下的copy命令。具体语法为：
cp + 文件名 + 路径 如果要复制目录，需要加参数-r或-R
例如：$cp /home/a.tar /home/demo
$cp –r /home/aaa /home/bbb 其中aaa 、bbb均为目录。

mv命令
用于移动文件或文件夹。具体语法为：
mv + 文件名 + 路径
例如，将 /home/long/a移动到/home/long/b目录中，用下面命令：
$mv /home/long/a /home/long/b

rm命令
用于删除文件或文件夹。具体语法为：rm + 路径 +文件名 。例如，要删除/home/long/manual.tar文件，用下面命令：$rm /home/long/manual.tar
如果要删除目录，用rm –r +路径+目录名 。例如：要删除/home/manual目录，用$rm –r /home/manual
注意：要删除一个文件或文件夹，首先要具有对这个文件夹的写权限。

mkdir命令
用于建目录具体语法为mkdir + 目录名。例如，要在/home/long目录下建立一个名为test的目录，用下面的命令：
$mkdir /home/long/test

rmdir命令
与mkdir的用途相反，用于删除一个目录。（注意，这里指的是空目录，里面没有文件）具体语法为：rmdir + 目录名 例如，将刚才建的目录删掉，就可以用下面的命令：$rmdir /home/long/test

vi命令
vi命令是unix下常用而重要命令，可在全屏幕方式下编辑一个或多个文件。若在vi执行时没有指定一个文件，那么vi命令会自动产生一个无名的空的工作文件。若指定的文件不存在，那么就按指定的文件名创建一个新的文件。若对文件的修改不保存的话，v i命令并不改变原来文件的内容。
注意：vi命令并不锁住所编辑的文件，因此多个用户可能在同时编辑一个文件，那么最后保存的文件版本将被保留。
下面是vi命令使用的一些选项及含义：
-c sub-command 在对指定的文件编辑前，先执行指定的命令 sub-command .
-r filename 恢复指定的文件filename .
-R 将指定的文件以只读的方式放入编辑器中，这样不会保存对文件的任何修 改。
-y number 将编辑窗口的大小设为number行。
下面是vi编辑所处的三种模式：
.命令模式 进入vi时所处的模式。在此模式下用户可输入各种子命令对进行操作，如删除行、粘贴行、移向下一个字、移向不同行等。
.文本输入模式 在此模式下可以修改一行的内容并增添新行。在命令模式下键入a 、i 或c键可进入文本输入模式，按Escape键可返回命令模式。
.命令项模式 在此模式下，可以通过子命令输入更多的参数。如：w子命令要求输入一文件名，“/”子命令要求输入一个查找项。用户使用Escape键返回命令模式。
下面是自命令模式下执行的，在同一行上移动的自命令：
h 将光标左移一格。
l 将光标右移一格。
j 将光标下移一格。
k 将光标上移一格。
w 将光标移到下一个小字的前面。
W 将光标移到下一个大字的前面。
b 将光标移到前一个小字的前面。
B 将光标移到前一个大字的前面。
e 将光标移到下一个小字的后面。
E 将光标移到前一个大字的后面。
fc 把光标移到同一行的下一个c字符处。
Fc 把光标移到同一行的前一个c字符处。
tc 把光标移到同一行的下一个字符c的前一格。
Tc 把光标移到同一行的前一个字符c的后一格。
number| 把光标移到递number列上。
下面是命令模式下在行间移动的子命令：
+或Enter 把光标移至下一行第一个非空白字符。
- 把光标移至上一行第一个非空白字符。
0 把光标移到当前行的第一个字符处。
$ 把光标移到当前行的最后一个字符处。
H 把光标移到屏幕最顶端一行。
L 把光标移到屏幕最底端一行。
M 把光标移到屏幕中间。
下面是命令模式下改变屏幕显示的子命令：
z- 把当前行作为屏幕的最后一行，并重新显示屏幕。
z. 把当前行作为屏幕的中间一行，并重新显示屏幕。
Ctrl+l 重新显示屏幕当前内容。
/pattern/z- 寻找pattern的下一个位置，并把所在行设为屏幕的最后一行。
下面是在命令模式下用来显示页面的子命令：
Ctrl + f向后滚一页。
Ctrl + d向后滚半页。 
Ctrl + b向前滚一页。
Ctrl + u向前滚半页。
Ctrl + e屏幕向下滚一行。
Ctrl + y屏幕项上滚一行。
下面是在命令模式下用来查找字符串所使用的子命令：
/pattern 向后寻找指定的pattern ,若遇到文件尾，则从头再开始。
？pattern 向前寻找指定的pattern ,若遇到文件头，则从尾再开始。
n 在上次指定的方向上，再次执行上次定义的查找。
N 在上次指定的方向的相反方向上，再次执行上次定义的查找。
/pattern/+number 将光标停在包含pattern的行后面第number行上。
/pattern/-number 将光标停在包含pattern的行前面第number行上。
% 移到匹配的“（）”或“{}”上。
下面是在文本输入模式下用来输入文本的子命令（用户可在任何时候按Escape返回到命令模式）：
a 在光标之后开始输入文本。
A在行尾开始输入文本。
i在光标之前开始输入文本。
I在行首第一个非空白字符前输入文本。
o在光标所在行后插入一空行。
O在光标所在行前插入一空行。
下面是在命令模式下改变文本所使用的子命令（用户可在任何的时候按Escape键返回到命令模式）：
cc或S 修改一整行。
C 改变一行光标位置以后的部分。
cw 改变光标所在单词。
dd删除当前行。
D 删除光标所在行光标后面的内容。
dw删除光标所在的单词。
J 把下一行内容加到本行行尾。
rc把光符所在字符替换成c .
R 覆盖本行内容。
u恢复上一次的修改。
x删除光标所在的字符。
~ 改变光标所在出字符的大小写。
. 重复上一个操作。
<<把当前行移到左边。
>>把当前行移到右边。
下面是用于文件中拷贝文本的字命令：
p 将缓冲区内容取到光标所在行的下面一行。
P 将缓冲区内容取到光标所在行的上面一行。
“bd 将文本删除至有名缓冲区b .
“bp 张贴有名缓冲区b中内容。
yy把当前行放入缓冲区。
Y 把当前行放入缓冲区。
Yw把光标所在的单词放入缓冲区。
下面是用于保存文件的子命令：
:w 回写修改后的文件。
:w filename 当filename不存在时，把修改后的文件存为文件filename ,当文件filename存在时，报错。
!w filename 如果文件filename存在时，把修改后的文件保存为文件filename .
下面列出了在vi编辑的多个文件之间切换所用的子命令：
:n开始编辑vi激活的文件列表中的下一个文件。
:n filenames 指定将被编辑的新的文件列表。
下面列出了用于在当前文件和另外一个文件间切换的子命令：
:e filename 使用filename激活vi （在vi中装入另一个文件filename）。
e!重新装入当前文件，若当前文件有改动，则丢弃以前的改动。
:e+filename 使用filename激活vi ,并从文件尾部开始编辑。
:e+number filename 使用filename激活vi ,并在第number行开始编辑。
:e# 开始编辑另外一个文件。
下面是在本文件中加入其他文件代码所使用的子命令：
:r filename读取filename文件，并将其内容加到当前文件后。
:r ! command执行command文件，并将其输出加到当前文件后。
下面是vi中其他的子命令：
ctrl+g 取得正在编辑文件的有关信息。
:sh启动sh ，从sh中返回可用exit或ctrl+d .
:! Command 执行命令command .
!!重新执行上次的:! Command子命令。
:q退出vi ，若用户对编辑的文件有所修改，系统不会让用户使用q命令退出。
:q!退出vi而不管是否对文件有改动。
ZZ或:wq 保存对文件的修改并退出vi .
用户可在一个特殊的文件.exrc中定义特殊的vi命令。在vi中使用这些命令时，必须在该命令前加上一个冒号( ：) 。

cat命令
用于在标准输出上显示文件的内容，但不会更新文件。具体语法是：cat +文件名 。例如，显示/home/long/example.txt文件的内容，可以用下面的命令：
$cat /home/long/example.txt

more命令
功能是在终端屏幕按屏显示文本文件。具体语法为：more + 文件名。例如：要分屏显示文件 /example.C ，可以用下面命令：$more /example.C

shutdown命令
shutdown命令中断操作系统，只有具有root权限的用户才能执行这个命令。在默认的情况下，执行shutdown命令时会收到一个消息，直到收到完整的s hutdown信息时，系统才会完成shutdown操作。当shutdown时间不断接近的时候，在用户端上会受到警告信息，在指定时间到达时，（默认为6 0秒）系统结束所有进程，unmount掉所有文件系统。具体的用法是：
#shutdown 关闭系统
#shutdown –Fr 快速关闭系统并且重新启动
#shutdown –F 快速关闭系统

talk命令
talk命令用于与其他终端用户交谈，具体语法如下：
$talk username1@rs6k 其中user是你想要谈话对象的帐户名称。同时，在对方终端上也要运行$talk username2@rs6k (其中rs6k是服务器的名称)
wall命令
用于向网络中的所有用户发送广播。具体语法为：wall + 内容
如，下面命令： $wall good morning !
在所有的终端上就会显示good morning !

date命令
用于显示或改变时间。如果改变时间，必须以root身份登录。例如，要显示时间可输入：$date
要改变时间输入：#date 0217142592 改变时间为 1992年2月17日14：25

clear命令
消除终端屏幕。具体的语法非常简单，只要执行$clear 就可以了。

whereis命令
用于确定文件的位置。具体的用法：whereis +文件名 。例如，定文件example.txt的位置，用下面的命令：$whereis example.txt

tar命令
tar是unix中的一个常用的命令，也是一个很重要的命令。tar命令将磁盘上的文件拷贝到档案媒体上（比如：软盘到硬盘，也可以是硬盘到软盘），或把档案媒体上记录的数据恢复成磁盘上的文件。遇到错误时，t ar命令不提供任何恢复操作。
tar的命令格式：
#tar -cvf (或xvf)+文件名+设备
c->是本地到其他设备
x->是其他设备到本地
r->是追加（比如拷了一个t.txt ,还要再拷一个r.txt
其中tar一个文件（或文件夹）用cvf参数；
将tar好的文件解开，用xvf参数。
例如：
将 /home/test中的文件tar到/anw中，并tar成exe.tar用下面的命令：
#tar -cvf /anw/exe.tar /home/test
将tar好的文件解开，用下面的命令：
#tar -xvf /anw/exe.tar 会将exe.tar解到压缩时指定的路径下。
tar命令生成文件的扩展名可以任意起，比如：test文件的生成文件可以是test.a 、test.b 、test.tar都行。

compress命令和uncompress 命令
这两个命令用于完成压缩和解压缩。比如，运行$compress test.tar ，是把test.tar文件压缩成test.tar.Z文件。解压缩时用uncompress命令：$uncompress test.tar.Z 
注意，如果压缩时输入的是绝对路径，解压缩时就自动解到相应的路径下。

查看磁盘空间命令df
df命令显示系统总共的磁盘空间和可用的磁盘空间。同时还会显示指定设备的文件系统参数、文件系统存在的路径，以及文件系统mount到哪个目录中。具体语法如下：$ df 显示的单位是512k ，$df –k命令显示的磁盘空间的单位是1024k ，也就是1 M .如下所示：
$ df
Filesystem 512-blocks Free %Used Iused %Iused Mounted on
/dev/hd4 327680 239248 27% 2616 4% /
/dev/hd2 11730944 5539296 53% 84989 6% /usr
/dev/hd9var 327680 301088 9% 709 2% /var
/dev/hd3 327680 316448 4% 68 1% /tmp
/dev/hd1 10092544 1426480 86% 9128 1% /home

$ df -k
Filesystem 1024-blocks Free %Used Iused %Iused Mounted on
/dev/hd4 163840 119624 27% 2616 4% /
/dev/hd2 5865472 2769648 53% 84989 6% /usr
/dev/hd9var 163840 150544 9% 709 2% /var
/dev/hd3 163840 158224 4% 68 1% /tmp
/dev/hd1 5046272 713236 86% 9128 1% /home

finger命令
用于显示当前登录用户信息。这个命令与f命令的功能相同。具体语法如下：
$finger 或$f

last命令
last命令用于显示前面登录的信息。例如，last会显示出最近一段时间内所有用户的登录信息。包括登录时间和进程。如果指定显示某一用户的登录信息，可以用l ast +用户帐号 。比如：$last user 就是显示user的登录信息。

ifconfig命令
用于配置或显示TCP/IP网络接口的参数。可以用下面命令来查看IP地址：
$ifconfig –a 显示RS6000上的IP地址。

ping命令
用来测试网络的物理连接。用户发一个ICMP（Internet Control Message Protocal）来从主机或网关获得一个ICMP回应。如测试与IP地址为192.168.0.161的主机的物理连接，可以用下面的命令：
$ ping 192.168.0.161
PING 192.168.0.161: (192.168.0.161): 56 data bytes
64 bytes from 192.168.0.161: icmp_seq=0 ttl=255 time=0 ms
64 bytes from 192.168.0.161: icmp_seq=1 ttl=255 time=0 ms
64 bytes from 192.168.0.161: icmp_seq=2 ttl=255 time=0 ms
64 bytes from 192.168.0.161: icmp_seq=3 ttl=255 time=0 ms
64 bytes from 192.168.0.161: icmp_seq=4 ttl=255 time=0 ms
64 bytes from 192.168.0.161: icmp_seq=5 ttl=255 time=0 ms
64 bytes from 192.168.0.161: icmp_seq=6 ttl=255 time=0 ms
----192.168.0.161 PING Statistics----
8 packets transmitted, 8 packets received, 0% packet loss
round-trip min/avg/max = 0/0/0 ms
这种情况说明收到了从192.168.0.161发回的相应，证明与主机的物理连接正常。
若出现下面的情况，说明没有与192.168.0.161连接。
$ ping 192.168.0.161
PING 192.168.0.161: (192.168.0.161): 56 data bytes

----123.123.1.1 PING Statistics----
53 packets transmitted, 0 packets received, 100% packet loss

su命令
用来改变用户的ID ，连同会话。例如，当前用户为user1 ，在执行$su – user2后，用户ID就变为了user2 ，并且可以以user2 身份来完成操作。在建立用户的时候，可以选择是否允许其他的用户su你的用户。

ps命令
用于显示当前正在运行的进程状态。使用不同的选项，用户可以查询所有正在运行的进程或是具有该用户ID的所有进程。通过ps命令，用户可以了解正在后台运行的进程或是具有该用户I D的所有进程。通过ps命令，用户可以了解正在后台运行的进程和正在运行的批处理进程。当不带参数时，只显示由当前终端创建的正在执行的进程。$ ps –A列出系统中正在运行的进程的详细信息。$ps a列出与终端有关的所有进程。

echo命令
用于将字符串标准输出。比如，输出Hello ，可以用$echo Hello
在变量赋值语句中，如U=Hello ，要输出Hello，用$echo $U，注意前面要加一个$符号，表示输出变量U 所对应的值。

env命令
env命令可用来显示当前的环境或设置一个或多个环境变量。还可以运行指定的命令。而环境的变化只在命令运行期间有效。如$env ，可以查看当前设定的环境变量。如：
$ env
_=/usr/bin/env
LANG=en_US
LOGIN=long
NLSPATH=/usr/lib/nls/msg/%L/%N:/usr/lib/nls/msg/%L/%N.cat
IMQCONFIGCL=/etc/IMNSearch/dbcshelp
PATH=/usr/bin:/etc:/usr/sbin:/usr/ucb:/home/long/bin:/usr/bin/X11:/sbin:.
LC__FASTMSG=true
CGI_DIRECTORY=/var/docsearch/cgi-bin
IMQCONFIGSRV=/etc/IMNSearch
LOGNAME=long
MAIL=/usr/spool/mail/long
LOCPATH=/usr/lib/nls/loc
USER=long
DOCUMENT_SERVER_MACHINE_NAME=localhost
AUTHSTATE=compat
SHELL=/usr/bin/ksh
ODMDIR=/etc/objrepos
U=rs6k
DOCUMENT_SERVER_PORT=49213
HOME=/home/long
TERM=vt100
MAILMSG=[YOU HAVE NEW MAIL]
PWD=/home/long
DOCUMENT_DIRECTORY=/usr/docsearch/html
TZ=BEIST-8BEIDT
A__z=! LOGNAME

passwd命令
用户登录时，系统要求用户输入用户名和口令。尽管用户可以不设口令，但大多数用户都在登录时使用口令。如果用户要修改口令，可以使用p asswd命令。执行passwd命令时，系统首先要求用户输入当前的口令，然后再连续输入两次新口令。当两次输入新口令一致时，新口令才有效。例如：改变l ong的password ，用下面命令：
$ passwd long
Changing password for "long"
long's Old password:
long's New password:
Enter the new password again:
$

chmod命令
为了增强目录或文件的安全性，或将目录和文件放给他人访问，用户常常要修改目录或文件的权限。Unix中，权限使用8进制数字(0到7)来指定文件或目录的权限。对目录或文件的使用对象有三种：
所有者：创建文件的用户
组成员：所有者所在的用户组内其他成员
其他用户： 既不是所有者，又不是组成员的其他用户。
系统使用一位8进制数字指定每一类访问者对象所具有的权限。对于所有者、组成员和其他用户，对文件的权限可分为读权限、写权限和执行权限。每一种权限对应一比特数据。即若读比特为1 表明可读，写比特为1表明可写，执行比特为1表示可执行。
注意，对于目录来说，执行比特的功能叫特殊。用户对目录可执行就是可访问该目录中的内容，否则不可以访问该目录内容。
读、写，执行三比特组合在一起就可表示一个8进制行数字，其中读比特权限为4，写为2，执行为1 。
再次提醒用户注意，目录的执行权限时对目录的访问权限，即对该目录操作的权限。如果对目录有写的权限，那么就可以任意的读、写、删除和执行目录中的所有文件，而不受具体文件权限的限制。
使用chmod命令，可以给文件或目录赋上新的权限值。例如，若设置文件testfile的权限为：所有者可读、写、执行，组成员可读，其他用户可执行。可用命令：$ chmod 741 testfile
当用户想把当前目录中文件testfile的权限设置为所有者和组成员可读、可写时，可执行：$chmod 660 testfile
$ ls -l
total 202
-rw------- 1 long system 287 Dec 28 14:56 mbox
drwxr-xr-x 3 long system 512 Dec 29 15:05 mm
-rw-r--r-- 1 long system 59034 Dec 29 14:32 smit.log
-rw-r--r-- 1 long system 12668 Dec 21 16:19 smit.script
drwxr-xr-x 3 long system 512 Dec 27 16:21 test
-rw-r--r-- 1 long system 10240 Dec 28 12:40 test.tar
-rw-r--r-- 1 long system 16949 Dec 10 13:24 websm.log
注意左边第一列，第一个字符为 – 表示文件；为d表示目录。后面9个字符，每三个为一组（8进制），分别表示用户、用户所在的组，以及其他用户的读、写可执行权限。其中r 表示可读，w表示可写，x表示可执行。

2.4 配置环境变量
为了使用户方便和灵活地使用shell，Unix引入了环境的概念。环境是一些数据，用户可以改变这些数据，增加新的数据或删除一些数据。这些数据称为环境变量。因为它们定义了用户的工作环境，同时又可以被修改。
2.4.1浏览和设置环境变量
每个用户都有自己不同的环境变量，用户可以用env命令（不带参数）浏览环境变量。输出的格式和变量名随着shell的不同和系统配置的不同而不同。例如：
$ env
_=/usr/bin/env
LANG=en_US
LOGIN=long
NLSPATH=/usr/lib/nls/msg/%L/%N:/usr/lib/nls/msg/%L/%N.cat
IMQCONFIGCL=/etc/IMNSearch/dbcshelp
PATH=/usr/bin:/etc:/usr/sbin:/usr/ucb:/home/long/bin:/usr/bin/X11:/sbin:.
LC__FASTMSG=true
CGI_DIRECTORY=/var/docsearch/cgi-bin
IMQCONFIGSRV=/etc/IMNSearch
LOGNAME=long
MAIL=/usr/spool/mail/long
LOCPATH=/usr/lib/nls/loc
USER=long
DOCUMENT_SERVER_MACHINE_NAME=localhost
AUTHSTATE=compat
SHELL=/usr/bin/ksh
ODMDIR=/etc/objrepos
U=rs6k
DOCUMENT_SERVER_PORT=49213
HOME=/home/long
TERM=vt100
MAILMSG=[YOU HAVE NEW MAIL]
PWD=/home/long
DOCUMENT_DIRECTORY=/usr/docsearch/html
TZ=BEIST-8BEIDT
A__z=! LOGNAME
$
有时用户的环境变量很多，如果只想查询其中一个的话，可以用echo命令。前面介绍过，为了表明输入的字符代表环境变量，必须在其前面加上$ .注意$和后面变量名之间不要有空格，例如用echo命令显示环境变量HOME的值：
$ echo $HOME
/home/long
$
用户可以通过给环境变量赋值，产生新的环境变量，可以通过给已存在的环境变量赋值的方法改变其原有的值。

2.5公共桌面环境CDE
CDE全称Common Desktop Environment ，即“公共桌面环境”，是一个图形界面管理程序。使用CDE的前提条件是RS6000连有图形显示器。使用之前，先要安装软件包X.11、Dt. rte和X11.Dt.helpinfo .安装过程中，系统将CDE设置为默认的用户界面，这样Aix启动后将直接显示CDE注册界面，输入用户注册名和口令后即进入CDE操作环境。

2.6 Unix的内核和shell
Unix内核：它是系统的心脏，是运行和管理象磁盘和打印机等硬件设备的核心程序，位于操作系统的最底层。
Shell是系统用户界面，提供了用户与内和进行交互操作的一种接口。Shell有自己的编程语言，用于对命令的编辑。它允许用户编写由sh ell命令组成的程序。目前，shell有以下版本：BASH (Linux) , K shell (Aix) ,C shell

2.7 Unix文件系统的层次结构
Unix以文件系统的方式来管理资源。文件系统分为动态文件系统和静态文件系统。动态文件系统：可执行文件、配置文件
静态文件系统：操作系统
下面是Unix文件系统的基本层次结构：
/usr /lib /sbin 主要用于存贮系统库文件和一些备份文件
/etc 主要是系统配置文件
/dev 设备文件
/var 日志文件
/tmp 临时文件
/opt 应用文件