+++
title = 'Bash'
date = 2024-10-06T15:17:36+02:00
draft = false
+++

## 查看Shell

```bash
$ cat /etc/shells
```

```bash
bash --version

echo $BASH_VERSION
```

## CLI

```bash
bash

exit

pwd

cat /etc/shells
```

## 基础语法

### echo

echo命令的作用是在屏幕输出一行文本，可以将该命令的参数原样输出。

```bash
$ echo hello world
hello world
```

如果想要输出的是多行文本，即包括换行符。这时就需要把多行文本放在引号里面。

```bash
$ echo "<HTML>
    <HEAD>
          <TITLE>Page Title</TITLE>
    </HEAD>
    <BODY>
          Page body.
    </BODY>
</HTML>"

```

#### -n 参数

默认情况下，echo输出的文本末尾会有一个回车符。-n参数可以取消末尾的回车符，使得下一个提示符紧跟在输出内容的后面。

```
$ echo -n hello world
hello world$
```
上面例子中，world后面直接就是下一行的提示符$。
```
$ echo a;echo b
a
b

$ echo -n a;echo b
ab
```

#### -e参数

-e参数会解释引号（双引号和单引号）里面的特殊字符（比如换行符\n）。如果不使用-e参数，即默认情况下，引号会让特殊字符变成普通字符，echo不解释它们，原样输出。
```
$ echo "Hello\nWorld"
Hello\nWorld

# 双引号的情况
$ echo -e "Hello\nWorld"
Hello
World

# 单引号的情况
$ echo -e 'Hello\nWorld'
Hello
World
```

### 命令格式

命令行环境中，主要通过使用 Shell 命令，进行各种操作。Shell 命令基本都是下面的格式。
```
$ command [ arg1 ... [ argN ]]
```

#### 短形式与长形式命令配置

在命令行工具中，许多配置项通常有两种形式：**短形式**和**长形式**。短形式一般以一个连字符 `-` 开头，而长形式则以两个连字符 `--` 开头，且两者的功能完全相同。

- **短形式**：`-l`（适合手动输入，简洁）
- **长形式**：`--list`（适合脚本编写，可读性更高，便于理解）

例如，以下两个命令功能一致：

```bash
# 短形式
$ ls -r

# 长形式
$ ls --reverse
```

在上例中，`-r` 是短形式，`--reverse` 是长形式。虽然它们执行相同的功能，但短形式更便于手动输入，而长形式则在脚本中更具可读性。

##### 多行命令的书写

通常，Bash 命令写在一行中，按下回车键后立即执行。但如果命令过长，分行书写有助于提高可读性。此时可以在每行末尾使用反斜杠 `\`，将下一行与当前行合并解释。例如：

```bash
$ echo foo bar

# 等同于
$ echo foo \
bar
```

### Bash 使用空格（或 Tab 键）区分不同的参数。

```bash
$ command foo bar
```

在上面的命令中，`foo` 和 `bar` 之间有一个空格，所以 Bash 认为它们是两个参数。

如果参数之间有多个空格，Bash 会自动忽略多余的空格。例如：

```bash
$ echo this is a     test
this is a test
```

在这个例子中，尽管 `a` 和 `test` 之间有多个空格，Bash 依然只会处理为一个空格，输出时没有多余空格。


### 分号

分号（`;`）是命令的结束符，可以让一行中包含多个命令。前一个命令执行结束后，接着执行下一个命令。

```bash
$ clear; ls
```

在此例中，Bash 会先执行 `clear` 命令，执行完成后再执行 `ls` 命令。

需要注意的是，使用分号时，无论第一个命令是否成功执行，第二个命令都会继续执行。

### 命令的组合符&&和||

除了分号，Bash 还提供了两个命令组合符：`&&` 和 `||`，可以更灵活地控制多个命令之间的执行顺序。

- `Command1 && Command2`  
  表示如果 `Command1` 成功执行，则继续执行 `Command2`。

- `Command1 || Command2`  
  表示如果 `Command1` 执行失败，则继续执行 `Command2`。

### 示例

```bash
$ cat filelist.txt; ls -l filelist.txt
```
在这个例子中，无论 `cat` 命令执行成功还是失败，都会继续执行 `ls` 命令。

```bash
$ cat filelist.txt && ls -l filelist.txt
```
在这个例子中，只有 `cat` 命令成功执行后，才会继续执行 `ls` 命令。如果 `cat` 执行失败（如文件不存在），`ls` 命令将不会执行。

```bash
$ mkdir foo || mkdir bar
```
在这个例子中，只有当 `mkdir foo` 命令执行失败时（如 `foo` 目录已存在），才会执行 `mkdir bar` 命令。如果 `mkdir foo` 成功，`bar` 目录将不会被创建。

### type 命令

`type` 命令用于判断 Bash 中一个命令的来源，是内置命令还是外部程序。

### 示例

```bash
$ type echo
echo is a shell builtin

