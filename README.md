# git-activity ![GitHub release (latest by date)](https://img.shields.io/github/v/release/olets/git-activity) ![GitHub commits since latest release](https://img.shields.io/github/commits-since/olets/git-activity/latest)

**git-activity**: Record and show your Git activity across multiple (or all) repos, optionally filtered by date, activity type (e.g. commit, branch creation, etc), repo name, branch name, commit message, and/or commit SHA.

It sort of like a global `git log` where you control which repos are watched and what types of activity are recorded. 

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
    git activity --commit-message '(card.*pay)|(pay.*card)'
    ```

## Set up automatic recording

> [!INFO]
> This is optional, but recommended.

### Automated

> [!TIP]
> **Recommended**

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

    Adds a new row to `git-activity`'s CSV log file with: date (`YYYY-MM-DD HH:MM:SS UTC-offset`), log message (typically an identifier of the type of activity, e.g. "commit", "rewrite"), repo name, the checked out branch's name, the checked out commit's message, and the checked out commit's SHA.

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
    git activity show [<date> | ([--date <date>] [--log-message <log message>] [--repo <repo>] [--branch <branch>] [--commit-message <commit message>] [--sha <commit SHA>])]
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

    - Filter by **any combination** of date, log message substring, repo substring, branch substring, commit message substring, and commit SHA substring.

        ```shell
        # what progress did I make on assignments yesterday?
        git activity show yesterday --commit-message 'feat\\('

        # what did I do on this project last week?
        git activity show "last week monday" --repo my-cool-project
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
        % git activity show --repo org-1
        # activity recorded in `org-1--front`, `org-2--front`, and `afronted`
        % git activity show --repo front
        # activity recorded in `org-1--front` and `org-2--front`
        % git activity show --repo '--front'
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

- <a href="https://www.github.com/olets/git-activity">git-activity</a> by <a href="https://www.github.com/olets">Henry Bley-Vroman</a> is, with the exception of its logo as covered below, licensed under a license which is the unmodified text of <a href="https://creativecommons.org/licenses/by-nc-sa/4.0">CC BY-NC-SA 4.0</a> and the unmodified text of a <a href="https://firstdonoharm.dev/build?modules=eco,extr,media,mil,sv,usta">Hippocratic License 3</a>. It is not affiliated with Creative Commons or the Organization for Ethical Source.

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

- The [git-activity logo](https://github.com/olets/git-activity/tree/main/docs/public/images/git-activity-logo.png) is licensed under the [Creative Commons Attribution 3.0 Unported License](https://creativecommons.org/licenses/by/3.0/). It is a modification of the [Git logo](https://git-scm.com/downloads/logos) created by [Jason Long](https://twitter.com/jasonlong) and is licensed under the same license. The license is available in the [LICENSE_LOGO](LICENSE_LOGO) file.

## Related

I've created other command line tools, including other Git tools. Check them out at <https://olets.dev/software/>.
