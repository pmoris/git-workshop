# What is next?

If you would like to learn more about git, here are a few resources and topics to get you started.

## General topics

- Learn to love `git stash`: [https://www.atlassian.com/git/tutorials/saving-changes/git-stash](https://www.atlassian.com/git/tutorials/saving-changes/git-stash) & [https://git-scm.com/book/en/v2/Git-Tools-Stashing-and-Cleaning](https://git-scm.com/book/en/v2/Git-Tools-Stashing-and-Cleaning).
- Using meaningful commit messages: [https://cbea.ms/git-commit/](https://cbea.ms/git-commit/)
- Interactive staging: a very useful alternative to stage files (or parts of files) separately, when you don't have access to a GUI that can do this. [https://git-scm.com/book/en/v2/Git-Tools-Interactive-Staging](https://git-scm.com/book/en/v2/Git-Tools-Interactive-Staging)
- Interactive rebasing: [https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History) (**WARNING, AGAIN!** Only use this for cleaning up local or private branches, because rewriting history of commits that were already pushed to a remote and used by others will lead to disaster.)
- The git config: [https://www.atlassian.com/git/tutorials/setting-up-a-repository/git-config](https://www.atlassian.com/git/tutorials/setting-up-a-repository/git-config)
- `git reflog`: a get-out-of-jail card (but don't rely on it too much).
- More on why you (mostly) shouldn't use `git push --force`: [https://www.datree.io/resources/git-push-force](https://www.datree.io/resources/git-push-force)
- git branching naming conventions: e.g. `
- A tutorial I found as I was wrapping up my own, to be used in case I missed something important ;) : [https://coderefinery.github.io/git-collaborative/centralized/#push-your-change-as-a-new-branch](https://coderefinery.github.io/git-collaborative/centralized/#push-your-change-as-a-new-branch)
- Explore git tags. Hint: they are just pointers like branches.
- Read about `cherrypick`.
- [https://ohshitgit.com/](https://ohshitgit.com/)

### Branch naming conventions

Branches can be given a particular name depending on their purpose, e.g. `fix-plotting-functions` or `feature-gsea`. These names can also be automatically created when opening a branch to fix a specific issue that was opened on the git server for example.

```{seealso}
- [https://idiv-biodiversity.github.io/git-knowledge-base/branch-naming-conventions.html](https://idiv-biodiversity.github.io/git-knowledge-base/branch-naming-conventions.html)
- [https://confluence.atlassian.com/bitbucketserver/branches-776639968.html](https://confluence.atlassian.com/bitbucketserver/branches-776639968.html)
```

### Git's revision syntax

Git uses a special syntax to refer to commits (and also trees) in many different ways.

For example, instead of specifying the SHA-1 hash of the previous commit, it can also be referred to as `HEAD^^`. `HEAD~3` refers to the third latest commit, while `master~3` does the same thing from the branch's point of view.

```{seealso}
- [https://devhints.io/git-revisions](https://devhints.io/git-revisions)
- [https://stackoverflow.com/questions/4044368/what-does-tree-ish-mean-in-git](https://stackoverflow.com/questions/4044368/what-does-tree-ish-mean-in-git)
```