$ type ls
ls is hashed (/bin/ls)
```

上面的例子中，`type` 命令告诉我们 `echo` 是一个内置命令，而 `ls` 是外部程序（位于 `/bin/ls`）。

`type` 命令本身也是内置命令：

```bash
$ type type
type is a shell builtin
```

### 查看命令的所有定义

通过 `type -a` 参数可以查看命令的所有定义：

```bash
$ type -a echo
echo is a shell builtin
echo is /usr/bin/echo
echo is /bin/echo
```

在上面的例子中，`echo` 既是一个内置命令，也有对应的外部程序。

### 返回命令类型

`type -t` 参数可以返回命令的类型，可能的结果包括：别名（alias）、关键词（keyword）、函数（function）、内置命令（builtin）、文件（file）。

```bash
$ type -t bash
file

$ type -t if
keyword
```

在这个例子中，`bash` 是一个文件，而 `if` 是一个关键词。

### 快捷键

Bash 提供了许多快捷键，能极大地提高操作效率。以下是一些常用的快捷键，更多细节可以参考《行操作》部分。

- **Ctrl + L**：清屏并将当前行移至页面顶部。
- **Ctrl + C**：中止当前正在执行的命令。
- **Shift + PageUp**：向上滚动查看终端内容。
- **Shift + PageDown**：向下滚动查看终端内容。
- **Ctrl + U**：删除光标位置到行首的内容。
- **Ctrl + K**：删除光标位置到行尾的内容。
- **Ctrl + W**：删除光标前的一个单词。
- **Ctrl + D**：关闭当前 Shell 会话。
- **↑，↓**：浏览历史命令记录。

### 自动补全

Bash 还支持自动补全功能。当命令输入到一半时，按下 **Tab** 键，Bash 会自动补全命令。例如，输入 `tou` 后按下 Tab 键，Bash 会自动补全为 `touch`。

不仅命令支持补全，文件路径也可以自动补全。输入路径的部分内容后按下 Tab 键，Bash 会补全剩余路径。如果有多个可能的选项，按两次 Tab 键，Bash 会显示所有可选路径。

## 模式扩展

### 波浪线扩展 

波浪线 `~` 在 Bash 中会自动扩展为当前用户的主目录。

```bash
$ echo ~
/home/me
```

`~/dir` 表示主目录中的某个子目录，其中 `dir` 是主目录中的子目录名。

```bash
# 进入 /home/me/foo 目录
$ cd ~/foo
```

`~user` 会扩展为指定用户的主目录：

```bash
$ echo ~foo
/home/foo

$ echo ~root
/root
```

如果 `~user` 后的用户名不存在，波浪线扩展不会生效，仍然显示原输入内容：

```bash
$ echo ~nonExistedUser
~nonExistedUser
```

`~+` 会扩展为当前所在的目录，等同于 `pwd` 命令的结果：

```bash
$ cd ~/foo
$ echo ~+
/home/me/foo
```

### ?扩展
`?` 字符在 Bash 中用于文件路径扩展，表示任意单个字符，不包括空字符。

#### 示例

```bash
# 存在文件 a.txt 和 b.txt
$ ls ?.txt
a.txt b.txt
```

在这个例子中，`?` 代表单个字符，所以会匹配 `a.txt` 和 `b.txt`。

如果要匹配多个字符，可以使用多个 `?` 连用。

```bash
# 存在文件 a.txt、b.txt 和 ab.txt
$ ls ??.txt
ab.txt
```

在这个例子中，`??` 匹配两个字符，因此 `ab.txt` 被匹配。

#### 文件名扩展

`?` 字符扩展是文件名扩展的一部分，只有在匹配的文件实际存在时才会生效。如果没有匹配到文件，扩展不会发生。

```bash
# 当前目录有 a.txt 文件
$ echo ?.txt
a.txt

