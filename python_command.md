> 这是一些比较常用的命令, 大家可以复制后用typora做成pdf格式，方便快速查询
后续不定期更新


###### pip配置

爬虫配置代理 特殊字符需要用urlencode解码 
from urllib.parse import quote
http http://username:password@host:port/
https https://username:password@host:port/




documentation location:
https://pip.pypa.io/en/latest/cli/pip_config/
pip 缓存 windows位置:
C:\Users\username\AppData\Local\pip\cache
pip配置windows位置: 
%APPDATA%\pip\pip.ini
```%APPDATA%\pip\pip.ini
[global]
index-url = http://mirrors.tools.huawei.com/pypi/simple/

timeout = 120
[install]
trusted-host = mirrors.tools.huawei.com
```
内网中如果代理是：http://proxy.huawei.com
不要主动设置pip中设置huawei的代理
如需要设置代理
proxy = xxx

##### 常见的pip镜像源
官方: https://pypi.python.org/pypi 
清华: https://mirrors.tuna.tsinghua.edu.cn/
中科大: https://mirrors.ustc.edu.cn/   
阿里: https://opsx.alibaba.com/mirror  
腾讯: http://mirrors.cloud.tencent.com/pypi/simple/
豆瓣: http://pypi.douban.com/simple/（推荐）

可以使用pip install -i 镜像源 package来临时指定镜像源
也可以在pip.ini文件中指定镜像源

##### pip command

```
pip:
package installer for python. use pip to install packages
from the python package index and other indexes

setuptools: 
installing, upgrading, and uninstalling setuptools
```

pip install --upgrade pip
pip install --upgrade setuptools
pip list -o 查看现有的包那些需要被升级
pip show package 查看某个包的信息
pip show -f package 包信息附加所有的相关文件
pip install --upgrade package_name 更新某个包
pip install --upgrade pip 升级pip 
pip install matplotlib==3.4.1 安装指定版本的第三方的包
pip freeze > requirements.txt  生成依赖库文件
pip install -r requirements.txt 批量安装依赖库
pip install lxml -i http://mirrors.tools.huawei.com/pypi/simple/ --trusted-host mirrors.tools.huawei.com
pip check package_name 查看是否有版本冲突问题 如果不指定package_name，则会检查所有
pip download package_name -d "某个路径" 下载包到指定路径但不安装

##### 命令行模块的使用
* sqlmap
python sqlmap.py -u http://yanhongzhi.com/admin/dashboard  --proxy=https://username:password@host:port/

* httpie
http --verify=no https://www.python.org/ --proxy=http:http://username:password@host:port/ -v

##### python command
nohup python my.py >> my.log 2>&1 &
# 或者
nohup python my.py >> nohup.out 2>&1 &
# 或者
nohup python my.py &  # 这种写法和上面第二种写法等价


