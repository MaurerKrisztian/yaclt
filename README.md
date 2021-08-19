![Build](https://github.com/mrjones2014/yaclt/actions/workflows/build.yml/badge.svg) ![NPM Publish](https://github.com/mrjones2014/yaclt/actions/workflows/publish.yml/badge.svg) [![Discord Server](https://img.shields.io/discord/868246245343367169?logo=discord&style=flat)](https://discord.gg/dv5x7tjqYk)

<img src="https://github.com/mrjones2014/yaclt/raw/master/images/logo_color_on_transparent.png" alt="yaclt logo" height="225"/>

# `yaclt`

What is `yaclt`? It's <ins>**Y**</ins>et <ins>**A**</ins>nother <ins>**C**</ins>hange<ins>**L**</ins>og <ins>**T**</ins>ool.

`yaclt` is an opinionated, yet highly configurable, file-based changelog generator and validator.

The idea is basically:

- For each pull request/merge request, include a changelog file generated by this tool -- it gets stored in source control until the next release
- To prepare a release, use the `prepare-release` command, which will:
  - Run the validator on all existing changelog entries
  - Gather all the individual changelog entries into a global `CHANGELOG.md` (or whatever file you've specified)
  - Delete all the individual changelog files

The changelog entry format and the global changelog file template are both [Handlebars](https://handlebarsjs.com) templates, and you can use the extra helpers from [`handlebars-helpers`](https://github.com/helpers/handlebars-helpers).

Help for each command can be found in [COMMANDS.md](./COMMANDS.md) or by running the command with the `--help` argument.

## Dependencies

Node 12+, and to be run from within a git repo.

## Configuration

All command line flags and arguments can be specified in a configuration file. Supported file names are `yacltrc.yml`, `yacltrc.yaml`, `yacltrc.json`, and `yacltrc.js` (`.js` config must use `module.exports =`).

If the working directory is inside a git repository, the tree will be traversed to the git root, using the first valid configuration file that is found. If no configuration file is found in the repo,
it will also check, in order of precedence, `$YACLT_CONFIG_HOME/`, `$XDG_CONFIG_HOME/yaclt/`, `$HOME/.config/yaclt/` for global configuration files.

If you are using a Javascript configuration file (e.g. `yacltrc.js`), any of the options can be a parameterless function which returns a value of the same type expected by the option. This can be useful,
for example, if you'd like to write some custom logic to parse the next release number based on git tags, or generate your messages automatically from git commit messages.

For options which are Handlebars templates (e.g. `--format`, `--releaseBranchPattern`, or `--changelogTemplate`), you may specif a filepath instead of a literal template string.
The CLI will check if the argument is a filepath to a file that exists, and if so, read the template from that file.

## Handlebars Variables

### Individual Changelog Entry Template

| Name         | Description                                                                                       | Required                                  |
| ------------ | ------------------------------------------------------------------------------------------------- | ----------------------------------------- |
| `changeType` | The change type for the entry, e.g. `NEW`, `IMPROVED`, `FIXED`, or custom ones you've configured. | `true`                                    |
| `message`    | The change log entry message.                                                                     | `true`                                    |
| `issueId`    | The issue ID. This is optional based on your config.                                              | `false` if `--requireIssueIds` is `false` |

### Global Changelog Template

| Name            | Description                                                                                                                                                        | Required |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------- |
| `releaseNumber` | The release number or label, specified by `--releaseNumber` flag or from configuration file                                                                        | `true`   |
| `entryGroups`   | An array of objects with the interface `{ label: string; items: string[]; }` where `label` is the change type and `items` is all the entries with that change type | `true`   |

### Release Branch Pattern

| Name            | Description                                                                                 | Required |
| --------------- | ------------------------------------------------------------------------------------------- | -------- |
| `releaseNumber` | The release number or label, specified by `--releaseNumber` flag or from configuration file | `true`   |

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md)