# 当前目录为空目录
$ echo ?.txt
?.txt
```

在这个例子中，如果 `?.txt` 匹配到文件名，`echo` 会输出扩展后的文件名；如果没有匹配到文件，`echo` 会原样输出 `?.txt`。

### \* 扩展

`*` 字符在 Bash 中用于文件路径扩展，表示任意数量的任意字符，包括零个字符。

#### 示例

```bash
# 存在文件 a.txt、b.txt 和 ab.txt
$ ls *.txt
a.txt b.txt ab.txt
```

在这个例子中，`*.txt` 匹配所有以 `.txt` 结尾的文件。

如果想输出当前目录的所有文件，直接使用 `*`：

```bash
$ ls *
```

#### 匹配空字符

`*` 可以匹配零个或多个字符：

```bash
# 存在文件 a.txt 和 ab.txt
$ ls a*.txt
a.txt ab.txt
```

```bash
# 存在文件 b.txt 和 ab.txt
$ ls *b*
b.txt ab.txt
```

#### 隐藏文件匹配

`*` 不会匹配隐藏文件（以 `.` 开头的文件），例如：

```bash
$ ls *
```

如果要匹配隐藏文件，需要使用 `.*`：

```bash
# 显示所有隐藏文件
$ echo .*
```

要排除 `.` 和 `..` 这两个特殊的隐藏文件，可以使用以下表达式：

```bash
$ echo .[!.]*
```

#### 文件存在条件

与 `?` 字符扩展一样，`*` 字符扩展只有在匹配的文件存在时才会生效。如果没有匹配到文件，扩展不会发生：

```bash
# 当前目录不存在 c 开头的文件
$ echo c*.txt
c*.txt
```

在这个例子中，由于目录中没有 `c` 开头的文件，`c*.txt` 会原样输出。

#### 目录匹配

`*` 只匹配当前目录中的文件，不会匹配子目录中的文件：

```bash
# 子目录有 a.txt 文件
# 无效写法
$ ls *.txt

# 有效写法
$ ls */*.txt
```

如果需要匹配多层子目录中的文件，可以使用 `*/*.txt`。对于多层子目录，需要使用相应数量的 `*` 级别。

#### Bash 4.0 中的 `globstar`

Bash 4.0 引入了 `globstar` 参数，当启用时，`**` 可以匹配零个或多个子目录。例如：

```bash
# 匹配顶层和任意深度子目录中的所有文本文件
$ ls **/*.txt
```

`globstar` 允许 `**/*.txt` 匹配顶层目录和任意深度子目录中的 `.txt` 文件。更多内容可以参考 `shopt` 命令的详细介绍。

### 方括号扩展

[start-end] 扩展

方括号扩展有一个简写形式[start-end]，表示匹配一个连续的范围。比如，[a-c]等同于[abc]，[0-9]匹配[0123456789]。

```
# 存在文件 a.txt、b.txt 和 c.txt
$ ls [a-c].txt
a.txt
b.txt
c.txt

# 存在文件 report1.txt、report2.txt 和 report3.txt
$ ls report[0-9].txt
report1.txt
report2.txt
report3.txt
```

下面是一些常用简写的例子。

```
    [a-z]：所有小写字母。
    [a-zA-Z]：所有小写字母与大写字母。
    [a-zA-Z0-9]：所有小写字母、大写字母与数字。
    [abc]*：所有以a、b、c字符之一开头的文件名。
    program.[co]：文件program.c与文件program.o。
    BACKUP.[0-9][0-9][0-9]：所有以BACKUP.开头，后面是三个数字的文件名。
