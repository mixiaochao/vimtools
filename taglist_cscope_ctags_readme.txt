
日期: 2020/07/16
米晓超测试总结

查看内核源码的linux环境工具配置及使用

1. 工具使用老牌的vim编辑器外加taglist、ctags、cscope

2. 确保vim编辑器已安装，如果未安装请通过如下命令安装

	sudo    apt-get	  install    vim
	
3. taglist插件下载安装

	下载来源: https://sourceforge.net/projects/vim-taglist/files/
	安	 装:	
			#解压
			unzip 	taglist_46.zip 
			#拷贝
			sudo  cp   doc/taglist.txt    /usr/share/vim/vim81/doc/
			sudo  cp   plugin/taglist.vim /usr/share/vim/vim81/plugin/

4. ctags安装

	sudo  apt-get   install   ctags
	
5. cscope安装

	sudo	apt-get   install   cscope
	
6. 针对taglist、ctags、cscope三个插件对应vim的配置

	#假定要追踪的源代码路径为：/home/mixch/works/loongson-kernel

	6.1 通过ctags命令生成需要的tags文件
	
	cd        /home/mixch/works/loongson-kernel
    ctags     -R
    
    #就可以看到当前目录多了一个tags文件。
    
    6.2 通过cscope命令在源码目录的顶层目录生成需要的cscope.out、cscope.in.out、cscope.po.out文件 
    
    cd        /home/mixch/works/loongson-kernel
    cscope    -Rbkq
 
	#R 表示把所有子目录里的文件也建立索引
	#b 表示cscope不启动自带的用户界面，而仅仅建立符号数据库
	#q 生成cscope.in.out和cscope.po.out文件，加快cscope的索引速度
	#k 在生成索引文件时，不搜索/usr/include目录
    #执行完cscope命令后就可以在当前目录看到cscope.out、cscope.in.out、cscope.po.out这三个文件。
   
    
7. 针对taglist、ctags、cscope三个插件的vim配置，在用户家目录新建文件.vimrc

	touch    ~/.vimrc
	
	#内容见vimrc_bak。
	
	#直接拷贝
	
	cp    vimrc_bak     ~/.vimrc
	
	#注意第一行内容根据情况修改 
	#set  tags=/home/mixch/works/loongson-kernel/tags
	#因为示例源码目录的tags文件路径是/home/mixch/works/loongson-kernel/tags
	#所以请根据情况修改，其他内容固定不变哦。
	
8. 查看源码过程中针对cscope的命令

	8.1 如果要使用cscope的命令请进入到源码目录。
	8.2 追踪内核源码的一般途径、方法

		@在命令行查找源码 vim    -t     函数名/宏名/变量名
		
		@在vim编辑器查看源码过程中要跳转到光标所在符号的地方请快捷键:
		
		ctrl  +  ]
		
		@通过ctrl +  ]跳转到的地方如何跳回原来调用的地方，快捷键:
		
		ctrl + t 或 ctrl + o
		
	8.3 taglist的使用
	
	taglist依赖ctags，所以需要安装好ctags，并生成tag文件，然后才可以使用taglist。

	在vim编辑器的命令行模式可以打开taglist窗口，默认以列的方式出现在vim编辑框的左侧。
	
	使用 “:TlistToggle” 在打开和关闭间切换
	使用 “:TlistOpen” 打开taglist窗口，用“:TlistClose”关闭taglist窗口。
	使用 “ctrl+w+w”快捷键 在正常编辑区域和tags区域中切换
	定位指定内容在tags区域中，把光标移动到变量、函数名称上，然后敲回车(或者是双击某个tag)，就会自动在正常编辑区域中定位到指定内容了。

	回车 跳到光标下tag所定义的位置，用鼠标双击此tag功能也一样

	o 在一个新打开的窗口中显示光标下tag
	空格 （空格）显示光标下的tag的原型定义
	u 更新taglist窗口中的tag
	s 更改排序方式，在按名字排序和按出现顺序排序间切换
	x taglist窗口放大和缩小，方便查看较长的tag
	+ 打开一个折叠，同zo
	- 将tag折叠起来，同zc
	* 打开所有的折叠，同zR
	= 将所有tag折叠起来，同zM
	[[ 跳到前一个文件
	]] 跳到后一个文件
	q 关闭taglist窗口
	
	8.4 cscope 插件的在vim编辑器的查找命令及快捷键
	
	@查找命令如下
	
	：cs find s ---- 查找C语言符号，即查找函数名、宏、枚举值等出现的地方
	：cs find g ---- 查找函数、宏、枚举等定义的位置，类似ctags所提供的功能
	：cs find d ---- 查找本函数调用的函数
	：cs find c ---- 查找调用本函数的函数
	：cs find t: ---- 查找指定的字符串
	：cs find e ---- 查找egrep模式，相当于egrep功能，但查找速度快多了
	：cs find f ---- 查找并打开文件，类似vim的find功能
	：cs find i ---- 查找包含本文件的文

	@对应查找命令的快捷键
	
	所有命令也可以通过快捷键来实现：
	比如Ctrl+\ 再按 c 等价命令 ：cs find s
	同理cs find + g,d,c,t,e,f,i命令的快捷键就是ctrl + \ + s,g,d,t,e,f,i。


