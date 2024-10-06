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




