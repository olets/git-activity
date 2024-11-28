# Installation & Setup

## Installation

### With Homebrew

> [!TIP]
> Recommended

git-activity is available on Homebrew. Run

```shell
brew install olets/tap/git-activity
```

and follow the post-install instructions logged to the terminal.

`brew upgrade` will upgrade you to the latest version, even if it's a major version change.

Want to stay on this major version until you _choose_ to upgrade to the next? When installing git-activity with Homebrew for the first time, run

```shell
brew install olets/tap/git-activity@1
```

If you've already installed `olets/tap/git-activity` with Homebrew, you can switch to the v6 formula by running

```shell
brew uninstall --force git-activity && brew install olets/tap/git-activity@6
```

### With a shell plugin manager

#### Zsh

You can install git-activity with a shell plugin manager, including those built into frameworks such as Oh-My-Zsh (OMZ) and prezto. Each has their own way of doing things. Read your package manager's documentation or the [zsh plugin manager plugin installation procedures gist](https://gist.github.com/olets/06009589d7887617e061481e22cf5a4a).

Want to stay on this major version until you _choose_ to upgrade to the next? Use your package manager's convention for specifying the branch `v1`.

After adding the plugin to the manager, it will be available in all new terminals. To use it in an already-open terminal, restart your shell in that terminal:

```shell
exec zsh
```

#### Other shells

Users of shells **other than zsh** may be able to install git-activity as a plugin. Check your plugin manager's documentation for support for installing commands.

### Manual

1. Either
    - download the archive of the release of your choice from <https://github.com/olets/git-activity/releases> and expand it (ensures you have the latest official release), 

    - or clone a single branch:

        ```shell
        git clone https://github.com/olets/git-activity --recurse-submodules --single-branch --branch <branch> --depth 1
        ```

        Replace `<branch>` with a branch name. Good options are `main` (for the latest stable release), `next` (for the latest release, even if it isn't stable), or `v6` (for releases in this major version).

1. Put the file `git-activity` in a directory in your `PATH`.

## Set up automatic recording

> [!INFO]
> This is optional, but recommended.

### Automated

> [!TIP]
> **Recommended**

> [!WARNING]  
> Does not check to see if you've already run the install. If you notice duplicative entries in your git-activity log, check your global `init.templatedir` hooks, and the `.git` hooks in any repos you've run `git init` in.

To configure Git to record all `git commit`s, `git commit --amend`s, `git merge`s, `git push`es, `git rebase`s, and most `git checkout -b`/`git switch -c`, run

```shell
git activity install [--global]
```

If run from a Git repo root, that will add Git hooks. If a hook file already exists, the command is appended to it.

If `--global` is passed, hooks are added to Git's `init.templatedir`; from now on, running `git init` for the first time in any directory will add the hooks to the new repo.

(If `init.templatedir` is not already configured, it will be set to `$XDG_CONFIG_HOME/.config/git-templates` if `XDG_CONFIG_HOME` is defined, or to `$HOME/.config/git-templates` if `XDG_CONFIG_HOME` is not defined. If a hook file already exists, the command is appended to it.)

`git activity install` sets up these hooks to run these commands:

Hook | Command
---|---
post-checkout | `git activity record-post-checkout "$@"`
post-commit | `git activity record commit`
post-merge | `git activity record merge`
post-rewrite | `git activity record rewrite`
pre-push | `git activity record push`

### Manual

> [!TIP]
> If you want to include the hooks in version control, consider a Git hook manager. For example, [Husky](https://typicode.github.io/husky/), [pre-commit](https://pre-commit.com/), or [simple-git-hooks](https://github.com/toplenboren/simple-git-hooks).

In a Git repo's `.git` directory, or in your global Git `init.templatedir`

1. Create a `hooks` directory, if it doesn't already exist.

1. In the `hooks` directory, create a file named after a Git hook (read the [githooks documentation](https://git-scm.com/docs/githooks) for details).

1. Make the file executable: run `chmod +x path/to/the/file/you/created`.

1. Add a `git-activity` command to the file.

Good starting points are [the hooks defined by `git activity install`](#automated). But you can be as complex as you want. Git's hooks and your shell scripting skill are the limit.
