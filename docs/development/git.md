# SPM Development with Git and GitHub

##  Git Installation

To use Git on Windows, please install the GitHub Desktop, and optionally the Git command line tool and TortoiseGit.

* [GitHub Desktop](https://desktop.github.com/)
* [Git](https://git-scm.com/downloads)
* [TortoiseGit](https://tortoisegit.org/)

It is also recommended to install the Visual Studio Code text editor and WinMerge:

* [Visual Studio Code](https://code.visualstudio.com/)
* [WinMerge](https://winmerge.org/)

### Visual Studio Code Extensions

Visual Studio Code has a native support of Git:

* [Git Support in Visual Studio Code](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)

but some extra extensions are worth considering for MATLAB development:

* [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker)
* [MATLAB](https://marketplace.visualstudio.com/items?itemName=Gimly81.matlab)
* [Remote - SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh)

## Git Configuration

### Username and commit email address

* [Setting your username](https://docs.github.com/en/get-started/getting-started-with-git/setting-your-username-in-git)
* [Setting your commit email address](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-email-preferences/setting-your-commit-email-address)

```
git config --global user.name "Farl Kriston"
git config --global user.email "f.kriston@ucl.ac.uk"
```

If you use another personal email for most of your repositories, make sure to set the SPM repository to specifically use your institutional email address:

```
git config --local user.email "f.kriston@ucl.ac.uk"
```

### Merge Strategy

The default of [`git pull`](https://git-scm.com/docs/git-pull) to reconcile divergent branches is to call `git merge`. Here this default is changed to call `git rebase` instead. This has the advantage of creating a linear history, like in a traditional SVN workflow.

```
git config --global pull.rebase true
```

This is equivalent to using `git pull --rebase`.

## Git Commits

### Log Messages

## Git Workflow

We are using a [Centralised Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows#centralized-workflow). This is also called [Trunk Based Development](https://trunkbaseddevelopment.com/) (see this [article](https://medium.com/@mattia.battiston/why-i-love-trunk-based-development-641fcf0b94a0) for a discussion). Popular other workflows include [GitHub Flow](https://githubflow.github.io/) and [Gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow).

## Pull Requests

Recommendation for pull request reviewers: When merging, we generally like `squash+merge`. Unless it is the rare case of a PR with carefully staged individual commits that you want in the history separately, in which case `merge` is acceptable, but usually prefer `squash+merge`.


## References

* [Dangit, Git!?!](https://dangitgit.com/)
* [On Git and Cognitive Load](https://dzone.com/articles/on-git-and-cognitive-load)
