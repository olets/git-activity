# Record activity

`git activity record`: Record some Git activity.

```shell
git activity record <log message>
```

Adds a new row to `git-activity`'s CSV log file with: date (`YYYY-MM-DD HH:MM:SS UTC-offset`), log message (typically an identifier of the type of activity, e.g. "commit", "rewrite") regex pattern, repo name regex pattern, the checked out branch's name regex pattern, the checked out commit's message regex pattern, and the checked out commit's SHA regex pattern.[^1]

- If you have automatic recording set up, go about your daily work, creating branches, committing, merging, rebasing, etc., and entries will be added to the log file for you. Read more about automatic recording in [Set up automatic recording](./installation.md#set-up-automatic-recording).

- If you don't have automatic recording set up, run manually record activity:

    ```shell
    git activity record "starting issue #1"
    # hack hack
    git activity record "finished issue #1"
    ```

    (Doesn't that make you want to set up automatic recording?)

[^1]: Specifically, `awk` regex patterns.
