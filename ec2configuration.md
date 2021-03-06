Configuring packages for python / mysql / mysqldb on an Amazon EC2 instance

I got tired of looking up the same five blogs and seven SO posts in order to configure a new EC2 instance with the python-based tools I use to get work done. This is a walkthrough on getting the following packages running:

* pip
* python
* virtualenv
* gcc
* mysql
* mysqldb

I also usually install ipython, pandas, numpy, scipy, pycolor, and other interesting tools, but none of those are as critical to my functionality as the above

I hope this saves someone else some time, as well as being useful for me in the future!


Ready, set, go:

Get pip for your ec2 instance -- do this in your parent installation directory (mine is usr/local/bin)

```
[sudo] wget http://pypi.python.org/packages/source/p/pip/pip-1.1.tar.gz#md5=62a9f08dd5dc69d76734568a6c040508
[sudo] tar -xvf pip*.gz
cd pip*
sudo python setup.py install
[sudo] rm pip-1.1.tar.gz 
```

Make sure you have downloaded the right copy of python for your venv; you do not have to designate this as the copy in your main $PATH

```
sudo yum install python27-devel
```

Get virtualenv to handle making virtual environments

```
sudo pip install virtualenv
```

In the target folder, make a virtual environment (called "venv" in this case) with a specified version of python

```
virtualenv -p /usr/bin/python2.7 venv
```
Activate your virtual environment and check the python version

```
cd venv
. bin/activate
which python
python --version
```
MySQLdb uses C to link to MySQL, so you need a C compiler

```
yum install gcc
```

Get the development version of MySQL -- it has the right config files in place for MySQLdb

```
yum install mysql-devel
```

Finally, install MySQLdb
```
pip install MySQL-python
```

Check to make sure that MySQLdb imports correctly
```
python
import MySQLdb AS mysql
```

Matplotlib requires some non-python packages to be installed solo, before the main enchilada
```
sudo yum install libpng-devel
sudo yum install freetype-devel
sudo pip install matplotlib
```

Don't forget to add your new ssh connection to the file in your local ~/.ssh/config file as a known host for ease of access

```
Host bar-analytics
 HostName foo.amazonaws.com
 IdentityFile ~/.ssh/id_rsa
 User me
```

If you want scipy, first you need a fortran compiler

```
sudo apt-get install gfortran
sudo pip install scipy
```


