# Show activity

`git activity show`: Show Git activity.

```shell
git activity show [<date> | ([--date=<date>] [--log-message-pattern=<pattern>] [--repo-pattern=<pattern>] [--branch-pattern=<pattern>] [--commit-message-pattern=<pattern>] [--sha-pattern=<pattern>])]
```

Use a terminal pager to display the log file's data, optionally filtered.

## Show it **all**

```shell
git activity show
```

## Show filtered results

- Filter by **day**:

    By **YYYY-MM-DD**:

    ```shell
    git activity show 2024-01-01
    # or
    git activity show --date 2024-01-01
    ```

    or by **name**:

    ```shell
    git activity show today
    # or
    git activity show --date today

    git activity show yesterday
    # or
    git activity show --date yesterday

    git activity show "last monday"
    # or
    git activity show --date "last monday"
    ```

- Filter by **any combination** of date, log message regex pattern, repo regex pattern, branch regex pattern, commit message regex pattern, and commit SHA (first seven characters) regex pattern.[^1]

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

    # show activity recorded in all `org-1` repos
    % git activity show --repo-pattern=org-1

    # show activity recorded in `org-1--front`, `org-2--front`, and `afronted`
    % git activity show --repo-pattern=front

    # activity recorded in `org-1--front` and `org-2--front`
    % git activity show --repo-pattern='--front'
    ```

[^1]: Specifically, `awk` regex patterns.
