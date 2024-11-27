# Examples

1. Wondering how you spent your day?

    ```shell
    git activity show today
    ```
    
    "Okay I did Project A from 8:16 to 10:20, Project B from 10:20 to 10:53, I know I was in a meeting 11-12, was back on Project A by 12:08, project C 1:18-1:40, project B 2:10-5:04 but oh look I would have forgotten: project A 3:02-3:06."

1. Wondering what projects you worked on last week, and what did you do on them?

    ```shell
    git activity "last week monday" && git activity "last week tuesday" && git activity "last week wednesday" && git activity "last week thursday" && git activity "last week friday"
    ```

    (You could make an alias for that. Or an abbreviation! fish has native abbreviations. Zsh has [zsh-abbr](https://zsh-abbr.olets.dev/), by the creator of `git-activity`.)

1. Looking for that time you solved a problem similar to the one you're working on now?

    ```shell
    git activity --commit-message-pattern='(card.*pay)|(pay.*card)'
    ```