腾讯Xlog的加解密功能不是必选项，如果不想使用加密模块或者嫌配置环境麻烦，直接执行命令：python [decode_mars_nocrypt_log_file.py](https://github.com/Tencent/mars/blob/master/mars/log/crypt/decode_mars_nocrypt_log_file.py) *.xlog脚本即可，这样的话只需要执行第1步操作（安装python，但是一定要是python2）

windows 解密腾讯xlog文件的步骤(实践可行)：根据自己环境是win32还是wind64下载对应版本

1、安装[Win64 python2.7.12](https://www.python.org/ftp/python/2.7.12/python-2.7.12.amd64.msi)，配置环境变量

2、安装[Win64 OpenSSL v1.1.1q](http://slproweb.com/download/Win64OpenSSL-1_1_1q.msi)，如果要尝试别的版本的话，官网下载地址[在这](http://slproweb.com/products/Win32OpenSSL.html)，但是亲测light版本的不行

3、下载[python setuptools 工具](https://pypi.python.org/packages/a9/23/720c7558ba6ad3e0f5ad01e0d6ea2288b486da32f053c73e259f7c392042/setuptools-36.0.1.zip#md5=430eb106788183eefe9f444a300007f0)，解压之后在终端进入到解压的当前目录中：使用命令：python setup.py install

4、下载[python Pip工具](https://pypi.python.org/packages/11/b6/abcb525026a4be042b486df43905d6893fb04f05aac21c32c638e939e447/pip-9.0.1.tar.gz.asc)，操作同上

5、下载[pyelliptic1.5.10](https://github.com/mfranciszkiewicz/pyelliptic/archive/1.5.10.tar.gz#egg=pyelliptic)，操作同上

6、环境配置好了，接下来只要拉取mars的项目，进入mars\log\crypt ，执行python gen_key.py，如果能生成成功则表示配置成功，如果不能请看6.1。python gen_key.py会生成private key 和public key,把pulic key作为Xlog代码初始化的appender_open 函数参数设置进去，private key存在安全的位置，防止泄露。并把这两个key设置到 mars\log\crypt 中[decode_mars_crypt_log_file.py](https://github.com/Tencent/mars/blob/master/mars/log/crypt/decode_mars_crypt_log_file.py)脚本中。

6.1、如果报错

File "build\bdist.win-amd64\egg\pyelliptic_init_.py", line 43, in

File "build\bdist.win-amd64\egg\pyelliptic\openssl.py", line 527, in

Exception: Couldn't load OpenSSL lib ...

根据代码判断，肯定是系统缺少了libeay32.dll这个文件，在[这里](https://github.com/Tencent/mars/files/2491092/libeay32dll.zip)下载完之后，把这个文件放到C:\Windows\System32里，有些64位系统需要放到C:\Windows\SysWOW64里，即可。

7、使用脚本的方法：直接执行命令：python [decode_mars_crypt_log_file.py](https://github.com/Tencent/mars/blob/master/mars/log/crypt/decode_mars_crypt_log_file.py) *.xlog，就会生成解压&解密后的文件
