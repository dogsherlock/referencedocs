
##### 前言

> 常见的快捷键可以帮助程序员脱离鼠标的束缚，提升效率 
> 当然，最重要的是可以装逼!
___






##### Keyboard shortcuts I often use

> Windows

```
Ctrl+Number
ALT+Esc 使当前窗口最小化
Win+D最小化所有窗口，再次按还原窗口
Win+Shift+左右arrow在双屏中左右移动当前窗口
Win+arrow缩放当前窗口
Alt+Tab/Win+Tab切换窗口
Alt+Esc在打开的窗口之间按首次打开的顺序切换(然后使用Win+arrow就可以放大选中的窗口了)
Ctrl+W关闭当前打开的一个页面
Alt+F4关闭当前窗口
Win+E打开文件资源管理器
Ctrl+Shift+Esc任务管理器(taskmgr命令)
Ctrl+T重新打开一个tab
Ctrl+N重新打开一个窗口(浏览器/文件资源管理器等等)
Ctrl+Shift+T重新打开已关闭的窗口或标签页
Alt+Space+Space -> Shitf+N最小化当前窗口

切换标签页
Ctrl+PageDown向左切换当前窗口的标签页
Ctrl+PageUp向右切换当前窗口的标签页

Win+Number
打开桌面任务栏中的应用(按排列顺序)
重复按键会最小化此应用窗口

```
___

> jetbrains系列(Pycharm、Idea、GoLand、CLion、WebStorm)

