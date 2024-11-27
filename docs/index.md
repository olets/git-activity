---
titleTemplate: :title # see also VitePress config
---

# git-activity

![GitHub release (latest by date)](https://img.shields.io/github/v/release/olets/git-activity) ![GitHub commits since latest release](https://img.shields.io/github/commits-since/olets/git-activity/latest)

![git-activity splash card](/git-activity-card.png)

Record your Git activity across multiple (or all) repos, and read it optionally filtered by date, activity type (e.g. commit, branch creation, etc) regex pattern, repo name regex pattern, branch name regex pattern, commit message regex pattern, and/or commit SHA (first seven characters) regex pattern.[^1]

It's sort of like a global `git log` where you control which repos are watched and what types of activity are recorded. 

Useful for

- retroactively filling out a time sheet, or correcting a time sheet when you realize you left one timer running all day

- answering a weekly department "What projects did you work on last week and what did you do on them?" prompt

- finding that time you solved a problem similar to the one you're working on now

and more!

The log is stored in a text file. You can run your own programs on it if the git-activity CLI doesn't meet your needs, and you can share it across your computers for a cross-repo cross-machine log.

[^1]: Specifically, `awk` regex patterns.
