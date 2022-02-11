### Permanently set `$PATH` variable in Mac `bash` shell

- Open Terminal
- Make sure you are on Zsh Shell, if not type `bash` and hit enter
- Now type `vi ~/.bash_profile`
- Press `i` on keyboard to enter insert mode
- At the end of file, add the line containing your $PATH variable. Example: `export PATH="$PATH:/opt/homebrew/bin/"`
- Press `esc` to exit insert mode. Then enter `:wq!` to modify the file (override pemission limits)
- Now type command `source ~/.bash_profile` to apply changes

### Permanently set `$PATH` variable in Mac `zsh` shell

Original post can be found [here](https://code2care.org/pages/permanently-set-path-variable-in-mac-zsh-shell).

- Open Terminal
- Make sure you are on Zsh Shell, if not type `zsh` and hit enter
- Now type `nano ~/.zshrc`
- Now add your $PATH variable details. Example: `export PATH="$PATH:/opt/homebrew/bin/"`
- Press `Control + X`, followed by `Y` to save the file, press enter to exit Nano
- Now type command `source ~/.zshrc` to apply changes
