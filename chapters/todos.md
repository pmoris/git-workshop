- move reset usage for files to different section
- move checkout usage to different section
- clarify how their use on files (resetting specific files via checkout, reset files for unstaging, checkout HEADless) are now handled by more simply commands that do 1 thing only.
-


# requirements / what to prepare

Before starting this workshop, ensure that you have a fully functional version of git available on your workstation. Ideally this is git installed inside an (Ubuntu) WSL, alongside a proper shell like Windows Terminal, but "Git for Windows" and the "Git Bash" cmd app can also work. In addition, a fully featured git GUI like Fork, Git Kraken or a similar application, or the git plugin of your favorite IDE like RStudio or VSCode (I highly recommend the GitLens extension) can be useful if you prefer to not do all of your work on the command line.

Bitbucket or GitLab account.

# working with remotes

adding remote (put on gitlab/bitbucket)
pushing branches
pushing tags

# Collab basics

- feature branch + merge into master
- feature branch + merge into master + no ff
- feature branch + updates to master + merge into master
- feature branch + conflicting updates to master (fly you fools / bluewhite dress) + merge into master
- feature branch + updates to feature branch by others + merge into master
+ when pushed to remote

# advanced

- rebase vs merge: https://www.atlassian.com/git/tutorials/merging-vs-rebasing
- trunk-based vs git flow

---


### feature branch + updates to master + merge into master

pull in updates from master into feature?

or just showing merge commit when both have different history (to different files) and then merge together?

or both?

### feature branch + conflicting updates to master (fly you fools / bluewhite dress) + merge into master

changes to same files + resolve conflicts


### feature branch + updates to feature branch by others + merge into master

pulling in updates from others (edit on remote) before pushing
similar to pull in updates from master into feature, so skip that part in the previous^^ section

pull request as example?



- revising basics
updating / linknig to online repo
- alert box
- use status often
- reverting some things
- diff cached
- git graph
- add quotes e.g. time is relative
- unit tests => in function of merge requests
- resolving conflicts
- pulling another person's branch, creating a new one
- when to pull from master before merge request


`````{tab-set}

````{tab-item} Exercise #

```{admonition} Instructions
:class: hint
Instructions
```

````

````{tab-item} Solution #

```bash
code block
```

````

`````





----


`````{tab-set}

````{tab-item} Exercise

```{admonition} Instructions
:class: hint
- step 1
- step 2
```

````

````{tab-item} Solution

````

`````
