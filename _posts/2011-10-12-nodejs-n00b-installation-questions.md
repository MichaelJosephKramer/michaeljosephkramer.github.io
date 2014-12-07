---
layout: post
title: "Node.js: N00b Installation Questions"
---

The first time I tried to install Node.js, I was hoping to click something and check back in a few minutes. I quickly found, however, that it's not quite that simple. Node.js has many different installation options,

and while I wasn't sure what I was getting into at first, I found a combination that works for me.

Thankfully, I was lucky enough to find this gist early on: [https://gist.github.com/579814][gist]

If you said 'Yikes!', or something worse, when you first saw it you're not alone. I don't claim to be the most experienced POSIX OS user but I have *some* experience, and I found that list to be a little overwhelming. After I answered a few questions and learned a few things about what I was actually seeing, however, it was a little less mystical.

Here are a few questions I had when I got started:

## Should I compile Node.js myself, or use a package manager?

### It won't matter that much once you install a version manager.

I've done both, and compiling Node.js yourself isn't as hard as it sounds, even if you're not familiar with make, etc. Installing it from a package manager, however, is even easier. While a new Node.js release comes every couple of weeks, and you'll see plenty of recommendations to compile it yourself so you can just `git pull` it, you can quickly install newer versions and switch between them using a version manager like n, npm, or nave.

For example, to install the latest version of Node.js and switch to it with n, it's just:

`n latest`

n is my current favorite version manager because it respects npm's globals when you switch versions, although I'm sure some don't use it for that same reason. On the downside, n is not fun to Google if you have a question.

Speaking of npm.  .  .

## Should I install npm at the same time?

### Yes.

npm stands for 'node package manager', and that's exactly what it is. You won't do much with Node.js before you'll need to install it.

Since Node.js is a relatively new platform, the ecosystem hasn't really declared too many winners or losers yet. As far as de facto standards, npm is about as close as you're going to get right now while working with Node.js. After I spent a little time working with npm, I understand the reasons. The commands are simple and intuitive, and if you're familiar with package managers for other platforms, like RubyGems, it won't take you long to feel comfortable.

## To chown or not to chown?

### Um, I chowned, but it seems to be a Coke or Pepsi type of question.

The installation methods in the [gist][gist] fall into two camps: some install into the home directory and some install into the `/usr/local` directory. To install into the `/usr/local` directory and work with Node.js, you either have to take ownership of that directory with the `chown` command or use `sudo` when executing commands.

As I criss-crossed the internet to find a simple answer for the "right" way to install it, I found I was in a deeper rabbit hole than I thought.

The internet rarely agrees on diverging viewpoints, but nobody seems to think using `sudo` all the time is a good idea. Executing package manager commands with `sudo` allows the scripts to do *anything* to any part of your machine. Anything malicious, or even unexpected, could cause major problems.

A better solution is to `chown` the directories, and take ownership of them with your account. After you do, you'll be able to install and run scripts that install software in that directory that you now own. While this does allow you install software there, it doesn't allow naughty scripts to pollute any other part of your system, but just the directories of which you took ownership.

This solution was not overly popular with the internet either. Justification varied from "it's a security risk" to "It's not what you should do", and then to "You'll anger the UNIX gods!". Other arguments seemed to to worry less, stating that `/usr/local` was meant to work with package managers and the home directory was meant to work with compiled software. But wait, can't we install Node.js either way? My head hurts. Generally, it seemed most Ubuntu people said no and OS X people were split.

Although the arguments seemed esoteric at times, it seems like installing it in the home directory is safer.

So why did I chown? I didn't have to because I'd *already* taken ownership of my `/usr/local` directory on my MacBook Pro because that's the recommended way to install [Homebrew][homebrew].

Some other reasons I didn't feel too badly about it:

1. My machine is solely a development machine, not a server. It can be a little hosed as long as it works. I'd worry I wasn't trying hard enough it my development machine was perfect.
2. If I do totally screw up my machine, all my important files are saved somewhere else, like Dropbox or Github. Reinstalling my OS is a minor annoyance, not a disaster.
3. I'm not that nice to my home directory. Copy this, delete that, etc. That leads me to accidentally delete something I want to keep out of my home directory about every other day, so I'm happy to have Node.js and npm largely out of the way.

If this sounds stupid or lazy to you, you might be right -- please feel free to call me wrong on the internet. While it might not be sound advice, it's worked for me.

### There's no Homebrew on Linux. Did you do it the same way on OS X and Ubuntu?

Yup. You'll want to install some npm packages globally, and for that to work you'll need to add the NODE_PATH variable to your .profile, .bashrc, or .zshrc. I like to use the same dotfiles for both my MacBook Pro and my Ubuntu machines, so I set it up the same way. When you install npm, it'll clearly tell you what you need to add to your shell's dotfile.

## Umm, thanks?

When I installed Node.js, I just wanted to start writing some code. While I do appreciate all the installation options now that I understand them a little better, I spent too much time looking at Ubuntu forum posts to find out if my permissions were wrong. So if this distilled version gets you coding faster, then my work here is done.

Happy Coding!

*Update: I originally said `/usr/bin` a couple of times when I meant `/usr/local`. I've made the correction. Sorry for any confusion.*

[gist]: https://gist.github.com/579814 "the installation gist"
[homebrew]: https://github.com/mxcl/homebrew/wiki/FAQ "Homebrew"
