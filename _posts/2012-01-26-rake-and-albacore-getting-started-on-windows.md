---
layout: post
title: 'Rake and Albacore: Getting Started on Windows'
---

There exists two types of developers: those who have written Ruby and those who would like to punch those who have written Ruby in the face.

If you're a .NET developer and have never written Ruby, I might be able to convert you from the latter to the former. Writing an entire application in Ruby might not be feasible in your organization, especially if you work in a .NET shop, but writing your build script in Ruby an easier battle if you're picking your fights. Thankfully, a gem called Albacore can make build scripts a breeze with just a little bit of code.

## What's Albacore? Wait, what's a gem? And what's this "Rake" you're about to mention?
We'll start with gems, and if you're familiar with NuGet, it's the same deal -- code libraries that you can install via the internet. [Albacore][albacore] is a gem, and it's a collection of Rake tasks that automate building a .NET application. And Rake is a Ruby task manager that makes running simple scripts simple, and adds in some functionality like prerequisites, defaults, and namespacing. Thankfully, you don't need to be an expert in Rake or the gem system to be able to use start building .NET apps with Albacore.

## Installation
1. Just use the [Rails Installer][railsinstaller]. Even though you're not using Ruby on Rails, this exe installs everything you need and more. The site has great documentation about what you're getting youreself into, and even has a screencast.
2. Install Albacore. Open a command prompt and type:

    <code>gem install albacore</code>
3. Profits!

## Got an example?
Yeah. It's [here][example] on GitHub. If you've never used Git before, you already have it installed -- it came with the rails installer. The [instructions][gitsetup] will walk you through it, but you can skip the "First: Download and Install Git" because you've already done that, and move straight to "Next: Set Up SSH Keys". After that, just pull down the example by cloning the repository from the command line:

```git clone git@github.com:MichaelJosephKramer/Albacore-Example.git```

To run all the examples, you'll need to install one other gem called Nokogiri. Nokogiri is a ruby-based XML processor.
    gem install nokogiri

The example contains four pretty task that are most likely common on most .NET applications. Here's the "build" task in the rakefile:

{% highlight ruby linenos %}
desc "Build the solution in .Net 4.0"
msbuild :build do |msb|
  msb.properties :configuration => :Debug
  msb.targets :Clean, :Build
  msb.verbosity = 'quiet'
  msb.solution = "FizzBuzz.sln"
end
{% endhighlight %}

To run this task, just type <code>rake build</code> in the directory with the rakefile. Any given directory can only contain one rakefile, usually named "rakefile" with no extension, although the file name is [flexible][casing].

- The first line is a description that you can see by issuing a <code>rake -T</code> at the command line to view the available tasks in the rakefile. This line isn't strictly necessary, but helpful. 
- The next line has the Albacore task type (msbuild) and the arbitrary task name (:build). 
- The next few lines are simply setting options, and if you're familiar with the MSBuild options then you'll see that they're the same. 
- The line before the end is just a path to the solution file.

The file also contains tasks that build the application in a release configuration, another that runs the unit tests, and one that uses Nokogiri to update the connection string in the config file.

## Why would I want to do this again?
Did you notice there's no XML?
 
Many traditional .NET build tools, like MSBuild and NAnt, are XML based. XML based build tools generally have several problems:

1. Your file is filled with XML overhead crap.
2. It's often harder to debug.
3. If you have to do something crazy, you'll have to glue some stuff together.

Rake and Albacore help solve some of these problems. An XML-based build file will certainly have more stuff in it, and you might not get a useful message if you fatfinger a value when creating or changing it. 

Even with the inherent advantage of code over XML, the biggest gain is access to the full power of the Ruby language. If an application is big enough or has been around long enough, then there are probably some weirdo things you need to do when building it. If you're using Rake, you'll easily be able to script around most situations without resorting to creating another command line application or referencing a batch file from your XML file.  It's also easy to add other functionality like deployment and testing to your script with a few simple Ruby statements.

If you're worried about crossing over the Ruby world, creating your .NET build script is an easy, low risk way to get started.

[albacore]: https://github.com/derickbailey/Albacore/wiki/ "Albacore Wiki"
[railsinstaller]: http://http://railsinstaller.org/ "Rails Installer"
[example]: https://github.com/MichaelJosephKramer/Albacore-Example "Example"
[gitsetup]: http://help.github.com/win-set-up-git/ "Git Setup"
[casing]: https://github.com/derickbailey/Albacore/wiki/Getting-Started "Rakefile Casing"
