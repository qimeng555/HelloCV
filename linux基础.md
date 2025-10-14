我用的方式是双系统安装ubuntu22.04

对于Linux的命令

```plain
sudo apt install xxxxxxx
```

```plain
rm -rf xxxxxxx
```

```plain
cp xxxxx xxxxx
```

```plain
chmod 777 -R xxxxxxx
```

等等一些基本的终端命令

大部分我写到纸质本子上了

<font style="color:rgb(0, 0, 0);"></font>

<font style="color:rgb(0, 0, 0);"> Linux会把计算机所有的硬件映射成一个软件来管理</font>

<font style="color:rgb(0, 0, 0);">例如dev中cpu会分成几个文件来进行管理</font>

<font style="color:rgb(0, 0, 0);">disk硬盘也会映射成一个文件</font>

<font style="color:rgb(0, 0, 0);">甚至于网络也会被映射成文件</font>

<font style="color:rgb(0, 0, 0);">每个文件都归属于不同的目录之下</font>

<font style="color:rgb(0, 0, 0);">在实际操作中，由专人进行负责</font>

<font style="color:rgb(0, 0, 0);">这就是，Linux中一切皆文件</font>

<font style="color:rgb(0, 0, 0);"></font>

<font style="color:rgb(0, 0, 0);">根目录/是文件系统的顶级目录和起点</font>

<font style="color:rgb(0, 0, 0);">作为所有目录的根基所在</font>

<font style="color:rgb(0, 0, 0);">提供统一的访问入口</font>

<font style="color:rgb(0, 0, 0);">具有唯一性，必须存在性和最高权限</font>

<font style="color:rgb(0, 0, 0);"></font>

<font style="color:rgb(0, 0, 0);">/home是每个用户的工作目录起点</font>

<font style="color:rgb(0, 0, 0);">用于存储用户配置文件，个人文档和应用程序</font>

<font style="color:rgb(0, 0, 0);"></font>

<font style="color:rgb(0, 0, 0);"></font>

<font style="color:rgb(0, 0, 0);">/etc则是系统配置文件的存储位置</font>

<font style="color:rgb(0, 0, 0);">例如网络配置，安装包管理等等</font>

<font style="color:rgb(0, 0, 0);"></font>

<font style="color:rgb(0, 0, 0);">对于其他的/var存储可改变的文件</font>

<font style="color:rgb(0, 0, 0);">/usr包含用户所用的所有命令</font>

<font style="color:rgb(0, 0, 0);">等等根目录下的各分目录</font>

<font style="color:rgb(0, 0, 0);"></font>

<font style="color:rgb(0, 0, 0);">而对于绝对路径和相对路径而言</font>

<font style="color:rgb(0, 0, 0);">就是路线就是指从根目录下层层分级到当前目录所在地</font>

<font style="color:rgb(0, 0, 0);">而相对路径则是对于当前文档而言的位置</font>

<font style="color:rgb(0, 0, 0);">打个比方</font>

<font style="color:rgb(0, 0, 0);">绝对路径就是位置所在地</font>

<font style="color:rgb(0, 0, 0);">比如说，银河系，太阳系，地球，中国，山东威海，哈尔滨工业大学威海校区</font>

<font style="color:rgb(0, 0, 0);">而相对路径则是</font>

<font style="color:rgb(0, 0, 0);">哈工威在山威的旁边</font>

<font style="color:rgb(0, 0, 0);"></font>

<font style="color:rgb(0, 0, 0);">Linux系统的网络配置文件通常存储在/etc/network/interfaces中</font>

<font style="color:rgb(0, 0, 0);">包含IP地址、子网掩码、网关等</font>

<font style="color:rgb(0, 0, 0);"></font>

<font style="color:rgb(0, 0, 0);">而vim编译器</font>

<font style="color:rgb(0, 0, 0);">即是相当于记事本，可添加标签</font>

<font style="color:rgb(0, 0, 0);">而其内部则是分为命令模式，编辑模式和底行模式（好像叫法不同）</font>

<font style="color:rgb(0, 0, 0);">直接进去是命令模式，如hjkl移动dd删除行，yy复制</font>

<font style="color:rgb(0, 0, 0);">通过键盘按键（例如i）进入编辑模式，就是记事本了</font>

<font style="color:rgb(0, 0, 0);">然后esc可退出，再按:进入底行模式：q不改退出</font>

<font style="color:rgb(0, 0, 0);">：wq该了且保存退出:q!该了不保存退出等等</font>

<font style="color:rgb(0, 0, 0);"></font>

<font style="color:rgb(0, 0, 0);">tmux</font>

<font style="color:rgb(0, 0, 0);">打开终端窗口输入命令进行会话的</font>

<font style="color:rgb(0, 0, 0);">比如ssh登陆远程计算机打开窗口执行命令，突然中断，会话就终止了</font>

<font style="color:rgb(0, 0, 0);">而tmux就是解绑的工具</font>

<font style="color:rgb(0, 0, 0);">窗口关闭，会话并不终止</font>

<font style="color:rgb(0, 0, 0);">ctrl+d用于退出</font>

<font style="color:rgb(0, 0, 0);">而这些窗口可同时存在很多个</font>

<font style="color:rgb(0, 0, 0);">编号为0,1,2,3......（也可以起名）</font>

<font style="color:rgb(0, 0, 0);">而tmux attach则命令重新连接某个已经存在的会话</font>

<font style="color:rgb(0, 0, 0);">tmux attach -t 0</font>

<font style="color:rgb(0, 0, 0);">tmux kill-session -t 0（终止会话)</font>

<font style="color:rgb(0, 0, 0);">tmux switch -t 0 (切换）</font>

<font style="color:rgb(0, 0, 0);">等等</font>

<font style="color:rgb(0, 0, 0);"></font>

<font style="color:rgb(0, 0, 0);">对于grep这个文本搜索工具</font>

<font style="color:rgb(0, 0, 0);">grep -i / -n / -r用于不同的输入匹配</font>



                      

