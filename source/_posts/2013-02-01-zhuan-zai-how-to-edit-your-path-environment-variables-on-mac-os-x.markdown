---
layout: post
title: "(转载)How to edit your path environment variables on Mac OS X"
date: 2013-02-01 22:38
comments: true
categories: Mac相关
tags: [mac]
---
原文链接：  
[http://hathaway.cc/2008/06/how-to-edit-your-path-environment-variables-on-mac-os-x/](http://hathaway.cc/2008/06/how-to-edit-your-path-environment-variables-on-mac-os-x/)  
If you are new to Mac OS X, you may need to know how to edit your PATH.  
The good news is that this is an easy task on Mac OS X.

The recommended way is by editing your `.bash_profile` file.   
This file is read and the commands in it executed by Bash every time you log in to the system.   
The best part is that this file is specific to your user   
so you won’t affect other users on the same system by changing it.

###Step 1: Open up a Terminal window(this is in your Applications/Utilites folder by default)

###Step 2: Enter the follow commands:
`touch ~/.bash_profile; open ~/.bash_profile`  
This will open the `.bash_profile` file in Text Edit (the default text editor included on your system).   
The file allows you to customize the environment your user runs in.

###Step 3: Add the following line to the end of the file adding whatever additional directory you want in your path:
`export PATH="$HOME/.rbenv/bin:$PATH"`  
That example would add `~/.rbenv` to the PATH. The `$PATH` part is important,  
as it appends the existing PATH to preserve it in the new value.  

###Step 4: Save the .bash\_profile file and Quit (Command + Q) Text Edit.

###Step 5: Force the .bash\_profile to execute.   
This loads the values immediately without having to reboot.   
In your Terminal window, run the following command.
`source ~/.bash_profile`  
That’s it!   

Now you know how to edit the PATH on your Mac OS X computer system.   
You can confirm the new path by opening a new Terminal windows and running:  
`echo $PATH`  

You should now see the values you want in your PATH.

[Updated 2/10/2012]  
The instructions now use the `.bash_profile` method of editing your PATH.  
This is preferred as it keeps the changes specific to your user.   
I also updated the instructions to use `TextEdit` instead of `vim` so it is easier for a beginner.
