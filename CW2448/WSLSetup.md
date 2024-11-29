# WSL Setup

Windows Subsystem for Linux (WSL) is a nice tool to install and run a Linux distribution on a Windows machine. This means you get access to the superpower of Linux on Windows :).

## Basic

Let's start a Powershell and set the WSL version to 2 as a first step. Next, we want to see a list of all available distributions. Once we have found a good distribution, we can simply install it.

```powershell
wsl --set-default-version 2
wsl --list --online # list all available distributions
wsl --install -d Ubuntu-22.04
```

Let's assume you want your new machine to appear with a nice name and not just the name of the distribution from above. To do this, you must first export the distribution you have just installed and then import it. The import allows you to enter a name of your choice.

```powershell
wsl --list --verbose
wsl --export Ubuntu-22.04 ubuntu-22.04.tar      # create a backup
wsl --unregister Ubuntu-22.04                   # if you like, remove VM
wsl --import MyDevMachine C:\WSL\MyDevMachine ubuntu-22.04.tar --version 2   # import with custom name
wsl --set-default MyDevMachine
wsl -d MyDevMachine # start your machine
```

Normally a nice prompt for your user and password is displayed during the installation. But for some reasons it was not displayed during the installation. But this is a simple thing, so I add a user. Assuming you already have a user, but you did something wrong, you can access your machine as root with the following command.

```powershell
wsl -d DevMachine -u root
```

Ok, but now we add the user so that it works and he has a home directory.

```bash
useradd -m ms           # login as root
userdel ms              # if you dont like the name or somehow messed up
usermod -a -G sudo ms   # Add the newly created user to the sudo group (makes sudo execution possible)
groups ms               # If you like to check if adding the group sudo to the specified user
passwd ms               # change password
su ms                   # switch to the newly created user
chsh -s $(which bash)   # switch shell from shell to bash
exit                    # go back to root user and re-login to user
su ms                   
echo $SHELL             # Check that bash is now your shell
```

The next time you log in, you will probably find that you are logged in as the root user. Normally we don't want this, so we need to change it. There is a configuration file for the WSL that defines which user you should be logged in with by default. There you can add your newly created user as the default user.

```bash
nano /etc/wsl.conf  # append the following to your config file
[user]
default=ms
```

## Docker Engine

I think on a real devmachine the docker engine is a must. So please refer to the following links to install Docker.

See the following link to do this.

[Docker Installation](https://docs.docker.com/engine/install/ubuntu/)

Also do the post install steps

[Docker post-installation](https://docs.docker.com/engine/install/linux-postinstall/)

## USB/IP for WSL

If you like playing with hardware, this is the right way to make devices available in your WSL.

If you like to connect a device via USB to your WSL distribution you need to install the [USB/IP tool](https://github.com/MicrosoftDocs/wsl/blob/main/WSL/connect-usb.md).

## VS Code

Using an editor inside WSL can be very easy if you use the Remote Development Extension from Microsoft in VSCode. Access your WSL by pressing Ctrl-P, type into the filed >WSL: Connect to WSL. Press enter and you should be in your machine immediately.

## Github

Let's also connect your machine to your github account so you can easily pull and push your repositories. To do this, go to github and click on your user icon. In the context menu that appears, select Settings. Scroll down and select Developer settings on the left-hand side. Go to Token (classic) and add a new token. In the selected area, check Repo and Workflow. This should be sufficient for the first step. Let's go back to WSL, there tell git your name and your email. Clone your repo add stuff and push to remote.

```bash
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
git clone https://github.com/matstutz/leftovers.git
git add *
git commit -m "initial commit"
```

With the Git Credentials Helper you can store your credentials easily (and insecurely in plain text). Activate the Credentials Helper before you push and authenticate with your token against Github.

```bash
git config --global credential.helper store
git push
```
