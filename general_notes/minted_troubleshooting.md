### Background

The pakage `pygmentize` has been installed using `pip install pygmentize` command and was verified using `which pygmentize` in terminal.

This message was returned after verification: 
```shell
/opt/anaconda3/bin/pygmentize 
```

The package `Pygments` has been installed using `sudo easy_install Pygments` command.

The environment PATH, which was checked using `echo $PATH` in the terminal, returned this message:
```shell
/opt/anaconda3/bin:/opt/anaconda3/condabin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/Library/TeX/texbin:
/usr/local/aria2/bin:/opt/X11/bin
```

When the package `minted` was used in LaTeX, an error message prompted that `...  you must have pygmentize installed ...`.

This was perplexing since the installation of `pygmentize` had been verified.

### Solution

The [original post](https://tex.stackexchange.com/questions/159620/cannot-get-pygmentize-to-work-correctly-on-mac) is from tex.stackexchange.

> Recently I had the same problem. The issue was that TeXworks has different PATH environment from your terminal. My solution was to create a symbolic link to pygmentize from `/Library/Frameworks/Python.framework/Versions/3.5/bin/pygmentize` to `/usr/local/bin`.

Following this, I have executed this in the terminal:
```shell
ln -s /opt/anaconda3/bin/pygmentize /usr/local/bin
```
This has created a symbolic link to the pygmentize package in the `/usr/local/bin` directory.

Also I manually deleted all temporary files in the LaTeX project folder and recompiled. 

Then everything was working as expected. Problem solved.
