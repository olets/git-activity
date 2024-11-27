# Quick start

1. Install git-activity

    ```shell
    brew install olets/tap/git-activity
    ```

1. Close the terminal, and open a new terminal.

1. Set up automatic recording in all new repos

    ```shell
    git activity install --global
    ```

1. Set up automatic recording in existing repos

    ```shell
    cd my-repo
    git activity install
    ```

1. Use Git for a while.

1. Review your Git activity

    ```shell
    git activity show
    ```

1. Read up on all the ways you can filter `git activity show`, at [Usage&nbsp;>&nbsp;Show](./show.md).
