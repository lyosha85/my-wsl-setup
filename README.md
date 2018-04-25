# My WSL Setup for Development
Quick rundown on my current setup  Windows Subsystem for Linux.

After a few headaches with running the Git Bash on Windows I’ve decided to move over the WSL for all development purposes. I’m very new to Linux so this is a very top-level overview so feel free to submit any changes.

Here’s a breakdown of how I got up and running below:

![Shell Screenshot](shell.png "Shell Screenshot")

### Download & Install the WSL
- Follow the very thorough instructions [here](https://msdn.microsoft.com/en-au/commandline/wsl/install_guide)

### Get your terminal looking pretty pt.2
- Install Oh My Zsh with `sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`
  - Read docs [here](https://github.com/robbyrussell/oh-my-zsh) on how to add more plugins and change themes (I went with their out of the box 'robbyrussell').

### Zsh Syntax Highlighting
This was a late addition but is an amazing add-on to the terminal. Follow the steps [here](https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md) to get it up and running.

### Zsh Auto Suggestions
Another late addition but amazing as well. Just follow the **Maunal** instructions [here](https://github.com/zsh-users/zsh-autosuggestions).

### Fix the ls and cd colours
Out of the box when you `ls` or `cd` + `Tab` you get some nasty background colours on the directories. To fix this, crack open your ~/.zshrc file and add this to the end:
```
#Change ls colours
LS_COLORS="ow=01;36;40" && export LS_COLORS

#make cd use the ls colours
zstyle ':completion:*' list-colors "${(@s.:.)LS_COLORS}"
autoload -Uz compinit
compinit
```


### Install Chocolatey
- Follow instuctions https://chocolatey.org/install

### Cmder / ConEmu
Its a terminal program that serves as excellent replacement for the built-in
cmd.exe. It's not a shell itself, so it supports running plain old cmd.exe
commands and running PowerShell.

As a comment notes below, Cmder is actually a packaged version of ConEmu.
If you don't want the out-of-the-box configuration that Cmder comes with, you
can install ConEmu by iteslf and customize it to your needs from there.
Both of course have the same features though.

With Chocolatey, run one of the following from an elevated propt:

`choco install cmder`

or

`choco install conemu`

### Git
#### Install Git in WSL
- Run this `sudo apt update`
- Then run `sudo apt install git`

#### Install Git in Windows
Git uses a per-user config file located at %USERPROFILE%\.gitconfig. It's a
plain-text file you can edit with your favorite text editor (or Notepad, if you
haven't chosen a favorite yet). Note that git calls this config the "global"
config. It's more general than per-repository config, and more specific than the
machine config.
##### TortoiseGit

This is optional, but highly recommended. Download and install TortoiseGit.
You'll want some of the tools it installs later. If you have another Tortoise
installed (e.g. TortoiseSVN) and you don't want to have TortoiseGit's context
menu clutter, you don't have to download it. If you're not sure, get it.
TortoiseGit is nice because it adds overlay icons (that don't always update
properly). Another benefit is that a full PuTTY install, which you'll also need.

If you really don't want the extra shell extension (I don't blame you), you can
install TortoiseGit, copy TortoiseGitPlink.exe from TortoiseGit's bin/
directory, store it somewhere else, and then uninstall TortoiseGit. Then, later
on when you set the GIT_SSH environment variable, just use the new path to it.

##### Chocolatey:

`choco install tortoisegit`

##### PuTTY
You can skip this step if you installed TortoiseGit.

If you didn't install it, download and run the Windows Installer so you get all
the apps installed from one package.

Chocolatey:
`choco install putty`

##### Authentication

There are a few different ways to authenticate with GitHub. The easiest is to
use Git Credential Manager for Windows. It supports authenticating with GitHub
over HTTPS even with two factor authentication. If you use this, you can skip to
the "Install Git" section. If you wan to use SSH, read on.
SSH Setup with PuTTY

If you're using a service like GitHub or Bitbucket, you have a couple of options
when authenticating so you can push your code. This will allow you to share your
code with other people. Even if you're the only person working on a project,
those sites can serve as a backup.

##### Set up SSH keys on Windows:

- Open PuTTYgen by searching for it in the Start menu or Start screen.
- Leave the settings as they are, unless you know what you're doing.
- Click "Generate".
- Wiggle the mouse in the top part of the window until the progress bar is full
- Once you've provided enough entropy, a bunch of text fields will appear.
- It's highly recommended that you provide a passphrase (recommended).
- After providing a passphrase, click "Save private key" in %USERPROFILE%\_ssh\.
- Call the file in a way that pleases you
- Log in to GitHub.com. Don't close PuTTYgen yet.
- Go to your Account settings and then to SSH keys.
- Click "Add SSH key". Copy the Public key into github and name it
- Click "Add key".

And with that, we're done setting things up to connect to GitHub.

If you want a native command-line build of ssh (i.e., ssh.exe or ssh-agent.exe)
to work, you'll need to also export your key in OpenSSH format. You can do this
from PuTTYgen by clicking on Conversions > Export OpenSSH Key. These keys are
typically saved in %USERPROFILE%\.ssh or (~/.ssh in *nix-style paths, which also
work in Bash environments on Windows). Once you export the key, you should copy
it to `%USERPROFILE%\.ssh\id_rsa`.

##### PoshGit

This is mostly optional if you're using Cmder, but if you want more general
support for Git in PowerShell, you can install an awesome package called
posh-git. Because PowerShell is awesome, and you should be using it instead of
batch scripts and plain old cmd.exe as much as you can. Even without this, you
can use Git commands from PowerShell, but posh-git will give you status
information right in the prompt.

- Make sure you have PowerShell 5 or later installed.
- Open an elevated PowerShell prompt and enter:
- `Set-ExecutionPolicy RemoteSigned` # Change PowerShell script execution policy
- `Install Posh-Git`
- `Install-Module Posh-Git`

Now, whenever you're in a Git workspace directory in your PowerShell prompt,
you'll get a fancy prompt, and you can still use tab completion and standard
Windows paths. Hooray! Git will still echo paths with backslashes, but it will
recognize forward slashes.

##### Config Tweaks
- Install some git nuggets with: `choco install diffmerge p4merge`
- Add this to your `.gitconfig`
```
[diff]
    tool = sgdm
    guitool = sgdm
[difftool]
    prompt = false
[difftool "sgdm"]
    cmd = "C:/Program\\ Files/SourceGear/Common/DiffMerge/sgdm.exe" --nosplash "\"$LOCAL\"" "\"$REMOTE\""
    path = "C:/Program\\ Files/SourceGear/Common/DiffMerge/"
[difftool "p4merge"]
    cmd = p4merge.exe\"$LOCAL\"\"$REMOTE\"
[merge]
    tool = p4merge
[mergetool]
    keepBackup = false
    prompt = false
[mergetool "sgdm"]
    cmd = "C:/Program\\ Files/SourceGear/Common/DiffMerge/sgdm.exe" --nosplash --merge --result="\"$PWD/$MERGED\"" "\"$PWD/$LOCAL\"" "\"$PWD/$BASE\""
    trustExitCode = true
    prompt = false
[mergetool "p4merge"]
    cmd = "p4merge.exe" "\"$BASE\"" "\"$LOCAL\"" "\"$REMOTE\"" "\"$MERGED\""
    keepTemporaries = false
    trustExitCode = true
```
### VSCode
- Install VSCode
- Get linux eol with this setting:
```
    "files.eol": "\n",
```
### Setup a SSH key and link to your Github
- Follow the Linux steps [here](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/#platform-linux) to create a key and add it to your SSH agent
- Then type `cat ~/.ssh/id_rsa.pub`
- Copy your key from the terminal and paste it into your Github keys

### Install N (alternative to Node Version Manager)
My shell was running slow with nvm so I switched to [n](https://github.com/mklement0/n-install). Just install it with their curl command and if n doesn't work as a command on zsh, it would have installed it's path into your ~/.bashrc so just copy it over to your ~/.zshrc.

### Install Gulp CLI
- Follow the Gulp docs [here](https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md).


### Docker
- Install Docker on windows and Enable "Expose daemon on tcp://localhost:2375"

#### Install Docker within WSL
```
# Environment variables you need to set so you don't have to edit the script below.
export DOCKER_CHANNEL=edge
export DOCKER_COMPOSE_VERSION=1.21.0

# Update the apt package index.
sudo apt-get update

# Install packages to allow apt to use a repository over HTTPS.
sudo apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common

# Add Docker's official GPG key.
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Verify the fingerprint.
sudo apt-key fingerprint 0EBFCD88

# Pick the release channel.
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   ${DOCKER_CHANNEL}"

# Update the apt package index.
sudo apt-get update

# Install the latest version of Docker CE.
sudo apt-get install -y docker-ce

# Allow your user to access the Docker CLI without needing root.
sudo usermod -aG docker $USER

# Install Docker Compose.
sudo curl -L https://github.com/docker/compose/releases/download/${DOCKER_COMPOSE_VERSION}/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose &&
sudo chmod +x /usr/local/bin/docker-compose
```
### Get your terminal looking pretty pt.1
- Download Hyper.js [here](https://hyper.is/) - I went with the 'hyperblue' theme.

### Automatically open in Bash
- Open up Hyper and type `Ctrl` + `,`
- Scroll down to shell and change it to `C:\\Windows\\System32\\bash.exe`

### Install Zsh
- Run this `sudo apt-get install zsh`
- Open your bash profile `nano ~/.bashrc`
- Add this to set it to use ZSH as default:
```
if [ -t 1 ]; then
  exec zsh
fi
```

#### Configure WSL to Connect to Docker for Windows
`echo "export DOCKER_HOST=tcp://0.0.0.0:2375" >> ~/.bashrc && source ~/.bashrc`

#### Bind custom mount points to fix Docker for Windows and WSL differences:
```
sudo mkdir /c
sudo mount --bind /mnt/c /c
```

It’s worth noting that whenever you run a docker-compose up, you’ll want to make sure you navigate to the /c/Users/nick/dev/myapp location first, otherwise your volume won’t work. In other words, never access /mnt/c directly.

#### Automatically set up the bind mount:
echo "sudo mount --bind /mnt/c /c" >> ~/.bashrc && source ~/.bashrc

#### Allow your user to bind a mount without a root password:
`sudo visudo`

`alex ALL=(root) NOPASSWD: /bin/mount #replace alex with your username`


### Aliases
Just to test out using aliases, I picked a few things I type a lot into the terminal that could save me some keystrokes. Add this to your .zshrc file to do the same and add anything else you see fit:
```
# aliases for git
alias add='git add -A'
alias status='git status'
alias push='git push -u origin master'
alias pull='git pull'
alias log='git log'
```

### Change default editor
Out of the box the default editor is nano and no syntax highlighting can make it pretty tough to navigate through. So I've decided to switch the default editor to Vim:
```
# Set default editor to vim
export VISUAL=vim
export EDITOR="$VISUAL"
```

### Always Update & Upgrade
Every few days it's a good idea to run `sudo apt update` then `sudo apt upgrade` to get the latest updates from Ubuntu.

### Shopify Themekit
I've recently been getting into Shopify and ran into a bit of a snag when using their [ThemeKit](https://shopify.github.io/themekit/) via the WSL. After creating a theme or downloading an existing one, then trying to run `theme open` to view changes. This command tries to open a browser with xdg-open - which doesn't work. What you'll need to do is create a shell script to get around this. Seeing as I didn't know how to create one prior to this, I'll add the details below:
- First, navigate to `usr/local/bin` as we'll need the script in here so it can be executed.
- Second, we'll need writing access, so write `sudo vi xdg-open` to create the file.
- Third, paste this into the file then save and exit:
```
#!/bin/sh
# shell script for shopify theme kit/xdg-open to open browser.
# replace firefox.exe with the browser of your choice.
cmd.exe /c start firefox.exe $1
```
- Fourth, you'll need to make the file executable. So write `sudo chmod +x xdg-open` and then you're good to go! Just head back to your directory and run `theme open` to test.

---

### Notes
- I had trouble with Node/npm/Gulp before realising Ubuntu won’t automatically upgrade from 14.04 to 16.04 - as mentioned in this [article](https://blogs.msdn.microsoft.com/commandline/2017/04/11/windows-10-creators-update-whats-new-in-bashwsl-windows-console/). If you’re in the same boat, upgrade via these [instructions](https://help.ubuntu.com/lts/serverguide/installing-upgrading.html).
- I also had trouble with older versions of Windows 10 not liking Gulp via the WSL - inotify and socket issues. Make sure you're running on the Creators Update or Insiders Program versions.
- A lot of Hyper plugins don't work with Windows so make sure to check before installing them.
- If you're having trouble with Ruby and it's sudo/gem issues, look at Ruby Version Manager as an alternative.

### Easter Eggs
A few goodies I installed recently for fun:
- Cowsay `sudo apt-get install cowsay`
- htop `sudo apt-get install htop`
- Isn't via apt-get but [Bash-Snippets](https://github.com/alexanderepstein/Bash-Snippets) is great
