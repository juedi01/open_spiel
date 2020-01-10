#https://github.com/deepmind/open_spiel/blob/master/docs/install.md

./install.sh

virtualenv -p python3 venv

source venv/bin/activate

curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py

#Install pip deps as your user. Do not use the system's pip.

python3 get-pip.py --user

pip3 install --upgrade pip --user

pip3 install --upgrade setuptools testresources --user

pip3 install -r requirements.txt


#https://github.com/deepmind/open_spiel/issues/64

mkdir build && cd build

CXX=g++-9 cmake -DPython_TARGET_VERSION=3.7.2 -DCMAKE_CXX_COMPILER=${CXX} ../open_spiel

#the way to update g++-9:https://www.jianshu.com/p/7a8878397213

make -j12

ctest -j12

examples/example --game=tic_tac_toe


Setting Your PYTHONPATH environment variable
To be able to import the Python code (both the C++ binding pyspiel and the rest) from any location, you will need to add to your PYTHONPATH the root directory and the open_spiel directory.

When using a virtualenv, the following should be added to <virtualenv>/bin/activate. For a system-wide install, ddd it in your .bashrc or .profile.

#把下面语句粘贴到 <virtualenv>/bin/activate
# For the python modules in open_spiel.
export PYTHONPATH=$PYTHONPATH:/<path_to_open_spiel>
# For the Python bindings of Pyspiel
export PYTHONPATH=$PYTHONPATH:/<path_to_open_spiel>/build/python


----------------------------------------------------------------------------------------------------------------------------------
#the way to update g++-9:

1、用于加入源，方便更新。

sudo add-apt-repository ppa:ubuntu-toolchain-r/test
2、更新

sudo apt-get update
3、将/usr/bin/gcc和/usr/bin/g++这两个快捷方式给删除

    sudo update-alternatives --remove-all gcc
    sudo update-alternatives --remove-all g++
4、安装 g++ 和 gcc （以7版本为例）

   sudo apt-get install gcc-7
   sudo apt-get install g++-7
5、将gcc和g++绑定到新安装的版本上

    sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 20
    sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-7 20
6、检查是否安装成功

   gcc --version
   g++ --version
··········以上就能用了························

二、g++ gcc的版本切换

因为笔者安装了多个版本，在以上绑定后，版本仍显示4.9，而不是7.为此虚做版本切换：

如果你的Ubuntu中安装了多个版本的g++或者gcc，比如4.8 4.9 5.5等多个版本，想要切换时，打开新的终端，并输入

sudo update-alternatives --config g++

按照提示数字选择想要使用的版本。

即可选择g++版本，gcc同理，在终端中输入

sudo update-alternatives --config gcc
选择相应数字，即选择想要使用的版本。

三、linux g++开启C++11/14支持

sudo vim ~/.bashrc
在some more ls aliases注释块的地方添加下面这两行：

alias g++11='g++ -g -Wall -std=c++11'
alias g++14='g++ -g -Wall -std=c++14'

作者：阮明晨
链接：https://www.jianshu.com/p/7a8878397213
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

