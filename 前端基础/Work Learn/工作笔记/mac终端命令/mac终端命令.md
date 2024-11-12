### 文件目录

```
"/":根目录

"~":用户主目录的缩写，例如当前用户为hello,那么"~"展开来就是：/user/hello

".":当前目录

".."父目录
```

### 命令

1. cd跳转到某个目录

```
cd / 表示跳转到根目录。

cd ~ 表示跳转到用户主目录。

cd ~apple 表示跳转到用户apple的主目录。

cd .. 表示跳转到上级目录。（cd和..之间的空格不能漏）
```

2. ls列出当前目录下的子目录和文件
3. pwd显示当前目录的路径
4. clear清空当前输入
5. history查看输入历史记录
   在Terminal输入命令时，可以使用上下方向键查看之前输入的命令（和windows的cmd相同）。另外，可以用history查看输入的完整历史
6. mkdir  

```
例：在驱动目录下建一个备份目录
backup mkdir /System/Library/Extensions/backup

在桌面上建一个备份目录
backup mkdir /User/用户名/Desktop/backup
```

7. cp拷贝文件
   cp 参数 源文件 目标文件
   
```
例：想把桌面的Natit.kext 拷贝到驱动目录中
cp -R /User/用户名/Desktop/Natit.kext /System/Library/Extensions

参数R表示对目录进行递归操作，kext在图形界面下看起来是个文件，实际上是个文件夹。
把驱动目录下的所有文件备份到桌面
backup
cp -R /System/Library/Extensions/* /User/用户名/Desktop/backup
```

8. 删除文件
   rm 参数 文件
```
rm -rf /System/Library/Extensions.kextcache rm -rf /System/Library/Extensions.mkext
参数－rf 表示递归和强制，千万要小心使用，如果执行了 rm -rf / 你的系统就全没了
```

9. 移动文件
   mv 文件

```
例：想把AppleHDA.Kext 移到桌面
mv /System/Library/Extensions/AppleHDA.kext /User/用户名/Desktop

想把AppleHDA.Kext 移到备份目录中
mv /System/Library/Extensions/AppleHDA.kext /System/Library/Extensions/backu
```

更多操作
https://blog.csdn.net/ancientear/article/details/81054986