```bash
符号        ASCII码        意义

\n            10          换行

\r            13          回车CR
```

```c
string s1 = "已经习惯了回车和换行一次搞定\n，敲一个回车键，即是回";
Console.WriteLine(s1);
s1 = "已经习惯了回车和换行一次搞定\r，敲一个回车键，即是回";
Console.WriteLine(s1);
s1 = "已经习惯了回车和换行一次搞定\r\n，敲一个回车键，即是回";
Console.WriteLine(s1);
Console.ReadLine();
```

![image-20230818153637123](E:\github\notes\maintenance\crlf&lf.assets\image-20230818153637123.png)

# 1. LF：换行

**LF**（Line Feed）代表“换行”，但你可能更熟悉术语换行符（转义序列 \n）。

# 2. CR：回车

**CR**（Carriage Return）代表回车（转义序列\r），将光标移动到当前行的开头。

**终端上的下载进度条**就是**通过CR实现**的，通过使用回车符，你的终端可以通过将光标返回到当前行的开头并覆盖任何先前呈现的文本来将文本动画化。

# 3. 历史

在计算机还没有出现之前，有一种叫做电传打字机（**Teletype Model 33**，Linux/Unix下的tty概念也来自于此）的玩意，每秒钟可以打10个字符。但是它有一个问题，就是打完一行换行的时候，要用去0.2秒，正好可以打两个字符。要是在这0.2秒里面，又有新的字符传过来，那么这个字符将丢失。

于是，研制人员想了个办法解决这个问题，就是在每行后面加两个表示结束的字符。一个叫做“回车”，告诉打字机把打印头定位在左边界；另一个叫做“换行”，告诉打字机把纸向下移一行。这就是“换行”和“回车”的来历，从它们的英语名字上也可以看出一二。

后来，计算机发明了，这两个概念也就被般到了计算机上。那时，存储器很贵，一些科学家认为在每行结尾加两个字符太浪费了，加一个就可以。于是，就出现了分歧。

# 4. 检查和转换行尾（在 Bash 中）

windows 系统使用CRLF(`\r\n`)作为换行符，Unix系统（Linux，MacOS）使用`LF`

```bash
$  cat -A file1.txt
111^M$	# ^M 表示回车，$ 表示换行
```

+ 通过`core.autocrlf`可以更改git处理换行符的方式：

  + 当设置为true时，git提交代码时会自动将CRLF转换为LF，在检出代码时会将LF转换成CRLF。一般在windows上使用该功能

  + 在Linux或MacOS上时，使用LF作为换行符，检出代码时不需要转换。但是当代码中已有一个CRLF换行符时，在提交时就需要将其转换成LF。因此input的功能是**在提交代码时，将CRLF转换成LF；检出代码时不转换**

  + 当设置成false时，表示不做任何转换。因此可以把CRLF提交到版本库中


+ 通过`core.safecrlf`可以检测crlf问题是否处理好了

```bash
# 不允许提交包含混合换行符的文件
git config --global core.safecrlf true

# 允许提交包含混合换行符的文件
git config --global core.safecrlf false

# 提交包含混合换行符的文件时给出警告
git config --global core.safecrlf warn
```

+ 通过.gitattribute文件配置仓库的git是更好的方式，使用了.gitattribute则不需要设置`core.autocrlf`和`core.eol`

```bash
* text=auto eol=lf
```