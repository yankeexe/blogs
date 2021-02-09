##  Faster Git workflow with Git Aliases

Git is an intricate part of our development workflow. There are handful of commands that you keep repeating day in and day out. I always relied on the command suggestions, or packages on top of my shell that gave access to handy git aliases.

But usually you had to stick with the aliases decided by the package creators . Although most of the aliases included are unofficially globally accepted like `ga` for `git add` and so on.

But guess what? You don’t have to rely on any third party packages ; you can create your own with the aliases you prefer!

## **There are two ways of creating Git aliases: ✌**

### **with the git config utility** 💻

This is the preferred way of adding aliases as `git` gives you an option to do so.

Suppose you feel tired of repeating the **commit** command every now and then. It would be nice if we could create an alias to write our commits faster.

**Say no more! 🎉**

```bash
git config --global alias.c "commit -m"
```

The above command follows the following syntax:

```bash
git config --global alias.<alias> "<command>"
```

Now we can use the `c` alias to represent `commit -m` .

```bash
git c "Update readme with social links"
```

### **editing the .gitconfig file** 📝

It would be tedious to add multiple aliases with the `git config` command, so there’s an easier alternate approach.

All the alias you create is saved to the `.gitconfig` file sitting in your home directory. We can open the file and add our aliases following the [TOML ](https://github.com/toml-lang/toml)formatting. Make sure you do not modify anything in the file apart from adding the `[alias]` table and its contents.

Open the file using your favorite editor.

```bash
vim ~/.gitconfig
# or
code ~/.gitconfig
```

Start adding your alias ✍️

```toml
...
[alias]
        st = status
        c = commit -m
        a = add
        cb = checkout -b
```

You can use the above aliases as follows:

```bash
git st # git status
git c "hello world" # git commit -m "hello world"
git a hallucination.py # git add hallucination.py
git cb multi-stage-build # git checkout -b multi-stage-build
```

There’s also a handy command: `git config --list` to view the contents of the file including other git configurations.

## **Bonus** 🛸

### **Git alias with parameters!**

We can take our alias a step further by using shell script to add parameters.

**Where do we benefit from the parameters?**
Let’s take an example of adding a git remote to our local repository. There are basically two variables in the command that I can pass as a parameter, namely, the **remote name** and **project name.**

```bash
git remote add <remote-name> git@github.com:yankeexe/<project-name>.git
```

We can represent the above abstraction using an anonymous bash function `f()`as follows:

```toml
[alias]
        ra = "!f() { git remote add $1 git@github.com:yankeexe/$2.git; };f"
```

The `$1` and `$2` is the order in which the parameter will be used by the function. `!` represents a shell script; our anonymous function is `f()` which we have invoked immediately at the end.
Let's use our alias:

```bash
git ra origin demo-project
```

Here **origin** will be used in `$1` since it’s the first parameter we are passing and `$2` will be **demo-project**.
The above command will translate to:

```bash
git remote add origin git@github.com:yankeexe/demo-project.git
```

### **Conclusion** 🚀

I hope this article has been of help in improving your Git workflow. If you have any queries or suggestions, let’s discuss in the comment section.
