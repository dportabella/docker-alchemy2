# Instructions on how to compile Alchemy on Ubuntu7, instead of using a Docker image

[Alchemy2](http://alchemy.cs.washington.edu/) MLN software (`learnstruct`, `learnwts` and `infer`):


```
sudo apt-get install gcc-4.9

mkdir ~/bin
cd ~/bin

wget http://ftp.gnu.org/gnu/bison/bison-2.3.tar.gz
tar -xvzf bison-2.3.tar.gz
cd bison-2.3
./configure
make
sudo make install

cd ~/bin
wget "http://downloads.sourceforge.net/project/flex/flex/2.5.4.a/flex-2.5.4a.tar.gz"
tar -xvzf flex-2.5.4a.tar.gz
cd flex-2.5.4
./configure
make
sudo make install

export PATH=/usr/local/bin:$PATH

cd ~/bin
wget "https://storage.googleapis.com/google-code-archive-downloads/v2/code.google.com/alchemy-2/alchemy-2.tar.gz"
tar -xvzf alchemy-2.tar.gz
cd alchemy-2
cd src
make

cd ..
ls bin/
infer  learnstruct  learnwts  liftedinfer  obj  runliftedinfertests

cd
export PATH=~/bin/alchemy-2/bin:$PATH

# run it ...
infer
```

About Alchemy:

- http://alchemy.cs.washington.edu/
- http://alchemy.cs.washington.edu/tutorial/tutorial.pdf
- https://alchemy.cs.washington.edu/mlns/tutorial/

Disclaimer: I am not affiliated to the authors of the [Alchemy software](http://alchemy.cs.washington.edu/).