```

这种简写形式有一个否定形式[!start-end]，表示匹配不属于这个范围的字符。比如，[!a-zA-Z]表示匹配非英文字母的字符。

$ ls report[!1–3].txt
report4.txt report5.txt

上面代码中，[!1-3]表示排除1、2和3。

### 大括号扩展

大括号扩展 `{...}` 用于生成一系列的值，括号内的值用逗号分隔。它会扩展为括号内的每一个值，无论对应的文件是否存在。

#### 示例

```bash
$ echo {1,2,3}
1 2 3

$ echo d{a,e,i,u,o}g
dag deg dig dug dog

$ echo Front-{A,B,C}-Back
Front-A-Back Front-B-Back Front-C-Back
```

注意，大括号扩展与文件名扩展不同，它会生成所有给定的值，而不考虑是否存在相应的文件：

```bash
$ ls {a,b,c}.txt
ls: 无法访问'a.txt': 没有那个文件或目录
ls: 无法访问'b.txt': 没有那个文件或目录
ls: 无法访问'c.txt': 没有那个文件或目录
```

#### 注意事项

- **逗号前后不能有空格**。如果有空格，大括号扩展将失效：

  ```bash
  $ echo {1 , 2}
  {1 , 2}
  ```

- 逗号前可以没有值，表示扩展的第一项为空：

  ```bash
  $ cp a.log{,.bak}
  # 等同于
  $ cp a.log a.log.bak
  ```

#### 嵌套大括号扩展

大括号可以嵌套使用：

```bash
$ echo {j{p,pe}g,png}
jpg jpeg png

$ echo a{A{1,2},B{3,4}}b
aA1b aA2b aB3b aB4b
```

#### 与其他模式联用

大括号扩展可以与其他模式（如 `*`）联用，并且优先于其他模式扩展：

```bash
$ echo /bin/{cat,b*}
/bin/cat /bin/b2sum /bin/base32 /bin/base64 ... ...
```

该命令等同于：

```bash
$ echo /bin/cat; echo /bin/b*
```

#### 多字符扩展

大括号扩展支持多字符模式，而方括号扩展 `[abc]` 只能匹配单个字符：

```bash
$ echo {cat,dog}
cat dog
```

#### 文件存在性区别

大括号扩展始终会扩展，而方括号扩展仅在文件存在时扩展：

```bash
# 不存在 a.txt 和 b.txt
$ echo [ab].txt
[ab].txt

$ echo {a,b}.txt
a.txt b.txt
```

在这个例子中，如果文件 `a.txt` 和 `b.txt` 不存在，方括号扩展 `[ab].txt` 不会展开，而大括号扩展 `{a,b}.txt` 依然会生成 `a.txt` 和 `b.txt`。

### {start..end} 扩展

大括号扩展的简写形式 `{start..end}` 表示生成一个连续的序列。该序列可以是字母或数字。

#### 示例

```bash
$ echo {a..c}
a b c

$ echo d{a..d}g
dag dbg dcg ddg

$ echo {1..4}
1 2 3 4

$ echo Number_{1..5}
Number_1 Number_2 Number_3 Number_4 Number_5
```

#### 支持逆序

该扩展还支持逆序生成：

```bash
$ echo {c..a}
c b a

$ echo {5..1}
5 4 3 2 1
```

#### 无法理解的简写

当 Bash 无法理解某种简写时，它将原样输出：

```bash
$ echo {a1..3c}
{a1..3c}
```

#### 嵌套扩展

简写形式支持嵌套使用：

```bash
$ echo .{mp{3..4},m4{a,b,p,v}}
.mp3 .mp4 .m4a .m4b .m4p .m4v
```

#### 新建目录

大括号扩展的一个常见用途是新建一系列目录：

```bash
$ mkdir {2007..2009}-{01..12}
```

这条命令会新建 36 个子目录，格式为“年份-月份”。

#### 用于循环

在 `for` 循环中也常用大括号扩展：

```bash
for i in {1..4}
do
  echo $i
done
```

这个循环会执行 4 次，输出 `1` 到 `4`。

#### 前导零

如果起始数字有前导零，输出的每一项都会保留前导零：

```bash
$ echo {01..5}
01 02 03 04 05

