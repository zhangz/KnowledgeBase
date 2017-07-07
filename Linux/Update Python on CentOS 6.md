## 在CentOS 6.X 上面安装 Python 2.7.x
#### 更新系统和开发工具集
```
# Compilers and related tools:
yum groupinstall -y "development tools"
# Libraries needed during compilation to enable all features of Python:
yum install -y zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel expat-devel
# If you are on a clean "minimal" install of CentOS you also need the wget tool:
yum install -y wget
```

#### 源码安装Python 2.7.x
```
wget https://www.python.org/ftp/python/2.7.13/Python-2.7.13.tar.xz
tar -xvf Python-2.7.13.tar.xz
cd Python-2.7.13
# enable UTF-32 in Python 2.7 by passing --enable-unicode=ucs4 to the configure command.
./configure --enable-shared --enable-unicode=ucs4
make
make install
make clean
make distclean
```

#### 设置 PATH
```
#备份老python
mv /usr/bin/python /usr/bin/python2.6.6
#建立软连接
ln -s /usr/local/bin/python2.7 /usr/bin/python
```

#### 修复 yum
升级python以后yum的使用可能会受到影响，需要进行一下设置
```
vi /usr/bin/yum
```
找到第一行`#!/usr/bin/python` 改为 `#!/usr/bin/python2.6.6`。

#### 升级 pip, setuptools
```
pip install -U pip setuptools
```

#### 离线安装pip
- Install pip offline via https://github.com/pypa/pip/issues/2351
- Download [get-pip.py](https://pip.pypa.io/en/latest/installing/), pip-9.0.1-py2.py3-none-any.whl, wheel-0.30.0a0-py2.py3-none-any.whl
- place pip and wheels in . (or another local path). 
- `python get-pip.py --no-index --find-links=.`

可能需要建pip软链接
```
mv /usr/bin/pip /usr/bin/pip.bak
ln -s /usr/local/bin/pip /usr/bin/pip
```

#### 安装 setuptools
```
#获取软件包
wget --no-check-certificate https://pypi.python.org/packages/source/s/setuptools/setuptools-1.4.2.tar.gz
# 解压:
tar -xvf setuptools-1.4.2.tar.gz
cd setuptools-1.4.2
# 使用 Python 2.7.13 安装 setuptools
python2.7 setup.py install
```

## Reference
[How to install the latest version of Python on CentOS](https://danieleriksson.net/2017/02/08/how-to-install-latest-python-on-centos/)