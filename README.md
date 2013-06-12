# How to install Vim locally from source

This how-to details the steps needed to compile, build and install Vim from source into a user's home directory on a Debian derived distributions (Linux Mint, Ubuntu, etc).

**It is preferable to have a local Ruby installation to be able to link to it**

**All commands are performed from the user's home directory unless otherwise stated.**


## Build tools and libraries

Install the necessary tools and libraries by running the following command:

~~~ sh
$ sudo apt-get install build-essential bison mercurial libreadline-dev libncurses-dev libgtk-3-dev libgtk2.0-dev libxt-dev libx11-dev
~~~

## Create installation directory and add to PATH

The popular approach is to install under a `bin` directory in the user's home directory (e.g. `/home/yerv000/bin`). This approach is not recommended as it does not offer seperation of space which comes very useful to cleanly remove the local installation.

The following approach is more flexible and adds seperation of space.

Create a local directory to hold all locally installed programs if needed (Ruby, Node, Vim, etc):

~~~ sh
$ mkdir ~/local
~~~

Create a `vim` subdirectory to hold the Vim installation:

~~~ sh
$ mkdir ~/local/vim
~~~

Add the Vim binaries to the search path. Append the local Vim path to the `.profile` file present in the user's home directory:

~~~ sh
$ echo 'PATH=$HOME/local/vim/bin:$PATH' >> ~/.profile
~~~

Log out and log back in for the changes to take effect.


## Create source directory and clone Vim repository

Create a directory to hold source codes if needed:

~~~ sh
mkdir ~/src
~~~

Clone the Vim repository under the `~/src` directory:

~~~ sh
$ cd ~/src
hg clone https://vim.googlecode.com/hg/ vim
~~~

## Create shell script to build and install Vim

Create a file called `build_vim.sh` in the `~/src` directory using the following commands:

~~~ sh
$ cd ~/src
$ touch build_vim.sh
$ chmod +x build_vim.sh
~~~

Copy and paste the following content to `build_vim.sh` using your preferred editor:

```sh
cd ~/src/vim
hg pull
hg update
rm src/auto/config.cache
./configure --prefix=$HOME/local/vim --enable-gui=gtk2 --enable-rubyinterp=yes --with-features=huge
make clean
make
make install
make clean
```

## Run build script

Change current directory to `~/src`:

~~~ sh
$ cd ~/src
~~~

Run build script:

~~~ sh
$ ./build_vim.sh
~~~

## Verify Vim installation

Run the following command:

~~~ sh
$ which vim
~~~

Output should point to `~/local/vim/bin/vim`.

Verify installed Vim version:

~~~ sh
$ vim --version
~~~


## Deleting Vim installation

To completely delete the locally installed Vim version execute the following:

~~~ sh
$ rm -rf ~/local/vim/*
~~~

## Updating Vim

To completely the locally installed Vim version simply rerun the build script:

~~~ sh
$ ~/src/build_vim.sh
~~~
