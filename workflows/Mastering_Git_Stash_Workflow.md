## Mastering Git Stash Workflow

Git is a powerful tool that makes up for a lot of use cases on our development workflow. One such case is to isolate the changes of a certain branch to itself. Let me elaborate.

Suppose you are working on a branch called `admin-dashboard` implementing ,well, an administrative dashboard. But you are not done yet, and the project manager wants a quickfix for the login implementation. Now you want to switch to the `login` branch and fix the issue but don’t want to carry the changes you are doing on the `admin-dashboard` branch. Well this is where git stash comes in.

### **Git stash what?**

Git stash allows you to quickly put aside your modified changes to a LIFO (Last In First Out) stack and re-apply them when feasible. We will be doing a rough walkthrough to see this in action.

## **Git stash walkthrough** 🚶‍♂️

Initiate git on an empty directory. Add a file named `add.py` and put the following code.

```python
# add.py

def add(a, b):
    return a + b
```

Let’s add the file to git and commit it .

```bash
git add add.py && git commit -m "Add function"
```

Next let’s create and checkout to a new branch called `mul` and create a file called `mul.py`

```bash
git checkout -b mul
```

Add the following code to the file.

```python
# mul.py

def mul(a, b):
    return a * b
```

Add the file to git and commit it.

```bash
git add mul && git commit -m "Mul function"
```

Now suppose, we need to update the mul function to take in a third argument and you just edited the function like so:

```bash
# mul.py

def mul(a, b, c):
    return a * b
```

Take note that, we still haven’t updated the return value with `c`. While we were making our changes, the project manager called-in to update the `add` function immediately with a third argument. Now you can’t waste a second on the `mul` function which you haven’t completed. **What are you going to do?**

If you try to checkout to master where add function resides, git won’t let you because you have unfinished changes that hasn’t been committed.

![](https://cdn-images-1.medium.com/max/2176/1*Sm1VcF-XIR9CZKJRrd9V0w.gif)

### **Stashing Changes** 📤

Well this is the situation you should be using the `git stash` command. We want to put away the changes on our current branch so that we can come back to it later.

Stash the file with a message on the `mul` branch.

```bash
git stash save "Multiply function"
```

Now if you `git status`, the working directory will be clean and you can jump into the master branch for the changes. We can view the items in our stash using `git stash list`. We can view the diff of the items on our stack using `git stash show`

![](https://cdn-images-1.medium.com/max/2176/1*1w-UUFmluedhcbGROyvi2A.gif)

Now you are in `master` branch modifying the `add` function, and comes an even higher priority job to include a `subtraction` function.

> **Note**:
> Project Managers in real life doesn’t put forward tasks on this manner.

Your `add` function looks pretty much like the `mul` function now.

```python
# add.py

def add(a, b, c):
    return a + b   # couldn't include `c` due to a priority task.
```

Stash it with a message.

```bash
git stash save "Add function third argument"
```

![](https://cdn-images-1.medium.com/max/2176/1*JPNqrbe449Konsoxrdg3-g.gif)

Now let’s suppose we have completed our task for `subtraction` branch and we want to continue working on other branches.

Let’s move to the `master` branch first to complete our changes for addition.

There are two ways we can re-apply those changes:

1. `git stash pop `
   applies the top most change stored on the stack and removes it from the stack.

1. `git stash apply <item-id>`
   applies the stash based on the index provided, keeps the applied item intact on the stack.

### **Popping stashed changes** 🍾

Like I mentioned earlier, stash follows the LIFO convention. The latest item we save are always on the top. And when we use `pop`, always the top most change is applied to the current branch. Run the following command on `master`.

    git stash pop

![](https://cdn-images-1.medium.com/max/2176/1*rL3Sk-m74A8v-GYGbxKkOg.gif)

Complete the `add` function and commit it.

### **Applying stashed changes** 📥

Next, checkout `mul` branch. We can use `pop` here as well since there is only one item remaining on the stack. But let’s see how `git stash apply` works.

```bash
git stash apply stash@{0}
```

![](https://cdn-images-1.medium.com/max/2176/1*VI7FJ2kNfy4_KWZCQ5WGVA.gif)

When we apply from the stash, the item still remains on the stack.
Complete the changes here and commit it.

### **Create a new branch with stashed changes**

Let’s do something fun. Let’s say we want to add a `divide` function on a new branch. Well it is kind of similar to our `multiple` function so why not utilize the item in stash and create a `divide` function with it?

We can do that with the `git stash branch` command. It takes the `<item-id>` and a branch name, then ***applies*** those changes to that branch.

```bash
git stash branch <branch-name> <item-id>

git stash branch divide stash@{0}
```

![](https://cdn-images-1.medium.com/max/2176/1*ROehknP7W7-a8v6Rdtr_sQ.gif)

Now we can rename the file and change the function to perform division.

### **Clearing the stack** 🧹

Now that the stash has served its purpose, we can clear it. There is again two ways of doing it:

1. `git stash clear`
   wipes the whole stack clean.

1. `git stash drop <item-id> `
   removes the item from stack based on provided id.

### **Conclusion** 🚀

Git stash is a powerful tool that comes handy in many situations. Hope this article helped you in understanding and implementing the concept in your project. If you have any suggestions or feedback, let’s talk on the comment section or you can [@ me on Twitter](https://twitter.com/yankexe).

Follow me on [GitHub](https://github.com/yankeexe)
