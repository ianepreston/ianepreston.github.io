# Setting up for data science in python on Windows

There are lots of great guides for setting up an environment to do data science. For my purposes though they generally lack two things:

* They're designed for Mac/Linux and I run Windows at work
* They just don't match my exact personal taste/environment

This guide is intended to be useful for anyone trying to get set up for data science in python on windows. I think most of the steps are fairly generic, and I'll make an effort to highlight the parts that are more opinionated. The sections below will go over the core software that will need to be installed, and some handy customizations.

## Install VS Code

First we need an editor to actually work in. I'm a huge fan of VS code for this. It's lightweight, extensible, free and open source, and very actively developed. Every month there's a new release with some fancy thing that makes my life easier. 

### App installation

Head to the [VS code download page](https://code.visualstudio.com/download#) and pick the Win64 user installer

Go through the install process, I think all the defaults are fine, so just keep hitting next. You can optionally add in things like "Add open with code action to Windows Explorer". It won't directly impact the rest of what we're doing, just integrates code with the rest of your desktop a little more fully.

### Basic extensions

What makes VS code so great is all its extensions. There are a few that require some configuration so I'll address them later, here are some out of the box ones that are great:

* Bracket Pair Colorizer 2 colour codes your brackets. Super helpful for debugging.
* Dracula Official is a nice dark theme. The Material themes are also nice.
* GitLens integrates a lot of git functionality into VS code, things like showing who made what changes inline in your code.
* indent-rainbow colour codes your indentation level, similar to the brackets
* markdownlint gives style suggestions when writing markdown files (like this guide!)
* Material icon theme makes the icons in the file explorer a little nicer
* Python is pretty core for this for obvious reasons
* Visual Studio Intellicode gives better code completion than the built in one.

### Finish up later

There's more configuration to do in VS code, but prior to that let's set up the actual applications we'll use with it.

## Install git and bash

Like any sort of coding work, data science is done best under version control, and git is the defacto standard for that. Bash is not as obviously essential to coding/data science, but it comes with git, and using it for everything rather than the Windows shells will make it easier to apply instructions from other guides in the future, since most of them will assume you're using bash. Plus I like bash way more than the command prompt or powershell.

git does not require any special admin privileges to install (although for some reason uninstalling it seems to require admin, so be aware of that). Go to the [Git download page](https://git-scm.com/download/win) and Choose 64-bit Git for Windows Setup and run the installer. First part is to read (if you want) and accept the license:

![](/images/windows_ds_software/git_01.PNG "git license")

The default path should be fine, but take note of where it's being installed because we'll need to point VS code to it later

![](/images/windows_ds_software/git_02.PNG "git path")

On the components selection screen I deselect git gui because I only ever want the terminal. If you like or want to try the gui you could leave that checked. I also check "Check daily for Git for Windows updates" because I don't want to have to remember to update.

![](/images/windows_ds_software/git_03.PNG "git components")

Next you'll be prompted to make a start menu folder, just click next.

The next thing you'll be prompted for is for the default editor. If you're comfortable with vim/nano/whatever you can change it to that. I'm going to use VS code because it's the editor I'll be using for everything else, and I'm going to add vim bindings to it anyway:

![](/images/windows_ds_software/git_04.PNG "git editor")

At the next prompt leave it on the default, we want VS code and other tools to know git exists.

![](/images/windows_ds_software/git_05.PNG "git path")

For the SSH executable we'll use openssh. Later we'll configure remote development with VS code and also have to use openssh there. If you use and like putty you could swap this out, but note that VS code (at least at time of this writing) doesn't support putty, so you'll have to set things up separately there.

![](/images/windows_ds_software/git_06.PNG "git ssh")

Next up is the https transport backend. If this is a personal machine you can probably just leave it on OpenSSL. If it's a work computer you should probably switch to "Use the native Windows Secure Channel library". For example, at work my git repos are hosted on an on prem TFS server, so I definitely want my AD Domain service to validate me.

![](/images/windows_ds_software/git_07.PNG "git ssl")

At the next prompt we again want the default. Windows and *NIX systems use different symbols to denote line endings. This setting will automatically convert to the Windows format when you pull down changes, but leave them *NIX style when you push them up. This will ensure everyone is getting the correct format of text file when pulling down changes.

![](/images/windows_ds_software/git_08.PNG "git line endings")

Leave the console on MinTTY, it works nicer than the other. I thought you might need it to be the Windows default console to integrate with VS code but that is not the case. In fact it seems to break console integration with VS code. Go figure.

![](/images/windows_ds_software/git_09.PNG "git terminal")

Finally I just left all the extra options on default.

![](/images/windows_ds_software/git_10.PNG "git extras")

### Customizing git

There are a couple handy things that are useful to customize about git. For one, VS Code is going to make a .vscode folder anywhere we open a folder with it. We're never going to want to commit that file to version control, so we'll create a global gitignore file (as opposed to the more standard repository specific ones) and exclude that file. I got this idea from [this blog](https://julien.danjou.info/properly-managing-your-gitignore/), so credit there. In your ```%UserProfile%``` folder, create a file named ```.gitignore_global```. Regular windows explorer probaby won't like you making a file with that name, so from git bash in the same folder you can run ```touch .gitignore_global```, which will make an empty file. Then you can open that with notepad/vim/VScode to update it. Mine is pretty sparse right now, it just has one line:

```git
*.vscode
```

If you find your setup generates other files you'd like to ignore put them here, but don't use this file for language specific stuff like ```.ipynb-checkpoints```, leave that to project specific ```.gitignore``` files.


The other thing that's nice to do is clean up your default prompt. Notably, git bash by default includes ```MINGW64``` in the prompt. I guess this is the ```$MSYSTEM$``` environment variable, but I can't imagine why I'd care to see that in my prompt. The other stuff it includes by default are pretty handy, but if you don't like them you can modify the same file I'm going to point to to update your setup.

The file that contains your prompt information should be in either ```%UserProfile%\AppData\Local\Programs\Git\etc\profile.d\git-prompt.sh``` or ```C:\Program Files\Git\etc\profile.d\git-prompt.sh```. This is a standard bash script, so if you're familiar with bash scripting, or modifying your bashrc in Linux or Mac this will be familiar. If not, it's generally pretty readable. Make a backup of it and fiddle. To get rid of the ```MINGW64``` we just have to find the lines that say

```bash
PS1="$PS1"'\[\033[35m\]'       # change to purple
PS1="$PS1"'$MSYSTEM '          # show MSYSTEM
```

and delete them or comment them out. I also got rid of

```bash
PS1="$PS1"'\n'                 # new line
```

so that my conda environment would be on the same line as the rest of my setup info.

### Set up ssh

This will actually work exactly the same as Linux, which is nice. GitHub has nice [docs](https://help.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh) on how to generate keys and associate them with your GitHub account, so I won't reiterate that here. Anything you can connect to via SSH rather than password you should though. It's more secure, and more convenient.

#### SSH note

I had an old install of putty when I first set up git bash. Even though I told it to use OpenSSH I guess I still had putty set somewhere in my environment. I had to modify the ```GIT_SSH``` environment variable for my system to point to the git ssh utility, which in my case was at ```C:\Program Files\Git\usr\bin\ssh```.

### Further reading

The [main git page](https://git-scm.com/) has tons of resources. I've also collected a few that I found useful under my [Tagpacker page.](https://tagpacker.com/user/ian.preston?t=git)

## Install Miniconda

Next up we install Miniconda to handle python and all its libraries for data science. You could go with Anaconda over Miniconda for a complete setup out of the box, but I want to do all my actual work in custom environments, so having all that stuff in the base environment is just bloat. Open up the link to the [Win64 MiniConda installer](https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe), download and run the installer.

I've found that even if I do a user level install I still get prompted for admin escalation during environment management. Might as well do a system level install.

![](/images/windows_ds_software/conda_01.PNG "conda user")

Default install path should be fine.

![](/images/windows_ds_software/conda_02.PNG "conda path")

Check the box to add Anaconda to the system PATH environment variable, this will allow you to use conda from git bash.

![](/images/windows_ds_software/conda_03.PNG "conda syspath")

Finally, open up git bash and run ```conda init bash```. You'll be prompted for admin privileges, but once you're done you should be all set to use conda commands from git bash.

There's one last thing we'll have to do. VS code expects ```activate``` to be the command used to activate a conda environment, but in git bash it's actually ```source activate```. What we do is add an alias in the bash profile so that running ```activate``` translates to ```source activate```.
In ```%UserProfile%``` you might already have a file named ```.bash_profile``` from when you ran ```conda init bash```. Add a line with the alias below. Here's what my full file looks like:

```sh
# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
eval "$('/c/ProgramData/Miniconda3/Scripts/conda.exe' 'shell.bash' 'hook')"
# <<< conda initialize <<<
alias activate="source activate"
```

Note, if you don't already have the file, then similar to the global gitignore section, you can run ```touch ~/.bash_profile``` to create the file, and then update it in whichever editor you want. You only have to add the ```alias part```, if the conda initialize stuff isn't there don't worry about it. It wasn't on my home computer, it was on my work one. I'm not sure why.

That should take care of it for conda installation

### Actually building environments

Conda environment management is a big separate topic. [Their documentation](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html) is really good, and I refer to it regularly. A write up on my particular environment management strategy will get linked in here if/when I write it up.

## Back to VS code

### Extra extensions

These ones require a bit more configuration than the ones above, but I think they're worth it.

#### SQL Server

If you're getting data from a MSSQL database then this is great. It adds another icon on your sidebar where you can browse various databases in your organization. Great for a quick check on table structure. I think you can use it to actually write queries too, but I haven't fiddled with that.

You can install it by searching for ```mssql``` in the extensions sidebar. Once it's installed you'll get a new icon on your sidebar at the bottom:

The first time you click it it will take a while, and do some installations in the background. After that you can click the "Add Connection" button at the top and add a path to a database.

First type in the path to the server, optionally add the name of the specific database. If you use your windows credentials to connect you can use integrated, which is nice. Finally, give it a nice name.

#### Vim integration

If you don't know what vim is, or do know and are scared of it, just skip this step. If you've trained yourself to use vim keyboard shortcuts though, you're definitely going to want that in your editor. Previously I was using the Vim extension, which was decent, but I did find a little laggy. Recently I've switched to neovim. It takes a little more up front configuration, but ultimately gives a nicer experience. For one thing, it's more responsive. For another, when in insert mode all the regular VS code shortcuts work. Finally, when you enter commands they show up in the big box at the top of the VS code window like other VS code commands, rather than down in the tiny bar at the bottom. That's particularly nice when you're trying to write out a big find replace or something.

First, grab the [Neovim installer](https://github.com/neovim/neovim/wiki/Installing-Neovim) and extract it somewhere (maybe your home folder).

Next install the Neo Vim extension, and hit the little cog beside it to open its configuration. Head down to the NeoVim path component and paste in the path to nvim.exe (should be in the bin folder that you just extracted). Restart VS code and you should have vim bindings. You could also add the ```bin``` folder from your download to your ```PATH```, definitely a good idea if you want to use neovim directly. You could go further and alias vim to neovim in git bash (just like in the last part of the miniconda installation instructions).

The last part is to have Neovim use the system clipboard as its register so you can copy and paste between VS code and other applications.

To do this you have to do a few things (at least that's how I got it working)
First, create an init.vim file in ```%UserProfile%/AppData/Local/nvim```
Mine looks like this:

```bash
set runtimepath+=~/vimfiles,~/vimfiles/after
set packpath+=~/vimfiles
source ~/_vimrc
```

then in ```%UserProfile%/_vimrc``` add the following line:

```bash
set clipboard+=unnamedplus
```

#### Remote development

If you're planning to do "remote" development, where remote can also mean docker containers or anything running in WSL, you will want to install the remote development extension pack. You could pick and choose just the ones you need, or grab the extension that ships the whole pack. Not a big deal either way.

### User settings

VS code comes mostly with sensible defaults, but there are a few things I like to change. In VS code hit ```F1``` and type ```Open settings (JSON)```. If you don't see that option (it didn't pop up for me the first time I tried it) just open settings and look for a setting that tells you to update it in ```settings.json```. Below are my settings, minus the stuff that just got added through configuring the extensions described above:

```json
{
    "diffEditor.renderSideBySide": true,
    "workbench.editor.enablePreviewFromQuickOpen": false,
    "workbench.editor.enablePreview": false,
    "git.autofetch": true,
    "editor.suggestSelection": "first",
    "editor.rulers": [88],
    "editor.acceptSuggestionOnEnter": "off",
    "editor.minimap.enabled": false,
    "explorer.confirmDelete": false,
    "terminal.integrated.shell.windows": "C:\\Program Files\\Git\\bin\\bash.exe",
    "terminal.integrated.env.windows": {
        "CHERE_INVOKING": "1"
    },
    "terminal.integrated.shellArgs.windows": [
        "-l"
    ],
    "vsintellicode.modify.editor.suggestSelection": "automaticallyOverrodeDefaultValue",
    "editor.lineNumbers": "relative",
    "[python]": {
        "editor.defaultFormatter": "ms-python.python"
    },
    "python.formatting.provider": "black",
    "python.linting.enabled": true,
    "python.linting.flake8Enabled": true,
    "search.exclude": {
        "**/node_modules": true,
        "**/bower_components": true,
        "**/env": true,
        "**/venv": true
    },
    "files.watcherExclude": {
        "**/.ipynb_checkpoints/**": true,
        "**/$tf/**": true,
        "**/.git/objects/**": true,
        "**/.git/subtree-cache/**": true,
        "**/node_modules/**": true,
        "**/env/**": true,
        "**/venv/**": true,
        "**/.hypothesis/**": true,
    },
    "files.exclude": {
        "*.sublime-*": true,
        "**/__pycache__": true,
        "**/.DS_Store": true,
        "**/.git": true,
        "**/.hypothesis/**": true,
        "**/.ipynb_checkpoints": true,
        "**/.pytest_cache": true,
        "**/.vscode": true,
        "**/*.log": true,
        "**/*.lst": true,
        "**/$tf": true,
        "**/node_modules": true,
        "venv": true
    },
}
```

Most of the settings have fairly clear names, and if you put them in your ```settings.json``` you'll get a little mouseover tip that will tell you what they do. 

**Note**: If your git bash is a user level install, or you picked somewhere else to install it you'll have to modify that path.

### Further Resources

There's tons of stuff to learn about VS code to make it super handy. At a minimum, check out their [keyboard shortcuts](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf). I'm collecting other useful resources (with a decent amount of overlap with vim stuff) on [my tagpacker](https://tagpacker.com/user/ian.preston?t=vs_code).

## Wrapping up

That's about it! After following this guide you should have a very comfy software setup in Windows to do data science from.
