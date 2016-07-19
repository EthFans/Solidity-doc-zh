###################

Installing Solidity 安装Solidity

###################

Browser-Solidity 基于浏览器的Solidity

================

If you just want to try Solidity for small contracts, you

can try browser-solidity <[https://chriseth.github.io/browser-solidity](https://chriseth.github.io/browser-solidity)>_

which does not need any installation. If you want to use it

without connection to the Internet, you can also just save the page

locally or clone http://github.com/chriseth/browser-solidity.

如果你只是想尝试一个使用Solidity的小合约，你不需要安装任何东西，只要访问 基于浏览器的Solidity<[https://chriseth.github.io/browser-solidity](https://chriseth.github.io/browser-solidity)>_

。如果你想离线使用，你可以保存页面到本地，或者从http://github.com/chriseth/browser-solidity 克隆一个。

NPM / node.js

=============

This is probably the most portable and most convenient way to install Solidity locally.

这可能安装Solidity到本地最轻便最省事的方法。

A platform-independent JavaScript library is provided by compiling the C++ source

into JavaScript using Emscripten for browser-solidity and there is also an NPM

package available.

在基于浏览器的Solidity上，Emscripten提供了一个跨平台JavaScript库，把C++源码编译为JavaScript，同时也提供NPM安装包。

To install it, simply use

去安装它就可以简单使用。，

::

    npm install solc

Details about the usage of the nodejs package can be found in the

repository <[https://github.com/chriseth/browser-solidity#nodejs-usage](https://github.com/chriseth/browser-solidity#nodejs-usage)>_.

如何使用nodejs包的详细信息可以在代码库 <[https://github.com/chriseth/browser-solidity#nodejs-usage](https://github.com/chriseth/browser-solidity#nodejs-usage)>_中找到.

Binary Packages 二进制安装包

===============

Binary packages of Solidity together with its IDE Mix are available through

the C++ bundle <[https://github.com/ethereum/webthree-umbrella/releases](https://github.com/ethereum/webthree-umbrella/releases)>_ of

Ethereum.

包括Mix IDE的二进制Solidity安装包在Ethereum网站C++ bundle <[https://github.com/ethereum/webthree-umbrella/releases](https://github.com/ethereum/webthree-umbrella/releases)>_ 中下载。 

Building from Source 从源码构建

====================

Building Solidity is quite similar on MacOS X, Ubuntu and probably other Unices.

This guide starts explaining how to install the dependencies for each platform

and then shows how to build Solidity itself.

在MacOS X、Ubuntu和其它类Unix系统中编译安装Solidity非常相似。这个指南开始讲解如何在每个平台下安装相关的依赖软件，然后构建Solidity。

MacOS X

-------

Requirements:

系统需求:

- OS X Yosemite (10.10.5)

- Homebrew

- Xcode

Set up Homebrew:

安装Homebrew：

.. code-block:: bash

    brew update

    brew install boost --c++11             # this takes a while 这需要等待一段时间

    brew install cmake cryptopp miniupnpc leveldb gmp libmicrohttpd libjson-rpc-cpp 

    # For Mix IDE and Alethzero only 仅仅安装Mix IDE和Alethzero

    brew install xz d-bus

    brew install llvm --HEAD --with-clang 

    brew install qt5 --with-d-bus          # add --verbose if long waits with a stale screen drive you crazy as well 如果长时间的等待让你发疯，那么添加--verbose输出信息会让你感觉更好。

Ubuntu

------

Below are the build instructions for the latest versions of Ubuntu. The best

supported platform as of December 2014 is Ubuntu 14.04, 64 bit, with at least 2

GB RAM. All our tests are done with this version. Community contributions for

other versions are welcome!

下面是在最新版Ubuntu系统上编译安装Solidity的指南。最佳的支持平台是2014年11月发布的64位Ubuntu 14.04，至少需要2GB内存。我们所有的测试都是基于此版本，当然我们也欢迎其它版本的测试贡献者。

Install dependencies:

安装依赖软件：

Before you can build the source, you need several tools and dependencies for the application to get started.

在你从源码编译之前，你需要准备一些工具和依赖软件。

First, update your repositories. Not all packages are provided in the main

Ubuntu repository, those you'll get from the Ethereum PPA and the LLVM archive.

首先，升级你的代码库。Ubuntu主代码库不提供所有的包，你需要从Ethereum PPA和LLVM获取。

.. note::

    Ubuntu 14.04 users, you'll need the latest version of cmake. For this, use:

    sudo apt-add-repository ppa:george-edison55/cmake-3.x

Ubuntu 14.04的用户需要使用：  sudo apt-add-repository ppa:george-edison55/cmake-3.x获取最新版本的cmake。

Now add all the rest:

现在加入其它的包：

.. code-block:: bash

    sudo apt-get -y update

    sudo apt-get -y install language-pack-en-base

    sudo dpkg-reconfigure locales

    sudo apt-get -y install software-properties-common

    sudo add-apt-repository -y ppa:ethereum/ethereum

    sudo add-apt-repository -y ppa:ethereum/ethereum-dev

    sudo apt-get -y update

    sudo apt-get -y upgrade

Use the following command to add the develop packages:

使用下面的命令获取开发相关的包。

.. code-block:: bash

    sudo apt-get -y install build-essential git cmake libboost-all-dev libgmp-dev libleveldb-dev libminiupnpc-dev libreadline-dev libncurses5-dev libcurl4-openssl-dev libcryptopp-dev libjson-rpc-cpp-dev libmicrohttpd-dev libjsoncpp-dev libedit-dev libz-dev

Building 编译

--------

Run this if you plan on installing Solidity only, ignore errors at the end as

they relate only to Alethzero and Mix

如果你只准备安装solidity，忽略末尾Alethzero和Mix的错误。

.. code-block:: bash

    git clone --recursive https://github.com/ethereum/webthree-umbrella.git

    cd webthree-umbrella

    ./webthree-helpers/scripts/ethupdate.sh --no-push --simple-pull --project solidity # update Solidity repo

    ./webthree-helpers/scripts/ethbuild.sh --no-git --project solidity --all --cores 4 -DEVMJIT=0 # build Solidity and others

                                                                                #enabling DEVMJIT on OS X will not build

                                                                                #feel free to enable it on Linux 

If you opted to install Alethzero and Mix:

如果你选择安装Alethzero和Mix：

.. code-block:: bash

    git clone --recursive https://github.com/ethereum/webthree-umbrella.git

    cd webthree-umbrella && mkdir -p build && cd build

    cmake ..

If you want to help developing Solidity,

you should fork Solidity and add your personal fork as a second remote:

如果你想帮助Solidity的开发，你需要分支（fork）Solidity并添加到你的私人远端分支：

.. code-block:: bash

    cd webthree-umbrella/solidity

    git remote add personal git@github.com:username/solidity.git

Note that webthree-umbrella uses submodules, so solidity is its own git

repository, but its settings are not stored in .git/config, but in

webthree-umbrella/.git/modules/solidity/config.

注意webthree-umbrella使用的子模块,所以solidity是其自己的git代码库，但是他的设置不是保存在 .git/config, 而是在webthree-umbrella/.git/modules/solidity/config.
