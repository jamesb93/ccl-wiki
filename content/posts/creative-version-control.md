# Creative Version Control

### [James Bradbury](https://www.jamesbradbury.xyz) and [Jacob Hart](https://jacob-hart.com)
---
# Introduction
All creative coders can benefit from some form of version control for their creative work. Not only does version control provide a mechanism for backing up work it can aid collaboration, assist in tracking strands of ideas and maintain a history of the work and thoughts that are invested into the final output.

To really get understand version control (and to fix some gnarly issues) it is useful to know a bit about the command-line and its relationship to the overall operating system of your machine. Knowing a bit about the command-line can also be useful to you when trying to integrate code or other people's work into your practice. I can definitely recall times when I would come across git repositories, or other people's work that needed to be compiled in order to be run and I had no idea where to start.

In this week's hackspace [Francesco] and [James] gave a short tutorial for beginners on using the command-line for both MacOS and Linux. We covered some basics such as moving around your filesystem, making directories and other fundamental principles. They also demonstrated how to create a git repository, commit changes and have another coder collaborate on that work with you.

# What is the command line?
The command-line is an interface to your computer that emulates the traditional terminal of a computer prior  to GUI applications and modern window managers. All you are presented with is text and all programs that operate on the command-line accept and produce text. They hark back to an era where network connections had minimal bandwidth and CPU cycles and RAM were precious resources.

The workshop was mainly aimed at UNIX systems, so a lot or all of these commands won't work on windows. However, if you are a windows user you might like to take a look at the Windows Subsystem for Linux (WSL) and [MSYS](http://www.mingw.org/wiki/msys) which provides a UNIX like interface on windows operating systems.

