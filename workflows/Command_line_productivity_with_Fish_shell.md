##  Command line productivity with Fish shell

There is more than one way to get around things from the terminal. That’s the beauty of it; endless customization and tweaks to get things done faster and more efficiently.

In my journey of using WSL, I bumped into performance issues with the beloved zsh and oh-my-zsh combo. Wandering around for an hour, I stumbled upon fish shell, which was 10x faster in WSL compared to zsh (no real metrics here, but felt really really fast 😅🚀). But it comes with its own set of quirks which we’ll see later.

## **Install Fish Shell** 🐟

If you are on Debian-based distros:

    $ sudo apt-get install fish

If you are on other platform, [follow the instructions here](https://github.com/fish-shell/fish-shell#getting-fish).

### **Fish Shell Intro**

Fish shell out of the box is lightweight, performant ⚡ and full of features. Meaning you don’t have to do much to make yourself productive in the terminal. Some of the native functionalities include:

* syntax highlighting,

* autosuggestion

* tab completion and

* auto loading functions.

The documentation is rich and elaborative, and you can get the documentation hosted locally in the browser by writing help in the fish shell. For most people a fresh install would be enough to get going. But we want to tweak and make it more functional with plugins, themes and what not. Like other shells, fish comes with a lot of plugin framework , but we will be using called [oh-my-fish](https://github.com/oh-my-fish/oh-my-fish) (omf).

## **Intro omf**

omf is a thin layer on top of fish shell, so you are not compromising on speed and performance. Install it with a single command:

    $ curl -L https://get.oh-my.fish | fish

With this done you will have the omf command on your shell to install themes and other useful plugins. It is really intuitive and if you have used nvm or pip you will feel right at home.

You can get the basic run down of the [commands here](https://github.com/oh-my-fish/oh-my-fish#getting-started).

### **themes with omf 🎨**

There are a variety of themes you can choose from. You can find the [themes hosted here,](https://github.com/oh-my-fish/oh-my-fish/blob/master/docs/Themes.md) or you can use the command omf theme to list all of them and also view the installed and default theme.

Installing a new theme will directly apply that theme.

    $ omf install <theme-name>
    $ omf install bira

![Listing themes and changing between installed ones. 🎨](https://cdn-images-1.medium.com/max/2032/1*RFN2ONxk2-Lzn_K9uUptzg.gif)*Listing themes and changing between installed ones. 🎨*

If you have multiple themes installed, you can change between them using:

    $ omf theme <theme-name>

### **aliases with omf 🍦**

aliases are handy when you want do complete repeated tasks with as few keystrokes as possible. Fish shell has a command named alias to define your aliases. You can simply do it from the command line.

    $ alias <alias> '<command>' -s
    $ alias install 'sudo apt-get install' -s
    $ alias remove 'sudo apt-get remove --purge' -s

You can install any packages with the install alias or uninstall it completely using the remove alias.

    $ install vim
    $ remove python2.7

![Creating alias `install` ✍](https://cdn-images-1.medium.com/max/2032/1*WThV-osTr7qeVxtDH7cSiw.gif)*Creating alias `install` ✍*

Aliases you create using the alias command only lasts for a session, meaning if you spawn a new terminal instance they won’t work. To make it work, we have to pass the -s flag. This will run funcsave <alias> behind the scene.

Now it is permanently set and can be used in any instance. In case you forget your aliases, you can summon them using alias command.

![Listing all aliases. 📃](https://cdn-images-1.medium.com/max/2032/1*vQumglI-6OpHawA-I5L9nA.gif)*Listing all aliases. 📃*

### **working with nvm 🔧**

One of the quirks of fish shell is that it cannot run few bash utilities natively like [nvm](https://github.com/nvm-sh/n) . For this you need a package named [bass](https://github.com/edc/bass)to expose nvm to fish shell.

basscreates a framework to support other bash utility packages, like the one we will be using called [fish-nvm](https://github.com/jorgebucaran/fish-nvm) . There are other packages for nvm but fish-nvm doesn’t bottleneck the performance.

    $ omf install bass
    $ omf install https://github.com/FabioAntunes/fish-nvm

**working with virtualenv 🐍**

This is just one of the gotchas of working with fish shell. When you are working with the Python virtual environment , you cannot activate is the normal way.

    $ python -m venv venv
    $ source venv/bin/activate

Instead, fish shell adds a script called activate.fish to enable the shell.

    $ source venv/bin/activate.fish

## Essential Packages 📦

### [pj](https://github.com/oh-my-fish/plugin-pj)

pj allows you to easily jump between your favorite directories in a predictable manner. You tell pj where to look for your projects/folders, and it will allow you to jump there easily with tab completion.

    $ omf install pj

For example: I have a folder called test in my home directory with a bunch of other folders.

![](https://cdn-images-1.medium.com/max/2000/1*WGRQj64vuEugFL_z2Ffzfw.png)

To mark the test folder as jump target we need to set the project path:

    $ set -Ux PROJECT_PATHS ~/test

Now I can jump between the folders inside the test directory from any location in my terminal.

![pj in action! ⚡](https://cdn-images-1.medium.com/max/2272/1*jI0uddiur0aJXbsjmz1y_g.gif)*pj in action! ⚡*

### [**z**](https://github.com/jethrokuan/z)

z is similar to pj but it is intelligent in a sense that it keeps track of your most visited folders so you can jump in that location easily.

    $ omf install z

Like I said, z is an intelligent tool, even if I make a typo that may closely resonate to your most visited folder name, it will try to resolve it navigate to that folder.

![z in action! ⚡](https://cdn-images-1.medium.com/max/2000/1*x8va4Ph_V_ADbMSre0PasA.gif)*z in action! ⚡*

### [plugin-git](https://github.com/jhillyerd/plugin-git)

Like the git plugin in zsh, the plugin-git package gives you the standard set of git aliases to accelerate your git workflow.

    $ omf install https://github.com/jhillyerd/plugin-git

![plugin-git in action! ⚡](https://cdn-images-1.medium.com/max/2000/1*N9xOm9M149wtYDQdlT7B3w.gif)*plugin-git in action! ⚡*

Not only that, to make sure you are using the right alias, it also expands the alias to form the complete command. Explore the [complete list of aliases here](https://github.com/jhillyerd/plugin-git#usage).

### [fzf](https://github.com/jethrokuan/fzf)

Fuzzy Finder or fzf is a general purpose command line tool to find anything faster, be it your files or the command history.

    $ omf install https://github.com/jethrokuan/fzf

To search through your command history, you can use the combo `ctrl + r` or write some part of the command and hit the combo to find only the commands that match your query.

![fzf in action! 🔍](https://cdn-images-1.medium.com/max/2000/1*lOEP-GYgbG07VnTca6o18g.gif)*fzf in action! 🔍*

If you want to search for files in the current directory then you can hit `ctrl + o` and browse through them. You can do much more with this tool, [checkout the usage section here](https://github.com/jethrokuan/fzf#usage).

### Conclusion 🙏

I hope this article has been a good hands on guide on installing fish shell and making your workflow productive. If you have suggestions or queries, feel free to comment down below.
