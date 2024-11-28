# git-activity ![GitHub release (latest by date)](https://img.shields.io/github/v/release/olets/git-activity) ![GitHub commits since latest release](https://img.shields.io/github/commits-since/olets/git-activity/latest)

**git-activity**: Record your Git activity across multiple (or all) repos, and read it optionally filtered by date, activity type (e.g. commit, branch creation, etc) regex pattern, repo name regex pattern, branch name regex pattern, commit message regex pattern, and/or commit SHA (first seven characters) regex pattern.

It's sort of like a global `git log` where you control which repos are watched and what types of activity are recorded. 

Useful for

- retroactively filling out a time sheet, or correcting a time sheet when you realize you left one timer running all day

- answering a weekly department "What projects did you work on last week and what did you do on them?" prompt

- finding that time you solved a problem similar to the one you're working on now

The log is stored in a text file. You can run your own programs on it if the git-activity CLI doesn't meet your needs, and you can share it across your computers for a cross-repo cross-machine log.

## Quick start

```shell
> brew install olets/tap/git-activity
# close the terminal
# open a new terminal
> git activity install --global # just once
# go about your day
> git activity show
```

And then read up on all the ways you can filter `git activity show` in [the command's documentation](https://git-activity.olets.dev/show).

&nbsp;

## Documentation

📖 https://git-activity.olets.dev/

&nbsp;

## Changelog

See the [CHANGELOG](CHANGELOG.md) file.

## Contributing

Thanks for your interest. Contributions are welcome!

> Please note that this project is released with a [Contributor Code of Conduct](CODE_OF_CONDUCT.md). By participating in this project you agree to abide by its terms.

Check the [Issues](https://github.com/olets/git-activity/issues) to see if your topic has been discussed before or if it is being worked on.

Please read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request.

## License

<a href="https://www.github.com/olets/git-activity">git-activity</a> by <a href="https://olets.dev">Henry Bley-Vroman</a> is, with the exception of its logo as covered below, licensed under a license which is the unmodified text of <a href="https://creativecommons.org/licenses/by-nc-sa/4.0">CC BY-NC-SA 4.0</a> and the unmodified text of a <a href="https://firstdonoharm.dev/build?modules=eco,extr,media,mil,sv,usta">Hippocratic License 3</a>. It is not affiliated with Creative Commons or the Organization for Ethical Source.

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
