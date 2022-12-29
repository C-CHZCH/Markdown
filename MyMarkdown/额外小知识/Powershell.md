## 简单小命令

ctrl+L可以清空当前powershell窗口内容并让光标回到顶部。
date ——输出当前系统所在地区的时间

在Windows Powershell中，**echo** 亦或者 **Write-Output**命令用于在控制台打印变量或者字符串。
比如我们可以给shell提供一个hello。
```Shell
echo hello
// return hello

echo "Hello World"
//return Hello World
```

我们应该确立的一个思想是，Shell是一种编程语言，它根据其特殊的搜索方式以及用户提供的关键词来确立程序运行的位置
在Shell中，我们不仅可以执行这样的一些小程序，我们也可以定义其他的东西，比如说while循环，for循环，条件if等等所有的这些我们都可以定义、也可以是函数，函数之中我们可以定义变量。所有的这些东西我们都可以在shell中做。

* 环境变量
	环境变量是在启动shell时进行设置的，它们并不是每次启动都会进行全部的设置，但是我们必须设置一些关键的东西，比如主目录，用户名，路径变量。
* 绝对路径
	完全确定文件位置的路径
* 相对路径
	相对于当前文件位置的路径

在powershell中，我们输入pwd用于打印当前的工作目录。

我们可以使用cd命令改变当前的工作目录 ，比如
cd C:\\，这样我们就可以把powershell的工作目录转移到我们所指定的目录上来。当然我们也不一定需要提供完整的目录。一个.代表着当前目录，..代表着父目录。我们可以使用cd\来回到根目录 。

除了pwd，我们还有ls命令，它可以告诉我们当前目录中的文件目录。ls也可以配合..来显示父级目录下存在哪些文件。当然和ls等效的powershell命令有dir与gci。我们也可以使用Get-ChildItem命令来显示指定目录中的所有文件和目录，当没有提供目录时，它会显示当前工作目录中的所有文件和目录。我们可以对其进行-Path来指定路径，Linux中ls-a命令用于列出文件或目录（包含隐藏目录），而在Get-ChildItem中，我们可以对其补充-Force参数来查看文件或目录（包含隐藏的目录）。

当我们展开一个文件目录时，最左边显示出来的其实时该文件设置的权限。这里需要区分目录权限和文件权限。比如：我拥有文件的写入权限，那么我就可以修改文件的内容，但如果我没有该目录的写入权限，那么我就只能清空文件而不能删除文件。

如果我们需要移动亦或者重命名一个文件，那么我们可以使用Move-Item命令。
1. 示例 1：将文件移动到另一个目录并将其重命名
此命令将 `Test.txt` 文件从 `C:` 驱动器移动到 `E:\Temp` 目录，并将其重命名 `test.txt` 为 `tst.txt`。
```
Move-Item -Path C:\test.txt -Destination E:\Temp\tst.txt
```

2. 示例 2：将目录及其内容移动到另一个目录
此命令将 `C:\Temp` 目录及其内容移动到 `C:\Logs` 目录。 目录 `Temp` 及其所有子目录和文件，然后显示在 `Logs` 目录中。

```
Move-Item -Path C:\Temp -Destination C:\Logs
```


重命名文件目录：
```
Rename-Item D:\temp\Test D:\temp\Test1
```

重命名文件：
```
Rename-Item D:\temp\Test\test.txt test1.txt
```

复制文件：
在PowerShell ISE控制台中键入以下命令
```
Copy-Item 'D:\temp\Test Folder\Test File.txt' 'D:\temp\Test Folder1\Test File1.txt'
```
如果想要递归的赋值文件目录下所有的文件，参考微软文档。

删除文件夹或文件夹我们可以使用 Remove-Item并提供文件夹的路径即可。在删除文件夹时，我们可以给powershell一个额外的命令。Recurse 参数以递归方式删除“OldApp”键的所有内容。 如果密钥包含子项，并且省略 了 Recurse 参数，系统会提示确认要删除密钥的内容。
```
Remove-Item HKLM:\Software\MyCompany\OldApp -Recurse
```

我们可以通过<>来分别改变powershell的输入流以及输出流
比如：
```
echo hello > hello.txt
```
假如当前目录不存在hello.txt，那么系统就会在当前目录创建此文件，并且把hello输出到该文件。
同样的，我们可以使用cat命令来把hello.txt中的内容输出到powershell中来
```
cat  hello.txt
```

Windows不支持< 这个定向符。
我们可以这样来实现复制txt中的内容到一个新的文件中：
```
cat hello.txt > hello1.txt
```

在Windows中>>是追加符号。  

powershell中有一个叫管道运算符 | 
它用于将前面命令的输出发送到后面的命令。当输出包含多个对象（“集合”）时，管道运算符一次发送一个对象。

```

```