```

Ctrl+H显示当前类的层次结构
Ctrl+Shift+F10 编译并执行
Ctrl+上下箭头可以将当前的窗口的代码下移一位
Ctrl+Alt+m将代码抽取成方法(Extract method), 可以在减少函数行数(>50) 
Shift+Ctrl+A 搜索操作会显示快捷方式，方便我们查找
Shift+Shift查找任何内容
Ctrl+Alt+L reformat code 将当前文件格式化
Shift+Ctrl+Enter完整当前语句, 比如打印出for、if、switch、while、try会自动补全
Ctrl+Alt+T添加块: if, for, try...catch with
Ctrl+Ctrl 运行一些命令，比如打开一些编辑器,cmder等等, 它同时会显示最近的历史记录
Alt+F7找到使用的地方(find usage)
Ctrl+E查看最近打开的文件
Alt+number打开小工具窗口, 比如Git、Project、Problems等
Alt+Enter查看警告和建议
F2跳转到下一个错误、警告或建议
Ctrl-/Ctrl+折叠和展开代码
Ctrl+O重写方法,如重写超类toString或父类的方法
Ctrl+ALT+L快速格式化代码
Ctrl+D复制光标所在行或复制选中内容
Shift+F6进行重构
Alt + Insert 	自动生成代码
Ctrl + Alt + 空格 代码补全
Ctrl + Q 快速查看文档
Home 定位到行首
END定位到行尾
Ctrl+Home跳到文件头
Ctrl+End跳到文件尾部
选中一行 Home定位到行首 Shift+END选中一行 or END定位到行尾 Shift+HOME选中一行
Shift+arrow选中多行, 这样可以快速删除、剪切、复制
Alt+Shift+arrow可以将选中的多行或者当前所在的行进行移动(移动一行)
Ctrl+Shift+arrow移动整个方法
Win+Alt+左右arrow 切换tab 
Ctrl+F4关闭当前tab
Alt+上下arrow 在文件中进行方法上下跳跃
Ctrl+G跳转到指定行和列
Ctrl+Tab 切换标签页
Ctrl+Shift+u对选中单词切换大小写
Shift+Shift全局搜索
Ctrl+N搜索选中的当前类      
Ctrl+F12 搜索当前文件类的方法
Alt+7 展示类的Structure 可以看到类的属性和方法
Ctrl+Alt+< 回到上一步
CTRL+ALT+S 进入settings->plugins
Ctrl+Alt+> 回到下一步 
Ctrl+Alt+F全局搜索
Ctrl+展开代码
Ctrl-折叠代码
Ctrl+[移动光标到当前所在代码的花括号开始位置
Ctrl+]移动光标到当前所在代码的花括号结束位置
谁调用这个方法Ctrl+Alt+H
还原ctrl+z掉的内容ctrl+y
查看类或方法被引用  定位到要查看的类的文件ALT+F7
F2快速定位到报错的位置
Shift + F6 重命名
Alt+Insert，然后选中Constructor 快速生成构造方法
注释 // Ctrl + /     /**/ Ctrl + shift + /
单词跳跃 Ctrl + 左右arrow
批量替换选中的代码 Ctrl+R
在路径中替换(可替换不同文件夹中的内容) Ctrl + Shift + R
多行编辑 alt + mouse left/ alt + shift + mouse left      
如果需要在同一列中快速编辑, ALT+Shift+Insert进入多行编辑模式,Shift+arrow就可以出现多个光标

Alt+Insert 打开Generate面板, 包含Constructor/Getter/Setter/Getter and Setter/equals() and hashCode()/toString()/Override Methods/Delegate Methods/Copyright

idea设置序列化id:
File -> Settings -> Editor -> Inspections -> Java -> Serialization issues
选择serialization class without serialVersionUID
进入文本编辑器使用Alt+Enter选择Add serialVersionUID field选项
private static final long serialVersionUID = 1L;

idea快速自动生成junit测试类
1. 在要生成的测试类的类里面，按ctrl+shift+t->create new test
2. 将鼠标光标放到要生成测试类的类名或者方法名上面，按ctrl + enter –> create test 

pycharm and idea: 
查看类的层次结构(Hierarchy): 
选中一个类，或者打开一个类，然后使用快捷键Ctrl + H即可
查看层次关系图(Diagram): 
选中一个类，或者打开一个类，右键 Diagrams -> Show Diagram，或者使用快捷键Ctrl + Alt + U即可
如果继承图太多，也可以手动删除一些不必要的子图，便于我们更加清晰地观察我们想要的继承关系
当然，如果觉得文字或者结构图太小，按住Alt还能有放大镜功能
Ctrl+B跳转到定义处
Ctrl+Alt+B查看实现的类, 也可以跳转到方法实现处
CTRL+SHIFT+I 快速在当前文件中查看某一个类的内容
CPU: $(((Get-CimInstance -Namespace root/WMI -ClassName MSAcpi_ThermalZoneTemperature)[0].CurrentTemperature - 2731.5) / 10) C
CPU: $(((Get-CimInstance -Namespace root/WMI -ClassName MSAcpi_ThermalZoneTemperature)[0].CurrentTemperature - 2731.5) / 10) C
覆写方法之间的跳转
从子类覆写override的方法跳转到父类的方法或者从子类跳到父类 ctrl+u
从父类的方法跳转到子类复写的override的方法 ctrl+alt+b


文件操作：
Ctrl+Alt+Insert new file, 创建文件
Alt+Insert Generater, 生成Setter/Getter/构造器/toString等
Alt+字母 打开菜单栏上的对应字母开头的小标签

```
> intelij idea 
> 
```
Ctrl+Shift+Alt+C 保存引用路径,适合快速复制类名等的所在路径
Ctrl+E 查看最近使用的文件
Ctrl+W递进式选择代码块
Ctrl+Y删除光标所在行或删除选中的行
Ctrl+X剪切光标所在行或剪切选中内容
Ctrl+Delete删除光标后面的单词
Ctrl+Alt+Shift+C 在当前文件中Copy Rerference



Show Solution Windows:
Alt+F1 
查看项目所在位置
查看文件
打开导航栏
在资源管理器中打开当前文件所在目录

```
___

> cmder

```
新建tab ctrl+t(也可以新建浏览器tab)
关闭tab ctrl+w
关闭所有tab alt+f4
切换tab ctrl+tab or ctrl+n
查询历史命令 ctrl+r
打开超链接/文件 ctrl+mouse click
开启工具选项视窗 win+alt+p
全屏操作 alt+enter
任务栏全局召唤 Ctrl+`
打开系统菜单 win+alt+space


