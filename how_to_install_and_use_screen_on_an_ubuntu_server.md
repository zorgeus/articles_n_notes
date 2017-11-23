 ![](<https://i.imgur.com/zLsL0W9.png>)

[Go to the original article](<https://www.digitalocean.com/community/tutorials/how-to-install-and-use-screen-on-an-ubuntu-cloud-server>)

[PDF file](<https://www.dropbox.com/s/u1uaf38iaaw75xz/Screen_linux.pdf?dl=0>)


### Introduction  ###

 Screen is a console application that allows you to use multiple terminal sessions within one window. The program operates within a shell session and acts as a container and manager for other terminal sessions, similar to how a window manager manages windows.

There are many situations where creating several terminal windows is not possible or ideal. You might need to manage multiple console sessions without an X server running, you might need to access many remote cloud servers easily, or you might need to monitor a running program’s output while working on some other task. All of needs are easily addressed with the power of screen.

<div>
 </div>

## Installation  ##

 In this tutorial, we’ll be using Ubuntu 12.04, but outside of the installation process, everything should be the same on every modern distribution.

Use “apt-get” to install on Ubuntu:

sudo apt-get update sudo apt-get install screen <div>
 </div>

## Basic Usage  ##

 To start a new screen session, we simply type the “screen” command.

screen You’ll be greeted with the licensing page upon starting the program. Just press “Return” or “Enter” to continue.

What happens next may be surprising. We are given a normal command prompt and it looks like nothing has happened. Did “screen” fail to run correctly? Let’s try a quick keyboard shortcut to find out. Hold the control key, and then hit the “a” key, followed by the “v” key:

Ctrl-a v screen 4.00.03jw4 (FAU) 2-May-06 We’ve just requested the version information from screen, and we’ve received some feedback that allows us to verify that screen *is * running correctly.

Now is a great time to introduce the way that we will be controlling screen. Screen is mainly controlled through keyboard shortcuts. Every keyboard shortcut for screen is prefaced with “Ctrl-a” (hold the control key while pressing the “a” key). That sequence of keystrokes tells screen that it needs to pay attention to the next keys we press.

You’ve already used this paradigm once when we requested the version information about screen. Let’s use it to get some more useful information.

Ctrl-a ? This is the internal keyboard shortcut screen. You’ll probably want to memorize how to get here, because it’s an excellent quick reference. As you can see at the bottom, you can press “space” to get more commands.

Okay, let’s try something more fun. Let’s run a program called “top” in this window, which will show us some information on our processes.

top Okay, we are now monitoring the processes on our VPS. But what if we need to run some commands to find out more information about the programs we see? We don’t need to exit out of “top”. We can create a new window to run these commands.

Ctrl-a c The “Ctrl-a c” sequence creates a new window for us. We can now run whatever commands we want without disrupting the monitoring we were doing in the other window.

Where did that other window go? We can get back to it using a new command:

Ctrl-a n This sequence goes to the next window that we are running. The list of windows wrap, so when there aren’t windows beyond the current one, it switches us back to the first window.

Ctrl-a p This sequence changes the current window in the opposite direction. So if you have three windows and are currently on the third, this command will switch you to the second window.

A helpful shortcut to use when you’re flipping between the same two windows is this:

Ctrl-a Ctrl-a This sequence moves you to your most recently visited window. So in the previous example, this would move you back to your third window.

At this point, you might be wondering how you can keep track of all of the windows that we are creating. Thankfully, screen comes with a number of different ways of managing your different sessions. First, we’ll create three new windows for a total of four windows and then we’ll try out one of the simplest window management tools, “Ctrl-a w”.

0$ bash 1$ bash 2-$ bash 3*$ bash We get some useful information from this command: a list of our current windows. Here, we have four windows. Each window has a number and the windows are numbered starting at “0”. The current window has an asterisk next to the number.

So you can see that we’re currently at window #3 (actually the fourth window because the first window is 0). So how do we get to window #1 quickly?

Ctrl-a 1 We can use the index number to jump straight to the window we want. Let’s see our window list again.

Ctrl-a w 0$ bash 1*$ bash 2$ bash 3-$ bash As you can see, the asterisk tells us that we’re now on window #1. Let’s try a different way of switching windows.

Ctrl-a “ Num Name Flags 0 bash $ 1 bash $ 2 bash $ 3 bash $ We get an actual navigation menu this time. You can navigate with either the up and down arrows or with “j” and “k” like you would in the vi text editor. Switch to a window by pressing “Return” or “Enter”.

This is pretty useful, but right now all of our windows are named “bash”. That’s not very helpful. Let’s name some of our sessions. Switch to a window you want to name and then use the “Ctrl-a A” sequence.

Ctrl-a 0 Ctrl-a A Set window's title to: bash Using the “Ctrl-a A” sequence, we can name our sessions. You can now backspace over “bash” and then rename it whatever you’d like. We’re going to run “top” on window #0 again, so we’re going to name it “monitoring”.

