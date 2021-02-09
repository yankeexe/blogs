## Command Line Productivity with ZSH Aliases

If you are on Linux, then the terminal is where you likely do most of your stuff. Working with CLI is cool, but writing commands after commands is somewhat draining sometimes. That’s why we have aliases so that we don’t have to write long lines of command repeatedly. With just a few keystrokes, we can get our work done, which feels like a CLI magic.

Most of our OS comes default with the bash shell, but there is only so much you can do with it. From the usability and customization perspective, one of the most popular options out there is the **zsh** or the **z-shell**. If you haven’t installed it already, you can do so within a minute. If you have installed it, you can skip this part.

###  ZSH Installation

```bash
$ sudo apt-get update

# install zsh
$ sudo apt-get install -y zsh

# check the version
$ zsh --version

# change your shell to zsh
$ chsh -s $(which zsh)
```

The best thing about **zsh** is that it comes with a whole plugin ecosystem.
I prefer to use **oh-my-zsh** to handle plugins. If you haven’t installed it, do so with this single line.

```bash
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

Now that we are done installing let’s move to our core topic —** “Aliases.”** Without aliases, finding the right command turns into an endless pursuit of pressing the up arrow key to return to previous commands, grepping the history or typing the same thing time and again.

**Today I will be discussing mainly 5 things related to zsh alias, they are:**

* Normal Alias

* Suffix Alias

* Global Alias

* Alias with parameters

* Global Alias Expansion

**Basic syntax of zsh alias:**

Alias basically has four parts:

> alias [flag] [custom-alias]="[command]"

1. alias keyword

1. flag

1. custom alias

1. a command for the custom alias

### **Note:**

Your zsh configuration file is basically sitting on your home directory. To add aliases, you need to open that file and start adding alias at the bottom (just so we don’t get confused).

**Open zsh configuration file**:

```bash
$ sudo vim ~/.zshrc
```

**Apply the changes:**

```bash
$ source ~/.zshrc
```

or

```bash
$ . ~/.zshrc
```

or you can restart your terminal session.

> Let’s begin

## Normal Alias

For the normal alias, we do not need a flag. Make sure the alias you put makes sense; it’s like naming a variable, which is an art in itself. Let’s begin, with our first alias example:

**Syntax**

> alias [custom-alias]="[command]"

**Example 1**

Here we will create an alias for installing new packages. At the end of the file write:

```bash
alias install="sudo apt-get install"
```

Now in the terminal, you can just write:

```bash
$ install unoconv
```

 and it translates to: `sudo apt-get install unoconv`. Don’t forget to `source ~/.zshrc` before you run your alias.

**Example 2**

Similarly, if you want an alias for removing any package, we can create a new one:

```bash
alias remove="sudo apt-get remove --purge"
```

We can make as many aliases as we like to do anything that we can think of, but remember not to use any reserved keywords!

## Suffix Alias

Suffix alias is used for assigning an app to open a particular file type. If you want to **open different file formats** with different applications, then suffix alias will help you do just that. It may sound like setting default applications on your OS, but we are talking about individual file extensions here.

In suffix alias, we will make use of the **flag -**s .

**Syntax**

> alias -s [file-extension]=[name-of-app]

**Example**

Here I will demonstrate how you can open two file types with two different applications. You can try with more file types!

1. Open **.txt** files in [vscode](https://code.visualstudio.com/download)

1. Open **.md** files in an app called [typora](https://typora.io/)

```bash
alias -s txt=code

alias -s md=typora
```



Now on your terminal, you can write **just the file name** and based on its extension it will open up in your desired application.

Writing `readme.md` will translate to `typora readme.md`

## Global Alias

Global alias are those aliases that can be used in more than one command. You can use it more than once in a single command or use it anywhere within the command as it fits, except at the very beginning.

You can create global aliases of frequently used commands like those after the `|(pipe)` symbol or you can assign your IP address or some text that might come often when you are working.

You can name your global alias however you want, but I prefer writing it capitalized as it distinguishes them from the normal aliases. Also, it will come handy when we talk about the **global alias expansion** later on.

**Syntax**

Here we make use of the -g flag to represent a global alias.

> alias -g [custom-alias]="[Command]"

**Example**

```bash
alias -g G="| grep"

alias -g L="| less"

alias -g GG="google.com"
```

In terminal:

- Use of first alias:

```bash
$ apt-cache search vlc G data will translate to

$ apt-cache search vlc | grep data
```

- Use of second alias:

```bash
$ cat readme.md L will translate to

$ cat readme.md | less
```

- Use of third alias

```bash
$ ping GG will translate to

$ ping google.com
```

## Alias with parameters

To be more flexible with using aliases, you would want to use parameters. Suppose you want to search for a package and grep something specific, how would you do that with single normal alias? Well, you can’t! That’s why we need parameterized aliases. You can imagine them like the functions in a programming language.

In ZSH, you need to create an alias using a function to support parameters.

**Syntax**

    [alias-name]() {
        command $param1 | $param2
    }

**Example**

Let’s take the same example we have taken on global alias and convert it to parameterized alias. I will use the shorthand of acs for apt-cache search

```bash
acs() {
    apt-cache search $1 | grep $2
}
```

Here `$1` and `$2` represents the order in which your command is placed. So now, we can do:

`$ acs vlc data` which translates to:

```bash
$ apt-cache search vlc | grep data
```

If you use `$1` in more than one place while specifying the alias then the same value will be applied to the next instance where you have mentioned it. Meaning you don’t have to type the same thing again!

## Expanding Global Alias

More often than not, you would want to know what you are doing or what’s really behind that alias. Global alias does make your commands concise but there are times when you would want to see the complete commands. If that’s the case with you, then let’s see how to expand the Global alias.

There is a function created by **Pat Regan **to expand the global alias which uses globalias function provided by zsh. For this to work, you can copy and paste the code on the bottom of your `.zshrc` file and you are good to go. And this works only for the global alias set with capital letters.

The globalias plugin was supposed to do this for normal and global alias but it didn’t work for me so hence this fix.

```bash
globalias() {
   if [[ $LBUFFER =~ ' [A-Z0-9]+$' ]]; then
     zle _expand_alias
     zle expand-word
   fi
   zle self-insert
}

zle -N globalias

bindkey " " globalias
bindkey "^ " magic-space           # control-space to bypass completion
bindkey -M isearch " " magic-space # normal space during searches
```

## Conclusion

There are thousands of possibilities on how you can create your own alias and use it to maximize your productivity. This article summarizes some of the things you can take into consideration while making them.

If you have suggestions or queries, feel free to comment down below.