On MacOS you can open up the default terminal by opening Terminal.app. Many people change their default terminal to [iTerm2](https://iterm2.com) as it has more features than the default application. If you are just beginning though I would recommend sticking to the default app and once you find that limiting you can investigate other terminal emulators. On Linux, popular terminal emulators are GNOME terminal, Guake, Yakuake and xterm but entirely depends on your distribution.

## The shell

When you open the terminal you are presented with almost nothing, just a small block of text on the left of the screen with the name of your computer. At this point you are ready to execute some commands.

<img src="/img/cvc/blank-terminal.png" alt="A blank ZSH terminal">

The first thing to know is that the application which interprets the commands you send to the terminal is a program like any other. There are many flavours of _shells_ that you can use and by default MacOS will use **bash** or **zsh** depending on your OS version. There are small differences between these two shells and for the purpose of what this article covers it is not necessary to understand too deeply how they vary. 

## ls

The second concept to understand is that the shell is always _somewhere_ in your filesystem. When you boot up a terminal, it starts at your home folder. You can see at the top of the preceding image that my terminal tells me where it currently is (/Users/james) and that it is running **zsh** as the shell. The home folder can also be abbreviated to **~/** which you can see on the left side of the image.

Try typing the command **ls** now and see what happens.

<img src="/img/cvc/ls.png" alt="Executing the 'ls' command">

Congratulations, you just executed your first program from the command-line. **ls** is a program that lists information about the files and folders of the current directory that you are in. As you can see below, what is displayed by executing **ls** has parity the files and folders listed in finder at the same location.

<img src="/img/cvc/finder.png" alt="Parity with the finder">

## cd

Now you might be wondering how you can navigate to different folders and files. The **cd** (change directory) is the program that helps you move around as you would in Finder or a GUI application. As you can see below, I execute the command `cd dev` which then transports my **current working directory** to the dev folder. The **prompt** of my command-line changes to reflect this movement throughout the filesystem and I can then execute **ls** again to show files inside **~/dev**. Whenever we pass additional bits of text to a program these are called _arguments_. In this case **dev** is an argument to **cd** and cd interprets what you mean by the simple virtue that it immediately comes after **cd**. Other programs can take arguments with a flag in both a long and short format like **-l** or **-limit** for example.

<img src="/img/cvc/cd.png" alt="Parity with the finder">

## mkdir

Say now you want a new folder in this directory and to create a new file inside of that folder.

Let's execute the command **mkdir** (make directory) with an argument for the name of the folder that we would like to create. We can verify that the folder was created by looking at Finder and by changing directory into this new folder.

<img src="/img/cvc/mkdir.png" alt="the mkdir command">

## touch

Now to create a file, we can execute the program **touch** with the new file name as the first argument. Again we can verify that this new file exists by looking in the finder as well as calling **ls** again to print the files inside of a directory.

<img src="/img/cvc/touch.png" alt="the touch command">

## piping

One final concept to understand is that the inputs and outputs of the shell can be 'piped' between each other as they emit and receive information that fits a specification. For example, I can pipe **ls** to another program called **grep** which you might like to think of as a filter of that text like find/replace in a text editor. In the below example, I pipe the output of **ls** to **grep** so that I can find all the files that contain the phrase _gesture_maker_. You might also be interested in the **>** redirection pipe which allows you to pipe the output of a command line program to a file. For example, try **ls > ls_output.txt** which will create a new file contaning the output of ls.

<img src="/img/cvc/piping.png" alt="piping commands">

This just about covers enough material to get you started in the terminal. Of course, the best way to learn is to try using the terminal and to get stuck. Once you become more fluent in the commands it can be a great way to interact with your operating system with just your keyboard and with a minimal interface. It is also helpful for later when you want to create and manage git respositories more fluently and without a GUI application.

# git

Okay, so now lets start learning some **git**. The first thing to know is **git** is __NOT__ github. git can exist without the internet ever being connected to your machine and is just a program which tracks changes inside a directory. All that this is is a normal directory on your computer with a small hidden folder named **.git**. This hidden folder contains information about changes, branches, remotes and other git related concepts which help you to version control the files that exist in the same directory.

Conveniently, most UNIX based operating systems are shipped with git installed and you can use it without every having to install it yourself. What we're going to do is learn first how to _init_ a git repository, _commit_ changes to a file inside the repository and then _push_ those changes to a _remote_. After that, it will be demonstrated how someone else can _fork_ your repository, _commit_ their own changes and subsequently submit a _pull request_ which you integrate back into the original project.

To make things easier, it can be useful to download a _git client_ on your machine. Although most of the following things can be done via the command line, one of these programs will make your life a whole lot easier. We can recommend [Fork](https://git-fork.com), [GitKraken](https://www.gitkraken.com) or [GitHub Desktop](https://desktop.github.com).

## initialising (init) a repository

So, you have an idea for a great project where version control would be useful. Maybe this project exists already, maybe you haven't even started it yet. You'll want to make sure that all the files concerning your project are grouped into one folder. Next, we need to create the hidden **.git** folder inside this project folder which will handle all the git functionalities. We can do this from the command line - below we've gone to the desktop, created our project folder, then used the command `git innit` and we're done!

<img src="/img/cvc/01_innit_cmd_line.png" alt="git innit command">

If you go to the project folder in your finder, you'll find that it appears to be empty. This is because the **.git** folder is hidden - to show it, hit Cmd+Shift+Dot and you should see this...

<img src="/img/cvc/02_hidden_git_folder.png" alt="see hidden folder">

You won't ever need to do anything with this folder, so you can go ahead and hide it again. But know that this is where all the magic of git happens! When you create a repository with one of the programs like _Fork_ or _GitKraken_, this folder gets created automatically. Now we can start adding files to our project.

## commiting (commit) to a repository

Let's create some simple, empty files using the `touch` command. These files are now in the folder, but we haven't told git to add them yet. To create a new _commit_ (a state of your project that you can go back to), there are two parts: _adding_ files and modifications, then _commiting_ these added files and modifications. Here we add files using the  `git add` command, then commit using the `git commit` command. When you commit, you'll be asked to describe the modifications made - we just wrote _initial commit_, to indicate that this was the first commit to git (we then hit ESC and type :wq to exit the text editor).

<img src="/img/cvc/03_commit.png" alt="command line commit">

If we open up our repository in on of the programs talked about above, we can visualise these changes we made. Here, in _Fork_, we see our initial commit, with the two files we added. A quick note, we could have just used the command `git commit -a` to add and commit everything, and `git commit -m "initial commit"` to bypass the text editor part.

<img src="/img/cvc/04_initial_commit_fork.png" alt="initial commit in fork">

How can we add the other file to our git repository? We could do it in the same way, using the comand line. However, it may be easier to use the GUI in _fork_. If we look at the top left, we see that there is one change that can be commited. Let's click on this to see the changes. We see that the file we didn't initially commit is there. To make a new commit, we first need to _stage_ the changes we want to make, add a _comment_ for out commit in the bottom right (this was the text editor part from before), then click _commit_! Now if we go back to the _master branch_, we see that there is a new commit.

<img src="/img/cvc/05_second_commit.png" alt="initial commit in fork">

We now see how we can keep a log of how a project progresses, and use this to go back to a project's previous state. But what if we want to have this log online, and let other people work on it also? This is where something like [github](https://github.com) will come in.

## pushing (push) changes to a repository

First of all, we're going to have to get our repository linked up to _github_. To do this, go to _github_ and 

## forking (fork) a repository



As both of us (the authors) are specialised in music, we deal with Max patches (JSON files essentially), SuperCollider, javascript and Python files all the time and large projects that integrate a number of different text based formats alongside audio files for corpora and recordings. One practice that can be seen often is to have a number of top-level Max patchers.