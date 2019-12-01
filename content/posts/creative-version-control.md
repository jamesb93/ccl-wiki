# Creative Version Control

### [James Bradbury](https://www.jamesbradbury.xyz) and [Jacob Hart](https://jacob-hart.com)
---
# Introduction
All creative coders can benefit from some form of version control for their creative work. Not only does version control provide a mechanism for backing up work it can aid collaboration, assist in tracking strands of ideas and maintain a history of the work and thoughts that are invested into the final output.

To really get understand version control (and to fix some gnarly issues) it is useful to know a bit about the command-line and its relationship to the overall operating system of your machine. Knowing a bit about the command-line can also be useful to you when trying to integrate code or other people's work into your practice. I can definitely recall times when I would come across git repositories, or other people's work that needed to be compiled in order to be run and I had no idea where to start.

In this week's hackspace [Francesco] and [James] gave a short tutorial for beginners on using the command-line for both MacOS and Linux. We covered some basics such as moving around your filesystem, making directories and other fundamental principles. They also demonstrated how to create a git repository, commit changes and have another coder collaborate on that work with you.

# Terminal Basics
So what is the command line? It's an interface to your computer that emulates the traditional terminal prior to GUI applications and modern window managers. All you are presented with is text and all programs that operate on the command-line accept and produce text. They hark back to an era where network connections were slow and minimal and bandwidth and computing power and memory were precious resources. 

On MacOS you can open up the default terminal by opening Terminal.app. Many people change their default terminal to [iTerm2](https://iterm2.com) as it has more features than the default application. If you are just beginning though I would recommend sticking to the default app, then you can complicate your setup later once you start to hit the limits of the default application. On Linux, popular terminal emulators are GNOME terminal, Guake, Yakuake and xterm.

When you open the terminal you are presented with almost nothing, just a small block of text on the left of the screen with the name of your computer. At this point you are ready to execute some commands.



- you are always somewhere in your computer
- ls
- cd
- touch
- mkdir
- pwd



As both of us (the authors) are specialised in music, we deal with Max patches (JSON files essentially), SuperCollider, javascript and Python files all the time and large projects that integrate a number of different text based formats alongside audio files for corpi and recordings. One practice that can be seen often is to have a number of top-level Max patchers.