---
layout: post
title: Hide Files from Git without Using .gitignore
---

The last great unexplored region of git is the .git directory. Once you start to dig, however, you'll find some interesting trinkets. One I've discovered lately is the 'exclude' file, which lets you hide files from git without anyone knowing about it.

## Where does he get those wonderful toys?

If you navigate into your .git directory, then into the info directory, you'll see a file called 'exclude'. If you open it up, you'll see something like this:

~~~ruby
# git ls-files --others --exclude-from=.git/info/exclude
# Lines that start with '#' are comments.
# For a project mostly in C, the following would be a good set of
# exclude patterns (uncomment them if you want to use them):
# *.[oa]
# *~
~~~

That should look familiar, like, say, a .gitignore. It works pretty much like a .gitignore too

~~~ruby
# git ls-files --others --exclude-from=.git/info/exclude
# Lines that start with '#' are comments.
# For a project mostly in C, the following would be a good set of
# exclude patterns (uncomment them if you want to use them):
# *.[oa]
# *~
.ruby-version
.ruby-gemset
~~~

## That's fantastic! Why would I want to do that? Um, .gitignore?

While this may seem like something you can already do more easily, using the exclude file does have some advantages. If you don't have control of the .gitignore file, like in a large OSS project, this method will exclude anything you'd like. The exclude file can also seperate project-specific omissions from developer-specific omissions -- if developers are using using tools or editors that no one else is using, you can exclude files without turning your .gitignore into a novel. I found this when working with a project that didn't use rvm. I wanted to use rvm, but since no one else was, I didn't want pollute the .gitignore with something that no one else cared about. If you're on a project that has several editors in use, you can easily remove omitting the files as a project concern by letting each developer add the necessary files to his/her exclude file. 

Be aware, if you blow away your repository, your exclude file will go with it, since it's not committed like a .gitignore.  

While this won't ever replace the .gitignore file, you can never have too many tools in your toolbox. Happy hiding.

[before]: http://michaeljosephkramer.com/images/git-exclude.png
[after]: http://michaeljosephkramer.com/images/git-exclude-content.png
