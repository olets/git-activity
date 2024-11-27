# Options

Variable | Added in | Type | Default | Use
---|---|---|---|---
`GIT_ACTIVITY_CSV_PATH` | `1.0.0` | String | If `XDG_CONFIG_HOME` is set, `$XDG_CONFIG_HOME/git-activity/git-activity.csv`<br>Otherwise, `$HOME/.config/git-activity/git-activity.csv` | Path to the log file

To customize a configuration variable, `export` it from your shell configuration file. For example

```shell
# .zshrc
export GIT_ACTIVITY_CSV_PATH="$HOME/git-activity.csv"
```
