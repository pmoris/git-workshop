# Branching

This section will teach you the basics of working with branches, which are another very neat feature for collaborating with git.

---

As we discussed earlier, git branches are just references to particular commits ([see image in introduction](fig-branch-target)). Consequently, multiple branches can point to the same commit, branches can be ahead of others, or they can be on separate "tracks" of the commit history graph.

Similarly, remote branches, denoted by `<remote>/<branch>`, are also just pointers. They show you which commits the branches in your remote repository were pointing to, the last time you communicated with that remote (via `git fetch` or `git pull`), more or less acting like bookmarks.

Lastly, keep in mind that these remote branches do not necessarily need to have the same name as your local branches, although it is recommended that they do. E.g. a local branch `dev` should be setup to track a remote branch named `origin/dev`, but it could point to `origin/master` as well if you feel like messing with people (don't do this, please).

Why would we want to use branches in the first place though? Well, they allow for more granular control of changes and make it easier to work on separate features independently. Here is a great example borrowed from the Git Pro book:

> Let’s go through a simple example of branching and merging with a workflow that you might use in the real world. You’ll follow these steps:
>
>   - Do some work on a website.
>   - Create a branch for a new user story you’re working on.
>   - Do some work in that branch.
>
> At this stage, you’ll receive a call that another issue is critical and you need a hotfix. You’ll do the following:
>
>   - Switch to your production branch.
>   - Create a branch to add the hotfix.
>   - After it’s tested, merge the hotfix branch, and push to production.
>   - Switch back to your original user story and continue working.

In the workshop we will introduce some of the basic branching commands that you might need. Afterwards we will walk through a few different scenarios of how they can be used to collaborate.

```{seealso}
More in-depth information on branching can be found in these excellent resources:
- [https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell)
- [https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
- [https://git-scm.com/book/en/v2/Git-Branching-Remote-Branches](https://git-scm.com/book/en/v2/Git-Branching-Remote-Branches)
- [https://www.atlassian.com/git/tutorials/using-branches](https://www.atlassian.com/git/tutorials/using-branches)
```

## Creating a new branch

`````{tab-set}

````{tab-item} Exercise

```{admonition} Instructions
:class: hint
- Create a new feature branch and switch to it.
- Create or modify files and commit them.
```

````

````{tab-item} Solution

There are a couple of different ways to create branches:

- `git branch <branch-name> <start-point>` simply creates a new branch, without switching to it. (HEAD is used as a starting point by default)
- `git switch -c <new-branch> <start-point>`: creates a new branch and switches to it.
- `git checkout -b <branch-name>` creates a new branch and immediately switches (`checkout`) to it (replaced by `switch`).

```{seealso}
The `switch` command was introduced in git v2.23 and it offers a more streamlined and safer alternative to `checkout`, which serves multiple different purposes:
- [https://www.git-tower.com/learn/git/commands/git-switch](https://www.git-tower.com/learn/git/commands/git-switch)
- [https://stackoverflow.com/questions/57265785/whats-the-difference-between-git-switch-and-git-checkout-branch](https://stackoverflow.com/questions/57265785/whats-the-difference-between-git-switch-and-git-checkout-branch)
- [https://stackoverflow.com/a/66309040](https://stackoverflow.com/a/66309040)

In a nutshell, avoid using `checkout` if you are new to git, since it servers multiple different purposes. Instead, use `switch` to safely switch between branches (or commits) (which `checkout` could also do). Use `restore` when attempting to reset files in your working directory and/or index (which `checkout` and `reset` could also do).
```

```bash
git switch -c things
echo "raindrops on roses and whiskers on kittens" > my-favorite-things.txt
git add -A
git commit -m "List favorite things"
cat >> my-favorite-things.txt <<EOL
- https://i.imgur.com/xDfo0.gif
- https://i.imgur.com/XX6ZxA7.mp4
EOL
git add my-favorite-things.txt
git commit -m "Add more favorite things"
```

````

`````

## Moving between branches

`````{tab-set}

````{tab-item} Exercise

```{admonition} Instructions
:class: hint
- List all branches in your repository.
- Switch between different branches and see how the `git log` changes.
- Try switching between branches when there are uncommitted changes.
```

````

````{tab-item} Solution

To list the existing branches in your repository, you can use the following command:

```bash
git branch -vv
```

The `-vv` flag is optional, but it comes with the bonus of showing the most recent commit on that branch (or more accurately, the commit that that branch is pointing to), as well as listing any remote tracked branches between brackets.

```bash
$ git branch -vv
  master 94af264 [origin/master] Add cake declaration
* things 1dfd056 Add more favorite things
```

We can use `git log` to review some of the things we have discussed earlier. Git uses a history of commits that point to one another. Branches are pointers to specific commits, in our case `things` and `master` have diverged and they point to different snapshots (`things` has additional newer commits compared to `master`). `HEAD` points to the branch (or commit) that is currently checked out (i.e. it produces the initial contents of your working directory and provides a frame of reference to compare against when introducing new changes).

```bash
$ git log
commit 1dfd056e7db978c24cdeaae8ee44b5b3aa2498f3 (HEAD -> things)
Author: Pieter Moris <13552343+pmoris@users.noreply.github.com>
Date:   Mon Oct 3 15:06:10 2022 +0200

    Add more favorite things

commit 6b5733bc18080a8f5c0c2a5601f35dbad1edae01
Author: Pieter Moris <13552343+pmoris@users.noreply.github.com>
Date:   Fri Sep 30 16:21:07 2022 +0200

    List beautiful things

commit 94af264c0ec9e4560d23c8bbacbb4370b9e20b65 (origin/master, new, master)
Author: Pieter Moris <13552343+pmoris@users.noreply.github.com>
Date:   Wed Sep 7 15:48:49 2022 +0200

    Add cake declaration

commit b2c2e84a1c0deebd6199e790f0db133fc0e0853c
Author: Pieter Moris <13552343+pmoris@users.noreply.github.com>
Date:   Wed Aug 17 12:30:36 2022 +0200

    Create a vector

    Adds an R script to create a nifty vector
    and talks about it in the readme.

...
```

When we `git switch master` and call `git log` again, we can see that `HEAD` moves around. Additionally, the log will only show you info that it deems relevant. After switching to `master`, it will only show you commits that are below it. You can use the `--all` flag to change this.

```bash
git switch master
git log
```

```bash
$ git switch master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.

$ git log
commit 94af264c0ec9e4560d23c8bbacbb4370b9e20b65 (HEAD -> master, origin/master, new)
Author: Pieter Moris <13552343+pmoris@users.noreply.github.com>
Date:   Wed Sep 7 15:48:49 2022 +0200

    Add cake declaration

commit b2c2e84a1c0deebd6199e790f0db133fc0e0853c
Author: Pieter Moris <13552343+pmoris@users.noreply.github.com>
Date:   Wed Aug 17 12:30:36 2022 +0200

    Create a vector

    Adds an R script to create a nifty vector
    and talks about it in the readme.

commit 02b6530617bd305f686df19fbbaac4689f5e5cf1
Author: Pieter Moris <13552343+pmoris@users.noreply.github.com>
Date:   Wed Aug 17 12:19:18 2022 +0200

    Initial commit
```

Lastly, attempting to switch between branches when there are uncommitted or staged changes, will result in a warning:

```bash
echo "- https://www.youtube.com/watch?v=3GRSbr0EYYU" >> my-favorite-things.txt
git switch master
```

```bash
error: Your local changes to the following files would be overwritten by checkout:
        my-favorite-things.txt
Please commit your changes or stash them before you switch branches.
Aborting
```

You can fix this by either staging and committing your changes, restoring the file to its previous state, or by using `git stash`. The latter option is incredibly useful and I highly suggest you to learn more about it. It will make your life so much easier.

````

`````

## Merging a branch

At some point, you will want to merge your branches back together. There is a lot of discussion on the how and when of properly merging branches. These branching strategies are given fancy names like _git flow_ and _trunk based development_, but we will not delve into them at this point. Instead, we will focus on the more simple situation where we have 1 main branch (`master` or `main`) and several smaller feature branches.

`````{tab-set}

````{tab-item} Exercise

```{admonition} Instructions
:class: hint
- Compare the differences between the two branches.
- Merge your feature branch into `master`.
- Check the git log again and use the `--graph` flag to get a better feel for the acyclic graph that makes up the git history.
```

````

````{tab-item} Solution
We can use `git diff` to compare all the changes between the two branches:

```bash
git diff things master
```

```bash
diff --git a/my-favorite-things.txt b/my-favorite-things.txt
deleted file mode 100644
index e61b78b..0000000
--- a/my-favorite-things.txt
+++ /dev/null
@@ -1,3 +0,0 @@
-raindrops on roses and whiskers on kittens
-- https://i.imgur.com/xDfo0.gif
-- https://i.imgur.com/XX6ZxA7.mp4
```

To merge our changes into the master branch, we first `switch` to it and then call `git merge`:

```bash
git switch master
git merge things
```

```bash
Updating 94af264..1dfd056
Fast-forward
 my-favorite-things.txt | 3 +++
 1 file changed, 3 insertions(+)
 create mode 100644 my-favorite-things.txt
```

Now the `git log` will show us that both branches have landed on the same commit, i.e. the changes introduced in the `things` branch are now part of `master` too. In fact, only `master` moved around though, the `things` branch stayed put.

```bash
$ git log
commit 1dfd056e7db978c24cdeaae8ee44b5b3aa2498f3 (HEAD -> master, things)
Author: Pieter Moris <13552343+pmoris@users.noreply.github.com>
Date:   Mon Oct 3 15:06:10 2022 +0200

    Add more favorite things

commit 6b5733bc18080a8f5c0c2a5601f35dbad1edae01
Author: Pieter Moris <13552343+pmoris@users.noreply.github.com>
Date:   Fri Sep 30 16:21:07 2022 +0200

    List beautiful things

commit 94af264c0ec9e4560d23c8bbacbb4370b9e20b65 (origin/master, borked/new)
Author: Pieter Moris <13552343+pmoris@users.noreply.github.com>
Date:   Wed Sep 7 15:48:49 2022 +0200

    Add cake declaration
```

Observe that the remote-tracking branches (`origin/master`) are still pointing to one of the older commits, because we have not pushed our changes to the remote yet.
````

`````

Did you notice that message about fast-fowarding after merging? To understand what this means and why it happens, we need to look more closely at the graph that makes up git's revision history.

Before merging, the history looks similar to this:

:::{figure-md} fig-branchbeforemerge-target
:align: center

![](../img/branch-before-merge.png)

Git history before merging `things` branch into `master`
:::

As you can see, the additional commit `f` on the `things` branch form a linear history on top of the commit history that `master` has. Thus, the merge between the two branches can be performed by simply moving the `master` branch ahead to the next commit `f`. Afterwards, both branches will be identical in terms of their commit history.

:::{figure-md} fig-branchafterffmerge-target
:align: center

![](../img/branch-after-ff-merge.png)

Git history after merging `things` branch into `master` via fast-foward.
:::

If we had used the `--no-ff` flag when merging, an additional _merge commit_ would have been created. This commit would have as its parents the latest commit of each of the two branches, as shown below.


:::{figure-md} fig-branchafternoffmerge-target
:align: center

![](../img/branch-after-noff-merge.png)

Git history after merging `things` branch into `master` without fast-fowarding. Commit `g` is called a _merge commit_ and it refers to commit `f` as well as commit `e`, which were the commits that the two branches were pointing to respectively.
:::

Try it out for yourself!

`````{tab-set}

````{tab-item} Exercise

```{admonition} Instructions
:class: hint
- Reset your repository to the way it was before the merge.
- Merge your feature branch into master without fast forwarding.
- Check the git log.
```

````

````{tab-item} Solution

First, check the commit that we need to reset to via `git log`. Or alternatively, because we know what happened in this case, just use `HEAD~2` to jump back exactly 2 commits.

```bash
git reset --hard 94af2
```

```{warning}
Because this bears repeating. Do **not** rewrite your git history like this if your changes have already been pushed to a remote repository, unless you are the only person working on the affected branches.
```

The `git log` shows what we expect now: `master` points to the old commit again and `things` has remained in place.

```bash
$ git log --all
commit 1dfd056e7db978c24cdeaae8ee44b5b3aa2498f3 (things)
Author: Pieter Moris <13552343+pmoris@users.noreply.github.com>
Date:   Mon Oct 3 15:06:10 2022 +0200

    Add more favorite things

commit 6b5733bc18080a8f5c0c2a5601f35dbad1edae01
Author: Pieter Moris <13552343+pmoris@users.noreply.github.com>
Date:   Fri Sep 30 16:21:07 2022 +0200

    List beautiful things

commit 94af264c0ec9e4560d23c8bbacbb4370b9e20b65 (HEAD -> master, origin/master, borked/new)
Author: Pieter Moris <13552343+pmoris@users.noreply.github.com>
Date:   Wed Sep 7 15:48:49 2022 +0200

    Add cake declaration
```

Now merge the branches without fast-fowarding.

```bash
git merge --no-ff things
```

```bash
Merge made by the 'recursive' strategy.
 my-favorite-things.txt | 3 +++
 1 file changed, 3 insertions(+)
```

Now the `git log` resembles the structure we saw in the figure above. A new merge commit has been introduced, and that is the commit the branch that we were on while merging now points to. The other branch remains untouched, just as before, and it still points to its most recent commit (`Add more favorite things`).

```bash
$ git log --graph
*   commit bda49b242576a9a8b913ab92d51e8a2794a60ddb (HEAD -> master)
|\  Merge: 94af264 1dfd056
| | Author: Pieter Moris <13552343+pmoris@users.noreply.github.com>
| | Date:   Wed Oct 5 14:42:57 2022 +0200
| |
| |     Merge branch 'things'
| |
| * commit 1dfd056e7db978c24cdeaae8ee44b5b3aa2498f3 (things)
| | Author: Pieter Moris <13552343+pmoris@users.noreply.github.com>
| | Date:   Mon Oct 3 15:06:10 2022 +0200
| |
| |     Add more favorite things
| |
| * commit 6b5733bc18080a8f5c0c2a5601f35dbad1edae01
|/  Author: Pieter Moris <13552343+pmoris@users.noreply.github.com>
|   Date:   Fri Sep 30 16:21:07 2022 +0200
|
|       List beautiful things
|
* commit 94af264c0ec9e4560d23c8bbacbb4370b9e20b65 (origin/master, borked/new)
| Author: Pieter Moris <13552343+pmoris@users.noreply.github.com>
| Date:   Wed Sep 7 15:48:49 2022 +0200
|
|     Add cake declaration
```

````

`````

```{image} ../img/merge.png
:alt: fig-merge-platypus
:class: bg-primary
:align: center
```

## A few short intermezzos

### Visualising the git graph

To save myself the trouble of creating all these images, let's introduce a few methods to browse the history directly in your terminal. If that doesn't end up floating your boat, just remember that you can always visualise your git history in a GUI, as well as view it on a remote git server (after pushing your changes at least, more on that later).

Perhaps you already caught me using the `--graph` option for `git log` in the solution to the previous exercise. In fact, there exist a whole plethora of options to tweak this output.

A simple yet effective example is the following:

```bash
git log --oneline --abbrev-commit --all --graph --decorate --color
```

However, many more options exist. Have a look at this SO thread with some suggestions: [https://stackoverflow.com/questions/1838873/visualizing-branch-topology-in-git](https://stackoverflow.com/questions/1838873/visualizing-branch-topology-in-git).

To save yourself the trouble of remembering all this esoteric syntax, I suggest that you create the following alias in your `~/.gitconfig` file. This is the same file where your username and email ended up while setting up git.

```bash
[alias]
        graph = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(reset) %C(bold green)(%ar)%C(reset) %C(bold cyan)(committed: %cD)%C(reset) %C(auto)%d%C(reset)%n''          %C(white)%s%C(reset)%n''          %C(dim white)- %an <%ae> %C(reset) %C(dim white)(committer: %cn <%ce>)%C(reset)'
```

Now you will be able to produce these very fancy history logs when you type `git graph` (the colours get lost when copied to this document unfortunately).

```bash
$ git graph
*   bda49b2 - Wed, 5 Oct 2022 14:42:57 +0200 (11 minutes ago) (committed: Wed, 5 Oct 2022 14:42:57 +0200)  (HEAD -> master)
|\            Merge branch 'things'
| |           - Pieter Moris <13552343+pmoris@users.noreply.github.com>  (committer: Pieter Moris <13552343+pmoris@users.noreply.github.com>)
| * 1dfd056 - Mon, 3 Oct 2022 15:06:10 +0200 (2 days ago) (committed: Mon, 3 Oct 2022 17:18:23 +0200)  (things)
| |           Add more favorite things
| |           - Pieter Moris <13552343+pmoris@users.noreply.github.com>  (committer: Pieter Moris <13552343+pmoris@users.noreply.github.com>)
| * 6b5733b - Fri, 30 Sep 2022 16:21:07 +0200 (5 days ago) (committed: Fri, 30 Sep 2022 16:21:07 +0200)
|/            List beautiful things
|             - Pieter Moris <13552343+pmoris@users.noreply.github.com>  (committer: Pieter Moris <13552343+pmoris@users.noreply.github.com>)
* 94af264 - Wed, 7 Sep 2022 15:48:49 +0200 (4 weeks ago) (committed: Wed, 7 Sep 2022 15:48:49 +0200)  (origin/master, borked/new)
|           Add cake declaration
|           - Pieter Moris <13552343+pmoris@users.noreply.github.com>  (committer: Pieter Moris <13552343+pmoris@users.noreply.github.com>)
* b2c2e84 - Wed, 17 Aug 2022 12:30:36 +0200 (7 weeks ago) (committed: Wed, 7 Sep 2022 15:47:42 +0200)
|           Create a vector
|           - Pieter Moris <13552343+pmoris@users.noreply.github.com>  (committer: Pieter Moris <13552343+pmoris@users.noreply.github.com>)
* 02b6530 - Wed, 17 Aug 2022 12:19:18 +0200 (7 weeks ago) (committed: Wed, 17 Aug 2022 12:19:18 +0200)
            Initial commit
            - Pieter Moris <13552343+pmoris@users.noreply.github.com>  (committer: Pieter Moris <13552343+pmoris@users.noreply.github.com>)
```

### Diff'ing in git

Similar to visualising the git history, showing the differences between files in different commits, branches, the staging area or your working directory, is something that git GUIs are very useful for. However, sometimes a CLI is all you have or even quicker for some of the more complex operations. So here is a quick overview of some of the options.

I suggest you play around with this as you go along the rest of the workshop, to get a feel for how these commands work.

```{note}
- `git diff [<filepath>]`: this will compare the contents of your working directory with the files that are staged in the index. Omitting the filepath will compare all files. Note that this will not show you untracked (new) files.
- `git diff --cached [<commit>] [<filepath>]`: shows the difference between the index and a commit (defaults to the most recent one, i.e. `HEAD`). Can also take a filepath.
- `git diff <branch> <branch> [<filepath>]`: shows the difference between two branches/commits. Can also take a filepath.
- `git diff <commit> <filepath>`: shows the differences in a file between a commit (`HEAD by default`) and the working directory. Can also take a filepath.
```

```{seealso}
- [https://www.atlassian.com/git/tutorials/saving-changes/git-diff](https://www.atlassian.com/git/tutorials/saving-changes/git-diff)
- [https://www.cloudbees.com/blog/git-diff-a-complete-comparison-tutorial-for-git](https://www.cloudbees.com/blog/git-diff-a-complete-comparison-tutorial-for-git)
```

---

Now let's continue exploring branches!

## Merging altered branches

In this section we will explore what happens when we try to merge branches that have both been altered.

`````{tab-set}

````{tab-item} Exercise

```{admonition} Instructions
:class: hint
- Create a new feature branch and switch to it.
- Create or modify files and commit them.
- Switch back to the `master` branch, create yet another file and commit it. Make sure the file differs from any files you created in the feature branch.
- Compare the differences between the branches.
- Merge your feature branch into `master`.
- Check the git log (graph).
```

````

````{tab-item} Solution

First, create a new branch and create a new commit:

```bash
git switch -c jokes

cat >> jokes.txt <<EOL
“A moth goes into a podiatrist’s office, and the podiatrist’s office says,
“What seems to be the problem, moth?”
The moth says “What’s the problem? Where do I begin, man?
I go to work for Gregory Illinivich, and all day long I work. Honestly doc, I don’t
even know what I’m doing anymore. I don’t even know if Gregory Illinivich knows.
He only knows that he has power over me, and that seems to bring him happiness.
But I don’t know, I wake up in a malaise, and I walk here and there… at night I…I
sometimes wake up and I turn to some old lady in my bed that’s on my arm. A lady that
I once loved, doc. I don’t know where to turn to. My youngest, Alexendria, she fell in
the…in the cold of last year. The cold took her down, as it did many of us.
And my other boy, and this is the hardest pill to swallow, doc. My other boy,
Gregarro Ivinalititavitch… I no longer love him. As much as it pains me to say,
when I look in his eyes, all I see is the same cowardice that I… that I catch
when I take a glimpse of my own face in the mirror. If only I wasn’t such a coward,
then perhaps…perhaps I could bring myself to reach over to that cocked and loaded gun
that lays on the bedside behind me and end this hellish facade once and for all…
Doc, sometimes I feel like a spider, even though I’m a moth, just barely hanging on
to my web with an everlasting fire underneath me. I’m not feeling good.
And so the doctor says, “Moth, man, you’re troubled. But you should be seeing a psychiatrist.
Why on earth did you come here?”

And the moth says, “‘Cause the light was on.”

― Norm Macdonald
EOL

git add -A
git commit -m "Add joke"
```

Inspect the history:

```bash
$ git log --graph --pretty --oneline
* e1d2ed8 (HEAD -> jokes) Add joke
*   bda49b2 Merge branch 'things'
|\
| * 1dfd056 (things) Add more favorite things
| * 6b5733b List beautiful things
|/
* 94af264 (origin/master, borked/new) Add cake declaration
* b2c2e84 Create a vector
* 02b6530 Initial commit
```

Then do the same thing on the `master` branch:

```bash
git switch master

cat >> quotes.txt <<EOL
"She was not someone to use extreme language, but it was possible to be sure that when she deployed a mild term like ‘a bee in her bonnet’ she was using it to define someone whom she believed to be several miles over the madness horizon and accelerating."

― Terry Pratchett, Witches Abroad
EOL

git add -A
git commit -m "Add quote"
```

Again, inspect the history:

```bash
$ git log --graph --pretty --oneline
* a75b2e0 (HEAD -> master) Add quote
*   bda49b2 Merge branch 'things'
|\
| * 1dfd056 (things) Add more favorite things
| * 6b5733b List beautiful things
|/
* 94af264 (origin/master, borked/new) Add cake declaration
* b2c2e84 Create a vector
* 02b6530 Initial commit
```

Lastly, try to merge the two and inspect the log.

```bash
$ git merge jokes
Merge made by the 'recursive' strategy.
 jokes.txt | 7 +++++++
 1 file changed, 7 insertions(+)
 create mode 100644 jokes.txt

$ git graph
*   b913749 - Wed, 5 Oct 2022 15:43:48 +0200 (39 seconds ago) (committed: Wed, 5 Oct 2022 15:43:48 +0200)  (HEAD -> master)
|\            Merge branch 'jokes'
| |           - Pieter Moris <13552343+pmoris@users.noreply.github.com>  (committer: Pieter Moris <13552343+pmoris@users.noreply.github.com>)
| * e1d2ed8 - Wed, 5 Oct 2022 15:41:06 +0200 (3 minutes ago) (committed: Wed, 5 Oct 2022 15:41:06 +0200)  (jokes)
| |           Add joke
| |           - Pieter Moris <13552343+pmoris@users.noreply.github.com>  (committer: Pieter Moris <13552343+pmoris@users.noreply.github.com>)
* | a75b2e0 - Wed, 5 Oct 2022 15:41:21 +0200 (3 minutes ago) (committed: Wed, 5 Oct 2022 15:41:21 +0200)
|/            Add quote
|             - Pieter Moris <13552343+pmoris@users.noreply.github.com>  (committer: Pieter Moris <13552343+pmoris@users.noreply.github.com>)
*   bda49b2 - Wed, 5 Oct 2022 14:42:57 +0200 (62 minutes ago) (committed: Wed, 5 Oct 2022 14:42:57 +0200)
|\            Merge branch 'things'
| |           - Pieter Moris <13552343+pmoris@users.noreply.github.com>  (committer: Pieter Moris <13552343+pmoris@users.noreply.github.com>)
| * 1dfd056 - Mon, 3 Oct 2022 15:06:10 +0200 (2 days ago) (committed: Mon, 3 Oct 2022 17:18:23 +0200)  (things)
| |           Add more favorite things
| |           - Pieter Moris <13552343+pmoris@users.noreply.github.com>  (committer: Pieter Moris <13552343+pmoris@users.noreply.github.com>)
| * 6b5733b - Fri, 30 Sep 2022 16:21:07 +0200 (5 days ago) (committed: Fri, 30 Sep 2022 16:21:07 +0200)
|/            List beautiful things
|             - Pieter Moris <13552343+pmoris@users.noreply.github.com>  (committer: Pieter Moris <13552343+pmoris@users.noreply.github.com>)
* 94af264 - Wed, 7 Sep 2022 15:48:49 +0200 (4 weeks ago) (committed: Wed, 7 Sep 2022 15:48:49 +0200)  (origin/master, borked/new)
|           Add cake declaration
|           - Pieter Moris <13552343+pmoris@users.noreply.github.com>  (committer: Pieter Moris <13552343+pmoris@users.noreply.github.com>)
* b2c2e84 - Wed, 17 Aug 2022 12:30:36 +0200 (7 weeks ago) (committed: Wed, 7 Sep 2022 15:47:42 +0200)
|           Create a vector
|           - Pieter Moris <13552343+pmoris@users.noreply.github.com>  (committer: Pieter Moris <13552343+pmoris@users.noreply.github.com>)
* 02b6530 - Wed, 17 Aug 2022 12:19:18 +0200 (7 weeks ago) (committed: Wed, 17 Aug 2022 12:19:18 +0200)
            Initial commit
            - Pieter Moris <13552343+pmoris@users.noreply.github.com>  (committer: Pieter Moris <13552343+pmoris@users.noreply.github.com>)
```

We can see that a merge commit was created which has as its parents the commits created on the different branches.
````

`````

So far, so good, right? Nothing unexpected happened. Onwards!

## Merging conflicting branches

What happens when the same files have been altered in the branches that we try to merge? Well, this is where so called _conflicts_ might arise. If the file already existed in both branches, and, for example, lines were added at the top and bottom in each branch respectively, there would be no issue and git will happily merge the branches and files. However, sometimes the changes contradict each other. Or there will be so many changes that git cannot work out what should be done. In those situations, we will have to solve the conflicts ourselves.

```{seealso}
- [https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging#_basic_merge_conflicts](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging#_basic_merge_conflicts)
- [https://git-scm.com/book/en/v2/Git-Tools-Advanced-Merging](https://git-scm.com/book/en/v2/Git-Tools-Advanced-Merging)
- [https://www.atlassian.com/git/tutorials/using-branches/merge-conflicts](https://www.atlassian.com/git/tutorials/using-branches/merge-conflicts)
```



`````{tab-set}

````{tab-item} Exercise

```{admonition} Instructions
:class: hint
- Create a new file and commit it to the `master` branch.
- Create a new branch, switch to it, modify the file and commit it.
- Switch back to `master`, modify the file in a different way and commit it.
- Attempt to merge the branches and resolve the conflicts.
```

````

````{tab-item} Solution

Creating a new file:

```bash
echo '"...!" cried Gandalf' > khazad-dum.bridge
git add -A
git commit -m "Recall the tragedy in Moria"
```

Creating a new branch and modifying the same file:

```bash
git switch -c flee
sed -i -E 's/[.]{3}/Flee you fools/g' khazad-dum.bridge
git add -A
git commit -m "Flee!"
```

Switch back to `master` and modify the same file:

```bash
git switch master
sed -i -E 's/[.]{3}/Fly you fools/g' khazad-dum.bridge
git add -A
git commit -m "Fly!"
```

Attempt to merge:

```bash
git merge flee
```

```bash
Auto-merging khazad-dum.bridge
CONFLICT (content): Merge conflict in khazad-dum.bridge
Automatic merge failed; fix conflicts and then commit the result.
```

Git warns us that there are conflicts in our files. Open them up in a text editor and see what it has done to help you resolve them! If you were to open this file in a fancy text editor like VSCode, it would even highlight these sections for you.

```bash
$ cat khazad-dum.bridge
<<<<<<< HEAD
"Fly you fools!" cried Gandalf
=======
"Flee you fools!" cried Gandalf
>>>>>>> flee
```

At this point, have a look at what `git status` has to say:

```bash
$ git status
On branch master
Your branch is ahead of 'origin/master' by 8 commits.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   khazad-dum.bridge

no changes added to commit (use "git add" and/or "git commit -a")
```

The `git merge --abort` command it mentions is a good one to remember. If you want to back out of the whole thing, perhaps because you attempted to merge the wrong branches, then you can still do so without any issues.

In order to fix the actual conflict itself, simply remove the section that is not required in the file. **Make sure that you remove the `<<<<<<<`, `=======` and `>>>>>>>` sections as well. Because otherwise git will happily commit them for you as part of your "intended" changes.

```bash
  GNU nano 4.8                                                                                    khazad-dum.bridge                                                                                    Modified
"Fly you fools!" cried Gandalf
```

Then, like `git status` instructed us, add the altered file and commit again to finalise the merge:

```bash
git add -A
git commit
```

```bash
$ git commit
[master 86447c5] Merge branch 'flee'
```

This is what the commit editor would show you:

```
Merge branch 'flee'

# Conflicts:
#       khazad-dum.bridge
#
# It looks like you may be committing a merge.
# If this is not correct, please remove the file
#       .git/MERGE_HEAD
# and try again.


# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
# Your branch is ahead of 'origin/master' by 8 commits.
#   (use "git push" to publish your local commits)
#
# All conflicts fixed but you are still merging.
#
```


````

`````

```{image} ../img/conflicts.jpg
:alt: fig-conflicts-everywhere
:class: bg-primary
:align: center
```
