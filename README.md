# tmux_conf
tmux configure file 

# Install without Root

Refer to 
1. [https://superuser.com/questions/1259140/how-to-install-tmux-locally-without-root-access](https://superuser.com/questions/1259140/how-to-install-tmux-locally-without-root-access)
2. [http://jhshi.me/2016/07/08/installing-tmux-from-source-non-root/index.html#.XOhad1SJLcQ](http://jhshi.me/2016/07/08/installing-tmux-from-source-non-root/index.html#.XOhad1SJLcQ)
3. [https://gist.github.com/ryin/3106801](https://gist.github.com/ryin/3106801)

## Tmux
[Tmux](https://github.com/tmux/tmux)
## Dependent Libraries
1. [Ncurses](https://www.gnu.org/software/ncurses/)
2. [libevent](https://libevent.org/)

## Basic Commands
Suppose the installation path is `${HOME}/local`. Some basic routine for install Ncurses, libevent, and Tmux are as follows.
Note that Ncurses and Tmux need more configurations. Please check them below.
```
tar zxvf xxx.tar.gz
cd xxx
./configure --prefix=${HOME}/local
make && make install
```

## Some Notes

1. For Ncurses 6.1, some extra configure flags are needed to support screen-256color. Otherwise I got `tput: unknown terminal 'xxx'` error when I used zsh as the shell. I'm not sure which one is exactly for the screen-256color.
```
export CXXFLAGS="-fPIC"
export CFLAGS="-fPIC"
./configure --prefix=${HOME}/local --enable-ext-colors --enable-sp-funcs --enable-term-driver --enable-shared
```
After adding the configuration and `export TERM="screen-256color"` at the first line of `.zsh` file, the tput error disappers.
`--enable-shared` and those two `export` is for `zsh` installation.

2. For Tmux, some compile options are needed.
```
./configure --prefix=${HOME}/local CFLAGS="-I${HOME}/local/include -I${HOME}/local/include/ncurses" LDFLAGS="-L${HOME}/local/lib"
```

3. Update the `LD_LIBRARY_PATH`.
```
export LD_LIBRARY_PATH="${HOME}/local/lib:${LD_LIBRARY_PATH}"
```

### Common command
```
# change to the app's directory
cd app/
mkdir build
cd build

```
### ncurses
```
../configure --prefix=${HOME}/local \
	CXXFLAGS="-I$HOME/local/include -I$HOME/local/include/ncurses" \
	CFLAGS="-I$HOME/local/include -I$HOME/local/include/ncurses" \
	LDFLAGS="-L$HOME/local/lib"

make clean 
make -j && make install
```

### libevent
```
../configure --prefix=${HOME}/local
make clean -j
make -j && make install
```

### Zsh
```
LOCAL_PATH="/home/zhenpeng/local"
#export PATH=$LOCAL_PATH/bin/:$PATH
export LD_LIBRARY_PATH=$LOCAL_PATH/lib:$LD_LIBRARY_PATH
export CFLAGS="-I$LOCAL_PATH/include -fPIC"
export CXXFLAGS="-I$LOCAL_PATH/include -fPIC"
export LDFLAGS="-L${LOCAL_PATH}/lib"

../configure --prefix=${LOCAL_PATH} --enable-shared
make clean -j
make -j && make install
```

### tmux
```
../configure --prefix=${HOME}/local \
	CXXFLAGS="-I$HOME/local/include -I$HOME/local/include/ncurses" \
	CFLAGS="-I$HOME/local/include -I$HOME/local/include/ncurses" \
	LDFLAGS="-L$HOME/local/lib"

make clean 
make -j && make install
```
