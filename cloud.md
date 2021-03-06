## shell

```shell
ps -ef|grep name
lsof -i :8888
kill -9 pid
```



## yum

```shell
yum install -y tree
yum -y install zlib zlib-devel bzip2 bzip2-devel ncurses ncurses-devel readline readline-devel openssl openssl-devel openssl-static xz lzma xz-devel sqlite sqlite-devel gdbm gdbm-devel tk tk-devel
yum -y install gcc+ gcc-c++
```

## anaconda

```shell
wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-2020.11-Linux-x86_64.sh
wget https://repo.anaconda.com/archive/Anaconda3-2020.11-Linux-x86_64.sh

vi /etc/profile
PATH=$PATH:/root/anaconda3/bin 
export PATH

pip install pip -U
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple

conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/bioconda/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/menpo/

conda config --set show_channel_urls yes
conda config --remove-key channels

conda create -n pytorch  python=3.8
pip install ipykernel
python -m ipykernel install --user --name=name

jupyter kernelspec list
jupyter ubinstall kernelname

pip install jupyter_contrib_nbextensions
jupyter contrib nbextension install --user


without systemd, run /root/.local/share/kite/kited
```

## nodejs

```shell
export NODE_MIRROR=https://mirrors.tuna.tsinghua.edu.cn/nodejs-release/
yum install nodejs -y
npm install -g cnpm --registry=https://registry.npm.taobao.org
npm i -g npm
n stable
```



## 安装数据库

```shell
yum install -y mariadb mariadb-server

systemctl start mariadb
systemctl enable mariadb-service

mysql_secure_installation

mysql-uroot -p



/*
yum install mysql-devel gcc gcc-devel python-devel
pip install mysqlclient
yum install MySQL-python
*/
```

## 云服务器上的Django

### 安装django

```shell
pip install django

 ln -s ~/anaconda/bin/django /usr/bin
 
 django-admin startproject mysite
```

### 运行Django

```shell
python3 manage.py migrate
python3 manage.py runserver 0.0.0.0:80
```

### uwsgi nginx

```
pip3 install uwsgi
ln -s  ~/anaconda3/bin/uwsgi /usr/bin/uwsgi3
yum install nginx
```



