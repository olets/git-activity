# git-activity ![GitHub release (latest by date)](https://img.shields.io/github/v/release/olets/git-activity) ![GitHub commits since latest release](https://img.shields.io/github/commits-since/olets/git-activity/latest)

**git-activity**: Record and show your Git activity across multiple (or all) repos, optionally filtered by date, activity type (e.g. commit, branch creation, etc) regex pattern, repo name regex pattern, branch name regex pattern, commit message regex pattern, and/or commit SHA (first seven characters) regex pattern.[^1]

It's sort of like a global `git log` where you control which repos are watched and what types of activity are recorded. 

Useful for

- Retroactively filling out a time sheet, or correcting a time sheet when you realize you left one timer running all day

    ```shell
    git activity show today
    ```
    
    "Okay I did Project A from 8:16 to 10:20, Project B from 10:20 to 10:53, I know I was in a meeting 11-12, was back on Project A by 12:08, project C 1:18-1:40, project B 2:10-5:04 but oh look I would have forgotten: project A 3:02-3:06."

- Answering a weekly department "What projects did you work on last week and what did you do on them?" prompt

    ```shell
    git activity "last week monday" && git activity "last week tuesday" && git activity "last week wednesday" && git activity "last week thursday" && git activity "last week friday"
    ```

    (You could make an alias for that. Or an abbreviation! fish has native abbreviations. zsh has [zsh-abbr](https://zsh-abbr.olets.dev/), by the creator of `git-activity`.)

- Finding that time you solved a problem similar to the one you're working on now

    ```shell
    git activity --commit-message-pattern='(card.*pay)|(pay.*card)'
    ```

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

## With a shell plugin manager

### Zsh

You can install git-activity with a shell plugin manager, including those built into frameworks such as Oh-My-Zsh (OMZ) and prezto. Each has their own way of doing things. Read your package manager's documentation or the [zsh plugin manager plugin installation procedures gist](https://gist.github.com/olets/06009589d7887617e061481e22cf5a4a); Fig users can install git-activity from [its page in the Fig plugin directory](https://fig.io/plugins/other/zsh-abbr_olets)

Want to stay on this major version until you _choose_ to upgrade to the next? Use your package manager's convention for specifying the branch `v1`.

After adding the plugin to the manager, it will be available in all new terminals. To use it in an already-open terminal, restart your shell in that terminal:

```shell
exec zsh
```

### Other shells

Users of shells **other than zsh** may be able to install git-random as a plugin. Check your plugin manager's documentation for support for installing commands.

### Manual

1. Either download the archive of the release of your choice from <https://github.com/olets/git-activity/releases> and expand it (ensures you have the latest official release)
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
> Does not check to see if you've already installed this. If you notice duplicative entries in your git-activity log, check your global `init.templatedir` hooks, and the `.git` hooks in any repos you've run `git init` in

To configure Git to record all `git commit`s, `git commit --amend`s, `git rebase`s, `git push`es, and most `git checkout -b`/`git switch -c`, run

```shell
git activity install
```

That will

1. Configure Git's global `init.templatedir`, if not already configured. Files in this directory are automatically silently copied to every repo you run `git init` in.
    - If `XDG_CONFIG_HOME` is defined and the global Git `init.templatedir` is not defined, Git's global `init.templatedir` will be configured as `$XDG_CONFIG_HOME/.config/git-templates`
    - Otherwise, if `XDG_CONFIG_HOME` is not defined and the global Git `init.templatedir` is not defined, Git's global `init.templatedir` will be configured as `$HOME/.config/git-templates`
1. Add a `hooks` directory to Git's global `init.templatedir` if it does not exist already.
1. Create or add to the `hooks` directory's `post-checkout`, `post-commit`, `post-rewrite`, and `pre-push` files.

### Manual

> [!TIP]
> If you want to include the hooks in version control, consider a Git hook manager. For example, [Husky](https://typicode.github.io/husky/), [pre-commit](https://pre-commit.com/), or [simple-git-hooks](https://github.com/toplenboren/simple-git-hooks).

In a Git repo's `.git` directory, or in your global Git `init.templatedir`

1. Create a `hooks` directory, if it doesn't already exist.

1. In the `hooks` directory, create a file named after a Git hook (read the [githooks documentation](https://git-scm.com/docs/githooks) for details).

1. Make the file executable: run `chmod +x path/to/the/file/you/created`.

1. Add a `git-activity` command to the file.

    A good starting point is to run
    
    ```shell
    git activity record-post-checkout "$@"
    ```
    
    for `post-checkout` hooks, and/or
    
    ```shell
    git activity record <replace me>
    ```

    for `post-commit`, `post-rewrite`, and/or `pre-push` hooks, where `<replace me>` is replaced with `commit`, `rewrite`, and `push`, respectively.

    But you can be as complex as you want. Git's hooks and your shell scripting skill are the limit.

## Usage

1. `git activity record`: Record some Git activity.

    ```shell
    git activity record <log message>
    ```

    Adds a new row to `git-activity`'s CSV log file with: date (`YYYY-MM-DD HH:MM:SS UTC-offset`), log message (typically an identifier of the type of activity, e.g. "commit", "rewrite") regex pattern, repo name regex pattern, the checked out branch's name regex pattern, the checked out commit's message regex pattern, and the checked out commit's SHA regex pattern.[^1]

    - If you have [automatic recording set up](#set-up-automatic-recording), go about your daily work, creating branches, committing, rebasing, etc., and entries will be added to the log file for you.

    - If you don't have automatic recording set up, run manually record activity:

        ```shell
        git activity record "starting issue #1"
        # hack hack
        git activity record "finished issue #1"
        ```

        (Doesn't that make you want to set up automatic recording?)

1. `git activity show`: Review Git activity.

    ```shell
    git activity show [<date> | ([--date=<date>] [--log-message-pattern=<log message pattern>] [--repo-pattern=<repo pattern>] [--branch-pattern=<branch pattern>] [--commit-message-pattern=<commit message pattern>] [--sha-pattern=<7-character commit SHA pattern>])]
    ```

    Use a terminal pager to display the log file's data, optionally filtered.

    - Show it **all**:

        ```shell
        git activity show
        ```

    - Filter by **day**:

        By **YYYY-MM-DD**:

        ```shell
        git activity show 2024-01-01 # or `git activity show --date 2024-01-01`
        ```

        or by **name**:

        ```shell
        git activity show today # or `git activity show --date today`
        git activity show yesterday # or `git activity show --date yesterday`
        git activity show "last monday" # or `git activity show --date "last monday"`
        ```

    - Filter by **any combination** of date, log message regex pattern, repo regex pattern, branch regex pattern, commit message regex pattern, and 7-character commit SHA regex pattern.[^1]

        ```shell
        # what progress did I make on assignments yesterday?
        git activity show yesterday --commit-message-pattern='feat\\('

        # what did I do on this project last week?
        git activity show "last week monday" --repo-pattern=my-cool-project
        ```

        > [!NOTE]
        > All values except for `<date>` are treated as regular expressions

        ```shell
        % ls ~/Projects
        afronted
        org-1--front
        org-1--back
        org-2--front
        org-2--back
        # activity recorded in all `org-1` repos
        % git activity show --repo-pattern=org-1
        # activity recorded in `org-1--front`, `org-2--front`, and `afronted`
        % git activity show --repo-pattern=front
        # activity recorded in `org-1--front` and `org-2--front`
        % git activity show --repo-pattern='--front'
        ```

## Options

Variable | Added in | Type | Default | Use
---|---|---|---|---
`GIT_ACTIVITY_CSV_PATH` | `1.0.0` | String | If `XDG_CONFIG_HOME` is set, `$XDG_CONFIG_HOME/git-activity/git-activity.csv`<br>Otherwise, `$HOME/.config/git-activity/git-activity.csv` | Path to the log file

To customize a configuration variable, `export` it from your shell configuration file. For example

```shell
# .zshrc
export GIT_ACTIVITY_CSV_PATH="$HOME/git-activity.csv"
```

## Changelog

See the [CHANGELOG](CHANGELOG.md) file.

## Contributing

Thanks for your interest. Contributions are welcome!

> Please note that this project is released with a [Contributor Code of Conduct](CODE_OF_CONDUCT.md). By participating in this project you agree to abide by its terms.

Check the [Issues](https://github.com/olets/git-activity/issues) to see if your topic has been discussed before or if it is being worked on.

Please read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request.

## License

- <a href="https://www.github.com/olets/git-activity">git-activity</a> by <a href="https://olets.dev">Henry Bley-Vroman</a> is, with the exception of its logo as covered below, licensed under a license which is the unmodified text of <a href="https://creativecommons.org/licenses/by-nc-sa/4.0">CC BY-NC-SA 4.0</a> and the unmodified text of a <a href="https://firstdonoharm.dev/build?modules=eco,extr,media,mil,sv,usta">Hippocratic License 3</a>. It is not affiliated with Creative Commons or the Organization for Ethical Source.

    Human-readable summary of (and not a substitute for) the [LICENSE](LICENSE) file:

    You are free to

    - Share — copy and redistribute the material in any medium or format
    - Adapt — remix, transform, and build upon the material

    Under the following terms

    - Attribution — You must give appropriate credit, provide a link to the license, and indicate if changes were made. You may do so in any reasonable manner, but not in any way that suggests the licensor endorses you or your use.
    - Non-commercial — You may not use the material for commercial purposes.
    - Ethics - You must abide by the ethical standards specified in the Hippocratic License 3 with Ecocide, Extractive Industries, US Tariff Act, Mass Surveillance, Military Activities, and Media modules.
    - Preserve terms — If you remix, transform, or build upon the material, you must distribute your contributions under the same license as the original.
    - No additional restrictions — You may not apply legal terms or technological measures that legally restrict others from doing anything the license permits.

## Related

I've created other command line tools, including other Git tools. Check them out at <https://olets.dev/software/>.

&nbsp;

[^1]: Specifically, `awk` regex patterns.