在文件管理器中打开目录:
start . 打开当前目录 
start命令的作用: 使用文件管理器打开指定的文件夹 
start命令后也可以跟windows中的shell系列命令:
start shell:startup
start shell:AppData
start shell:OneDrive
start shell:desktop
start shell:Personal
start shell:SendTo

在Windows文件管理器中打开目录:
# 使用 explorer.exe 打开当前目录
explorer.exe .

history命令可以显示搜索的历史:
默认history命令历史记录位置
bash: ~/.bash_history 
cmder: cmder\config\clink_history
```

> sublime 

```
Ctrl+PageDown向左切换当前窗口的标签页
Ctrl+PageUp向右切换当前窗口的标签页
Alt+number切换tab 
```

> chrome 

```
F11 全屏模式 再次按下退出全屏模式
Esc 停止加载页面或者从页面中下载
Ctrl+/Ctrl- 代替鼠标缩放页面

###### 打开窗口类
Ctrl+Shift+O打开chrome书签管理器便于快速搜索书签

Ctrl+Shift+B隐藏chrome标签栏

Ctrl+D用当前打开的页面添加书签

Ctrl+H 使用新的tab打开历史记录

Ctrl+J 查看chrome正在下载的内容

Ctrl+Shift+T 打开最后一次关闭的tab

F12/Ctrl Shitf J打开开发者工具中的控制台

Ctrl+L 选中当前tab的url 

Shift+Enter 先Ctrl+L, 复制当前的tab

CtrL+E 在当前标签中直接搜索
focuses on the address bar, search bar, or omnibox.


###### 切换到当前历史记录上一页和下一页
Alt+Left Arrow 回退历史记录
Alt+Right Arrow 前进历史记录


###### 切换tab 
左右切换tab
Ctrl+PageDown向左切换tab
Ctrl+PageUp向右切换tab
类似
Ctrl+Tab
Ctrl+Shift+Tab

使用数字精准切换tab
Alt+number切换tab

切换到最后的tab
Ctrl+9切换到最后的tab

Ctrl+Shift+Delete 清除浏览器记录


chrome://net-internals/#dns 清除chrome dns cache

标签页的刷新:
三种刷新方式分别是Normal Reload, Hard Reload, Empty Cache and Hard Reload.它们
的区别在于是否加载浏览器的缓存内容

1. 正常重新加载
f5, ctrl+r, 在地址栏回车，点击链接.如果缓存不过期，会使用缓存.
f5和ctrl+r都是普通刷新，若页面之前访问过，就会发一个空请求到
服务器，服务器返回302,表示资源未更新，可以使用浏览器缓存

2. 硬性重新加载/强制刷新
ctrl+shift+r/shitf+f5,强制浏览器重新下载并加载内容

3. Empty Cache and Hard Reload
完全清楚页面的缓存并重新下载所有内容

2和3的区别: 如果使用第二种方式刷新网页，虽然浏览器会强制的重新下载页面资源，但其可控的资源
只是刷新后首次显示的界面资源，对于一些触发后由js控制的动态页面，无法强制重新下载，触发js事件后
可能还是会从缓存中读取数据填充页面，因为此时已经脱离hard reload的作用范围.Empty Cache and Hard Reload则直接清空缓存，包括了js可能用到的资源，可以做到完全不从缓存中读取数据.

```
___


##### terminal amulator for windows

> [cmder](https://cmder.net/)
___

##### keymap documentation websites

> [idea](https://resources.jetbrains.com/storage/products/intellij-idea/docs/IntelliJIDEA_ReferenceCard.pdf)
> [pycharm](https://shortcutworld.com/PyCharm/win/JetBrains-PyCharm_Shortcuts)
> [vscode](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)
> [sublime text](https://www.sublimetext.com/docs/key_bindings.html)


##### vscode 
```
ctrl+j 隐藏或显示terminal
```