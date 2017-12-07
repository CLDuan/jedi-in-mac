# jedi-in-mac

this repository records the issues I met during jedi-vim plugin installation.

1. pip install jedi
2. vim .vimrc, add "Plugin 'davidhalter/jedi-vim'", and run ":PluginInstall" in vim

things looks fine after above operations, but when open a test.py file with vim, error message appears, saying cannot import jedi successfully because another module cannot be imported, as something cannot be found which is a function named _remove_dead_weakdef.

firstly, after google the issue about "jedi-vim cannot work as jedi cannot be found", there indeed were several posts talking about how to install jedi correctly. but those were not my case. but still, there was someone pointed out a very important clue, which directed me to look into the python envrionment inside vim and outside vim.

outside vim:
$python -c "import sys; print sys.version; print sys.path"

inside vim:
:python import sys; print sys.version; print sys.path

look into the output of above two operations, the sys.version is apparently different, while vim-python version is 2.7.10 and shell-python is 2.7.13. but the path is the same, something like /Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/..., oh, right, my laptop is MacBook Air, with macOS High Sierra 10.13.1.

another interesting thing is that "import jedi" in shell-python works fine while it tells error in vim-python. this is important, which means the shell-python and the vim-python is different at all!

so, the conclusion is that not only the python execuables used by shell and vim are different, and the path used by shell-python and vim-python are also different. furthermore, the sys.path does not tell you the truth. still don't know the reason yet...

then, I use "locate" command to find all python related stuff in the whole system, and found there are two python in macOS, the first one is installed along side the system, located at /System/Library/Frameworks/Python.framework/Versions/2.7/..., and another one is installed by 3rd party application, located at /Library/Frameworks/Python.framework/Versions/2.7/....

since vim-python's version is 2.7.10, lower than the latest version, so the final resolution is:
1. brew install python
2. brew install vim
3. pip install jedi
4. install jedi-vim using vundle
5. maybe, cd ~/.vim/bundle/jedi-vim, git submodule update --init

there are still some other tiny issues should be noted. 
you can try "brew upgrade" before "brew install" when you already have that application. 
and remember after install python, you should "source .bash_profile" to find the just installed python executable, instead of the original one. 

this whole jedi-vim plugin envrionment issue took me about 6 hours to work out, but it indeed help me to understand a lot of vim / python / system knowledge anyway. it worths.