$ echo {001..5}
001 002 003 004 005
```

#### 步长指定

可以使用第二个双点号 `{start..end..step}` 来指定步长：

```bash
$ echo {0..8..2}
0 2 4 6 8
```

在这个例子中，数字从 0 递增到 8，每次增加 2。

#### 连用扩展

多个简写形式连用时，会有循环处理效果：

```bash
$ echo {a..c}{1..3}
a1 a2 a3 b1 b2 b3 c1 c2 c3
```

这会生成所有可能的组合结果。


Bash 中的变量扩展、子命令扩展、算术扩展和字符类扩展都是常见的扩展操作，以下是详细介绍：

### 变量扩展

使用 `$` 符号来引用变量，Bash 会将其扩展为变量的值：

```bash
$ echo $SHELL
/bin/bash
```

变量也可以使用 `${}` 进行引用：

```bash
$ echo ${SHELL}
/bin/bash
```

使用 `${!string*}` 或 `${!string@}` 可以返回所有匹配指定前缀的变量名：

```bash
$ echo ${!S*}
SECONDS SHELL SHELLOPTS SHLVL SSH_AGENT_PID SSH_AUTH_SOCK
```

### 子命令扩展

子命令扩展使用 `$(...)` 语法，可以将另一个命令的运行结果作为返回值：

```bash
$ echo $(date)
Tue Jan 28 00:01:13 CST 2020
```

另一种较老的语法是使用反引号 `` `...` ``：

```bash
$ echo `date`
Tue Jan 28 00:01:13 CST 2020
```

子命令扩展支持嵌套，例如：

```bash
$ echo $(ls $(pwd))
```

### 算术扩展

使用 `((...))` 进行整数运算，并返回结果：

```bash
$ echo $((2 + 2))
4
```

### 字符类扩展

字符类 `[[:class:]]` 用于匹配特定类型的字符。常用字符类如下：

- `[[:alnum:]]`：字母或数字
- `[[:alpha:]]`：字母
- `[[:blank:]]`：空格和 Tab
- `[[:cntrl:]]`：控制字符（ASCII 码 0-31）
- `[[:digit:]]`：数字
- `[[:graph:]]`：可打印字符（不含空格）
- `[[:lower:]]`：小写字母
- `[[:print:]]`：可打印字符（含空格）
- `[[:punct:]]`：标点符号
- `[[:space:]]`：空白字符
- `[[:upper:]]`：大写字母
- `[[:xdigit:]]`：十六进制字符

例如，列出所有以大写字母开头的文件：

```bash
$ echo [[:upper:]]*
```

字符类可以通过加 `!` 或 `^` 来表示否定：

```bash
$ echo [![:digit:]]*
```

#### 使用注意点

1. **通配符先解释再执行**  
   Bash 会先扩展通配符，然后执行命令：

   ```bash
   $ ls a*.txt
   ab.txt
   ```

2. **文件名扩展原样输出**  
   如果没有匹配的文件，通配符原样输出：

   ```bash
   $ echo r*
   r*
   ```

3. **通配符仅匹配单层路径**  
   `*` 和 `?` 只能匹配当前目录的文件，无法匹配子目录。如果要匹配子目录中的文件，可以使用 `*/`：

   ```bash
   $ ls */*.txt
   ```

4. **文件名中使用通配符**  
   如果文件名中包含通配符，需要使用引号引起来：

   ```bash
   $ touch 'fo*'
   $ ls
   fo*
   ```

这些扩展功能帮助你灵活操作 Bash 命令和文件路径。

### 量词语法

Bash 中的量词语法用来控制模式匹配的次数，但需要 `extglob` 参数开启。一般情况下，`extglob` 是默认打开的，你可以使用以下命令来检查或开启它：

```bash
$ shopt extglob
extglob         on

$ shopt -s extglob  # 开启 extglob
```

### 量词语法

- `?(pattern-list)`：匹配零次或一次模式。
- `*(pattern-list)`：匹配零次或多次模式。
- `+(pattern-list)`：匹配一次或多次模式。
- `@(pattern-list)`：只匹配一次模式。
- `!(pattern-list)`：匹配给定模式以外的任何内容。

#### 示例

```bash
$ ls abc?(.)txt
abctxt abc.txt
```
`?(.)` 匹配零个或一个点。

```bash
$ ls abc?(def)
abc abcdef
```
`?(def)` 匹配零个或一个 `def`。

```bash
$ ls abc@(.txt|.php)
abc.php abc.txt
```
`@(.txt|.php)` 只匹配 `.txt` 或 `.php` 后缀的文件。

```bash
$ ls abc+(.txt)
abc.txt abc.txt.txt
```
`+(.txt)` 匹配一个或多个 `.txt`。

```bash
$ ls a!(b).txt
a.txt abb.txt ac.txt
```
`!(b)` 匹配非 `b` 的内容。

#### `shopt` 命令

`shopt` 命令可以控制 Bash 的一些行为，特别是通配符扩展相关的参数：

- `dotglob`：匹配包括隐藏文件（以 `.` 开头的文件）。

  ```bash
  $ shopt -s dotglob
  $ ls *
  abc.txt .config
  ```

- `nullglob`：当没有匹配文件时，通配符返回空字符串。

  ```bash
  $ shopt -s nullglob
  $ rm b*  # 如果没有 b* 匹配的文件，rm 不会报错。
  ```

- `failglob`：当没有匹配文件时，直接报错。

  ```bash
  $ shopt -s failglob
  $ rm b*  # 如果没有匹配文件，Bash 会直接报错。
  bash: 无匹配: b*
  ```

- `extglob`：支持扩展的通配符语法（如量词语法），默认开启。

  ```bash
  $ shopt -s extglob
  ```

- `nocaseglob`：通配符扩展不区分大小写。

  ```bash
  $ shopt -s nocaseglob
  $ ls /windows/program*
  /windows/ProgramData /windows/Program Files /windows/Program Files (x86)
  ```

- `globstar`：允许 `**` 匹配零个或多个子目录。默认关闭。

  假设有如下文件结构：

  ```bash
  a.txt
  sub1/b.txt
  sub1/sub2/c.txt
  ```

  默认情况下，你需要分层写出通配符来匹配子目录：

  ```bash
  $ ls *.txt */*.txt */*/*.txt
  ```

  开启 `globstar` 后，`**/*.txt` 可以匹配所有子目录中的 `.txt` 文件：

  ```bash
  $ shopt -s globstar
  $ ls **/*.txt
  a.txt  sub1/b.txt  sub1/sub2/c.txt
  ```

通过这些选项，Bash 的通配符扩展可以变得更加灵活和强大，满足各种复杂文件匹配的需求。

## 引号和转义

Bash 只有一种数据类型，就是字符串。不管用户输入什么数据，Bash 都视为字符串。因此，字符串相关的引号和转义，对 Bash 来说就非常重要。

在 Bash 中，某些字符具有特殊含义（如 `$`、`&`、`*` 等），如果你想要原样输出这些特殊字符，需要使用反斜杠进行转义。

### 特殊字符的转义

例如，直接输出 `$` 符号时，它会被解释为变量符号，因此不会有任何结果：

```bash
$ echo $date

# 输出为空
```

为了原样输出 `$`，需要在它前面加反斜杠进行转义：

```bash
$ echo \$date
$date
```

#### 反斜杠的转义

反斜杠 `\` 本身也是一个特殊字符。如果想要输出反斜杠本身，需要对其自身进行转义，使用两个反斜杠 `\\`：

```bash
$ echo \\
\
```

#### 表示不可打印字符

反斜杠还可以用于表示一些不可打印的字符：

- `\a`：响铃
- `\b`：退格
- `\n`：换行
- `\r`：回车
- `\t`：制表符

为了正确输出这些字符，需要使用 `echo` 命令的 `-e` 参数，并将字符放在引号中：

```bash
$ echo -e "a\tb"
a        b
```

在不使用 `-e` 参数时，Bash 不会正确解释这些特殊字符：

```bash
$ echo a\tb
atb
```

#### 多行命令的书写

反斜杠还可以用于将一条命令分成多行书写。在行尾加上反斜杠，Bash 会忽略换行符，将下一行视为同一条命令的延续：

```bash
$ mv \
/path/to/foo \
/path/to/bar

# 等同于
$ mv /path/to/foo /path/to/bar
```

这种写法适用于命令较长的情况下，能够提高命令的可读性。

通过使用反斜杠进行转义和多行书写，你可以灵活处理 Bash 中的特殊字符和长命令。

在 Bash 中，单引号用来保留字符的字面含义，所有在单引号内的字符都会被视为普通字符，即使它们通常在 Bash 中具有特殊含义。

### 单引号的特点

- **保留特殊字符的字面含义**：在单引号内，特殊字符如 `*`、`$`、`\` 等不会被 Bash 扩展或解释。

#### 示例

```bash
$ echo '*'
*
```

```bash
$ echo '$USER'
$USER
```

```bash
$ echo '$((2+2))'
$((2+2))
```

```bash
$ echo '$(echo foo)'
$(echo foo)
```

在这些例子中，单引号使得 Bash 的扩展（如变量、算术运算、子命令）失效，所有内容都按字面输出。

### 单引号内不能转义

由于反斜杠 `\` 在单引号内被视为普通字符，所以不能用于转义单引号。如果要在单引号内使用单引号，需要使用 `\$` 形式。

#### 错误示例

```bash
$ echo it's
# 报错，因为单引号没有正确关闭
```

```bash
$ echo 'it\'s'
# 报错，反斜杠在单引号内不起作用
```

#### 正确方法

可以通过在外层加上 `$` 来允许转义单引号：

```bash
$ echo $'it\'s'
it's
```

或者更简洁的方式是使用双引号来包含单引号：

```bash
$ echo "it's"
it's
```

#### 总结

- 单引号内的内容不会被 Bash 解析或扩展，所有字符都按字面输出。
- 反斜杠在单引号内不起作用，不能转义单引号。如果需要在单引号内使用单引号，可以使用 `$'...'` 或改用双引号。

使用单引号时，确保所有字符的含义都不需要 Bash 扩展，否则建议使用双引号。

### 双引号

双引号在 Bash 中的行为比单引号更加宽松，允许某些特殊字符继续发挥它们的作用，而其他字符则会变为普通字符。

#### 特点

- **大部分特殊字符失去特殊含义**：例如，通配符 `*` 在双引号中会失去文件名扩展功能，变为普通字符。

#### 示例

```bash
$ echo "*"
*
```

在这个例子中，`*` 被当作普通字符，未进行文件名扩展。

#### 保持特殊含义的字符

在双引号中，三个字符保持其特殊含义：
1. **美元符号 `$`**：继续引用变量。
2. **反引号 `` ` ``**：继续执行子命令。
3. **反斜杠 `\`**：继续作为转义字符。

#### 示例

```bash
$ echo "$SHELL"
/bin/bash

$ echo "`date`"
Mon Jan 27 13:33:18 CST 2020
```

在这个例子中，`$SHELL` 继续引用变量，`` `date` `` 继续执行子命令。

#### 反斜杠的转义作用

反斜杠在双引号中仍然保留其转义功能，可以用于插入双引号或反斜杠本身。

```bash
$ echo "I'd say: \"hello.\""
I'd say: "hello."

$ echo "\\"
\
```

#### 多行文本

在双引号中，换行符不再被 Bash 解释为命令的结束符，而只是作为普通的换行符。因此，可以在双引号中输入多行文本。

```bash
$ echo "hello
world"
hello
world
```

#### 文件名包含空格

当文件名包含空格时，必须使用双引号或单引号来将文件名括起来，否则 Bash 会将文件名中的空格解释为多个文件名。

```bash
$ ls "two words.txt"
```

#### 原样保存空格和格式

双引号可以保留多余的空格，确保输出与输入一致。

```bash
$ echo "this is a     test"
this is a     test
```

双引号还能保存原始的命令输出格式。如果不使用双引号，Bash 会将所有输出压缩为单行。

#### 示例

```bash
# 单行输出
$ echo $(cal)
一月 2020 日 一 二 三 四 五 六 1 2 3 ... 31

# 原始格式输出
$ echo "$(cal)"
      一月 2020
日 一 二 三 四 五 六
          1  2  3  4
 5  6  7  8  9 10 11
12 13 14 15 16 17 18
19 20 21 22 23 24 25
26 27 28 29 30 31
```

在这个例子中，双引号保留了 `cal` 命令的原始格式。

#### 总结

- **双引号**：保留 `$`、`` ` `` 和 `\` 的特殊含义，其他字符变为普通字符。
- **用途**：用于处理包含空格的文件名，保持输出格式，防止 Bash 进行不必要的扩展。

### Here 文档

Here 文档（here document）是一种在 Bash 中输入多行字符串的方法，其格式如下：

```bash
<< token
text
token
```

#### 格式说明

- **开始标记**：`<< token`，其中 `token` 是 Here 文档的名称，可以是任意名称。
- **结束标记**：`token`，必须单独一行顶格书写。

Here 文档会将开始标记和结束标记之间的所有内容作为输入，并将这些内容提供给一个命令处理。

#### 示例

以下是一个通过 Here 文档输出 HTML 代码的示例：

```bash
$ cat << _EOF_
<html>
<head>
    <title>
    The title of your page
    </title>
</head>

<body>
    Your page content goes here.
</body>
</html>
_EOF_
```

在这个例子中，`_EOF_` 是 Here 文档的标记，内容是多行 HTML 代码，Bash 会将它们作为输入传递给 `cat` 命令。

#### Here 文档中的变量替换

Here 文档支持变量替换和反斜杠转义，但是不支持通配符扩展，双引号和单引号在 Here 文档中失去了它们的语法作用。

#### 示例

```bash
$ foo='hello world'
$ cat << _example_
$foo
"$foo"
'$foo'
_example_
```

输出：

```bash
hello world
"hello world"
'hello world'
```

在这个例子中，变量 `$foo` 被替换为其值，但双引号和单引号被原样输出。

#### 禁用变量替换

如果希望避免变量替换，可以将 Here 文档的开始标记放在单引号中：

```bash
$ foo='hello world'
$ cat << '_example_'
$foo
"$foo"
'$foo'
_example_
```

输出：

```bash
$foo
"$foo"
'$foo'
```

在这个例子中，由于开始标记 `_example_` 被单引号包裹，因此变量 `$foo` 没有被替换。

#### Here 文档的作用机制

Here 文档的本质是重定向。它将字符串重定向给某个命令，相当于 `echo` 命令的重定向：

```bash
$ command << token
string
token
```

等同于：

```bash
$ echo string | command
```

Here 文档适用于那些能够接受标准输入的命令。它不能作为命令本身的参数，例如 `echo` 命令不能使用 Here 文档：

```bash
$ echo << _example_
hello
_example_
```

在这个例子中，`echo` 命令不会输出任何内容，因为 Here 文档对 `echo` 命令无效。

#### 注意事项

- **Here 文档不能作为变量的值**，它只能用于命令的参数。
- **适用于接受标准输入的命令**，例如 `cat`、`grep` 等，但对于不接受标准输入的命令无效。

Here 文档是一种方便的方式来输入多行文本并将其传递给命令处理，尤其是在处理长文本块时非常有用。

### Here字符串

Here 字符串（Here string）是 Here 文档的变体，使用三个小于号（`<<<`）表示，它将一个字符串通过标准输入传递给命令。

#### 基本语法

```bash
<<< string
```

Here 字符串的作用是将给定的字符串传递给命令的标准输入，而不是作为命令的直接参数。这对于一些只接受标准输入的命令非常有用。

#### 示例

```bash
$ cat <<< 'hi there'
```

上面的命令使用 Here 字符串将 `'hi there'` 传递给 `cat` 命令，等同于下面的管道命令：

```bash
$ echo 'hi there' | cat
```

使用 Here 字符串的语法更加简洁，且语义上更加清晰。

#### 应用场景

有些命令只能通过标准输入接收数据，而不能直接将字符串作为参数传递。这时 Here 字符串非常有用。例如 `md5sum` 命令：

```bash
$ md5sum <<< 'ddd'
```

这等同于：

```bash
$ echo 'ddd' | md5sum
```

如果你直接运行 `md5sum ddd`，`ddd` 会被解释为文件名，而不是字符串内容。因此，使用 Here 字符串可以避免这种误解，将 `'ddd'` 作为字符串传递给 `md5sum`。

#### 总结

Here 字符串是一个方便的工具，它允许将字符串直接作为标准输入传递给命令，语法简洁且清晰，适用于那些只接受标准输入的命令。






