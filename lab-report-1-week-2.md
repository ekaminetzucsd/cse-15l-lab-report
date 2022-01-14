# Week 2 -- Lab Report 1

[Link to index](./index.html)

This lab report, written in the form of a tutorial, will describe how to log into a course-specific account on and interact with the ieng6 cluster at UCSD over openssh.

## Step 1: Installing Visual Studio Cod{ium|e}

*Note: with VS Code being the slow, non-native electronJS nightmare (figure 1) it already is, "proprietary" shouldn't be added to that list. The VS Codium project compiles just the MIT-licensed base without any of Microsoft's nonfree blobs or telemetry.*

This one's pretty simple for real operating systems. For my package manager, the command was `yay -S vscodium-bin`. After that, it's just a matter of opening the integrated terminal and typing `emacs -t .` to get a real text editor, too (figure 2).

![Bruh](./resources.png)

*Figure 1: VS Code and Github Desktop using over 2 gigs together at idle, significant on even a beefy gaming laptop. This is your brain on electron.*

![Nested editors](./nested-editors.png)

*Figure 2: nano running inside vim running inside emacs running inside VS Codium, demonstrating the only use for VS Codium's integrated terminal*

## Step 2: Remotely Connecting

If it isn't already, install openssh (for my package manager, `sudo pacman -S openssh`). To connect without any special configuration, the command is `ssh cs15lwi22apt@ieng6.ucsd.edu`, replacing `cs15lwi22apt` with the relevant username for that class, quarter, and person. If this is the first time connecting, one will recieve a warning that the fingerprint to the server is unrecognized to help protect against MITM attacks; as far as I know, UCSD does not publicly provide the fingerprint of its servers, so one should just type `yes` if this is the first time connecting. Regardless, the server will then ask for the password to the specified account; one should input this and hit enter, upon which he will be logged in and the welcome message in the .bash_profile will print (figure 3).

![Logging in over ssh](./logging-in.png)

*Figure 3: the default (I think) welcome message for ieng6 printing upon login*


