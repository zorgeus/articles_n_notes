
## ![](<https://i.imgur.com/zLsL0W9.png>)  ##


##  ##


## Using Byobu  ##

 A great enhancement for screen is a program called “byobu”. It acts as a wrapper for screen and provides an enhanced user experience. On Ubuntu, you can install it with:

sudo apt-get install byobu Before we begin, we need to tell byobu to use screen as a backend. We can do this with the following command:

byobu-select-backend Select the byobu backend: 1. tmux 2. screen Choose 1-2 [1]: We can choose screen here to set it as the default terminal manager.

Now, instead of typing in “screen” to start a session, you can type “byobu”.

byobu As you can see, you now have screen wrapped in a nice interface. When you type “Ctrl-a” for the first time, you’ll have to tell byobu to recognize that as a screen command and not an Emacs command.

Ctrl-a Configure Byobu's ctrl-a behavior... When you press ctrl-a in Byobu, do you want it to operate in: (1) Screen mode (GNU Screen's default escape sequence) (2) Emacs mode (go to beginning of line) Note that: - F12 also operates as an escape in Byobu - You can press F9 and choose your escape character - You can run 'byobu-ctrl-a' at any time to change your selection Select [1 or 2]: Select “1” to use byobu as normal. The interface gives you a lot of useful information, such as a window list and system information. On Ubuntu, it even tells you how many packages have security updates with a number followed by an exclamation point on a red background. One thing that is different between using byobu and screen is the way that byobu actually manages sessions. If you simply type “byobu” again once you’re detached, it will simply reattach your previous session instead of creating a new one. To create a new session, you must type:

byobu –S sessionname Change “sessionname” to whatever you’d like to call your new session. You can see a list of current sessions with:

byobu –ls There are screens on: 22961.new (07/01/2013 06:42:52 PM) (Detached) 22281.byobu (07/01/2013 06:37:18 PM) (Detached) 2 Sockets in /var/run/screen/S-root. And if there are multiple sessions, when you type “byobu”, you will be presented with a menu to choose which session you want to connet to.

byobu Byobu sessions... 1. screen: 22961.new (07/01/2013 06:42:52 PM) (Detached) 2. screen: 22281.byobu (07/01/2013 06:37:18 PM) (Detached) 3. Create a new Byobu session (screen) 4. Run a shell without Byobu (/bin/bash) Choose 1-4 [1]: You can select any of the current sessions, create a new byobu session, or even get a new shell without using byobu.

One option that might be useful on a cloud server you manage remotely is to have byobu start up automatically whenever you log into your session. That means that if you are ever disconnected from your session, your work won’t be lost, and you can simply re-connect to get right back to where you were before.

To enable byobu to automatically start with every login, type this into the terminal:

byobu-enable The Byobu window manager will be launched automatically at each text login. To disable this behavior later, just run: byobu-disable Press to continue... As it says, if you ever want to turn this feature off again, simply type:

byobu-disable It will no longer start automatically.

