# prettier-eslint-cli

CLI for [`prettier-eslint`][prettier-eslint]

[![Build Status][build-badge]][build]
[![Code Coverage][coverage-badge]][coverage]
[![Dependencies][dependencyci-badge]][dependencyci]
[![version][version-badge]][package]
[![downloads][downloads-badge]][npm-stat]
[![MIT License][license-badge]][LICENSE]

[![All Contributors](https://img.shields.io/badge/all_contributors-13-orange.svg?style=flat-square)](#contributors)
[![PRs Welcome][prs-badge]][prs]
[![Donate][donate-badge]][donate]
[![Code of Conduct][coc-badge]][coc]
[![Roadmap][roadmap-badge]][roadmap]
[![Examples][examples-badge]][examples]

[![Watch on GitHub][github-watch-badge]][github-watch]
[![Star on GitHub][github-star-badge]][github-star]
[![Tweet][twitter-badge]][twitter]

## The problem

You have a bunch of files that you want to format using [`prettier-eslint`][prettier-eslint].
But `prettier-eslint` can only operate on strings.

## This solution

This is a [CLI](https://en.wikipedia.org/wiki/Command-line_interface) that allows you to use
`prettier-eslint` on one or multiple files. `prettier-eslint-cli` forwards on the `filePath`
and other relevant options to `prettier-eslint` which identifies the applicable `ESLint`
config for each file and uses that to determine the options for `prettier` and `eslint --fix`.

## Installation

This module is distributed via [npm][npm] which is bundled with [node][node] and should
be installed (with [`yarn`][yarn]) as one of your project's `devDependencies`:

```
yarn add --dev prettier-eslint-cli
```

> If you're still using the [`npm`][npm] client: `npm install --save-dev prettier-eslint-cli`

## Usage

Typically you'll use this in your [npm scripts][npm scripts] (or [package scripts][package scripts]):

```json
{
  "scripts": {
    "format": "prettier-eslint \"src/**/*.js\""
  }
}
```

This will format all `.js` files in the `src` directory. The argument you pass to the CLI
is a [glob][glob] and you can pass as many as you wish. You can also pass options.

### Vim

Vim users can add the following to their .vimrc:

```
autocmd FileType javascript set formatprg=prettier-eslint\ --stdin
```

This makes prettier-eslint-cli power the gq command for automatic formatting without any plugins. You can also add the following to your .vimrc to run prettier-eslint-cli when .js files are saved:

```
autocmd BufWritePre *.js :normal gggqG
```

### CLI Options

```
prettier-eslint --help
Usage: prettier-eslint <globs>... [--option-1 option-1-value --option-2]

Options:
  -h, --help       Show help                                           [boolean]
  --version        Show version number                                 [boolean]
  --write          Edit the file in-place (beware!)   [boolean] [default: false]
  --stdin          Read input via stdin               [boolean] [default: false]
  --eslint-ignore  Only format matching files even if they are not ignored by
                   .eslintignore. (can use --no-eslint-ignore to disable this)
                                                       [boolean] [default: true]
  --eslintPath     The path to the eslint module to use
    [default: "<path-to-root>/node_modules/eslint"]
  --prettierPath   The path to the prettier module to use
  [default: "<path-to-root>/node_modules/prettier"]
  --ignore         pattern(s) you wish to ignore (can be used multiple times and
                   includes **/node_modules/** automatically)
  --log-level, -l  The log level to use
        [choices: "silent", "error", "warn", "info", "debug", "trace"] [default:
                                                                         "warn"]
  --prettier-last  Run prettier last                  [boolean] [default: false]
```

#### <globs>

Any number of [globs][glob] you wish to use to match the files you wish to format. By default, `glob` will ignore
`**/node_modules/**` unless the glob you provide
includes the string `node_modules`.

#### --write

By default `prettier-eslint` will simply log the formatted version to the terminal. If you want to overwrite the file
itself (a common use-case) then add `--write`. You should quote your globs, otherwise your terminal will expand the glob before it gets to `prettier-eslint` (which can have unexpected results):

```json
{
  "scripts": {
    "format": "prettier-eslint --write \"src/**/*.js\""
  }
}
```

> **NOTE:** It is recommended that you keep your files under source control and committed
> before running `prettier-eslint --write` as it will overwrite your files!

#### --list-different

Instead of printing the formatted version of the files to the terminal, `prettier-eslint` will log the name of the files that are different from the expected formatting. This can be usefull when using `prettier-eslint` in a version control system hook to inform the committer which files need to be formatted.

#### --stdin

Accept input via `stdin`. For example:

```
echo "var   foo =    'bar'" | prettier-eslint --stdin
# results in: "var foo = 'bar';" (depending on your eslint config)
```

#### --eslint-path

Forwarded as the `eslintPath` option to `prettier-eslint`

#### --prettier-path

Forwarded as the `prettierPath` option to `prettier-eslint`

#### --log-level

Forwarded as `logLevel` option to `prettier-eslint`

#### --no-eslint-ignore

Disables application of `.eslintignore` to the files resolved from the glob. By
default, `prettier-eslint-cli` will exclude files if they are matched by a
`.eslintignore`. Add this flag to disable this behavior.

> Note: You can also set the `LOG_LEVEL` environment variable to control logging in `prettier-eslint`

#### --prettier-last

By default, `prettier-eslint-cli` will run `prettier` first, then `eslint --fix`. This is great if
you want to use `prettier`, but override some of the styles you don't like using `eslint --fix`.

An alternative approach is to use different tools for different concerns. If you provide the
argument `--pretter-last`, it will run `eslint --fix` first, then `prettier`. This allows you to
use `eslint` to look for bugs and/or bad practices, and use `prettier` to enforce code style.

#### --prettier
 
Passes prettier configuration options to prettier-eslint, such as `trailingComma` or `singleQuote`,
using the dot-notation.

For example: `prettier-eslint --prettier.trailing-comma es5`

The options supported are the same as [prettier-eslint](https://github.com/prettier/prettier-eslint#prettieroptions-object)

## Related

- [prettier-eslint](https://github.com/prettier/prettier-eslint) - the core package
- [prettier-eslint-atom](https://github.com/kentcdodds/prettier-eslint-atom) - an atom plugin

## Contributors

Thanks goes to these people ([emoji key][emojis]):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
| [<img src="https://avatars.githubusercontent.com/u/1500684?v=3" width="100px;"/><br /><sub>Kent C. Dodds</sub>](https://kentcdodds.com)<br />[💻](https://github.com/prettier/prettier-eslint-cli/commits?author=kentcdodds "Code") [📖](https://github.com/prettier/prettier-eslint-cli/commits?author=kentcdodds "Documentation") [🚇](#infra-kentcdodds "Infrastructure (Hosting, Build-Tools, etc)") [⚠️](https://github.com/prettier/prettier-eslint-cli/commits?author=kentcdodds "Tests") | [<img src="https://avatars3.githubusercontent.com/u/3266363?v=3" width="100px;"/><br /><sub>Adam Harris</sub>](https://github.com/aharris88)<br />[💻](https://github.com/prettier/prettier-eslint-cli/commits?author=aharris88 "Code") [📖](https://github.com/prettier/prettier-eslint-cli/commits?author=aharris88 "Documentation") [👀](#review-aharris88 "Reviewed Pull Requests") | [<img src="https://avatars.githubusercontent.com/u/622118?v=3" width="100px;"/><br /><sub>Eric McCormick</sub>](https://ericmccormick.io)<br />[👀](#review-edm00se "Reviewed Pull Requests") | [<img src="https://avatars.githubusercontent.com/u/12389411?v=3" width="100px;"/><br /><sub>Joel Sequeira</sub>](https://github.com/joelseq)<br />[📖](https://github.com/prettier/prettier-eslint-cli/commits?author=joelseq "Documentation") | [<img src="https://avatars.githubusercontent.com/u/103008?v=3" width="100px;"/><br /><sub>Frank Taillandier</sub>](https://frank.taillandier.me)<br /> | [<img src="https://avatars3.githubusercontent.com/u/292365?v=3" width="100px;"/><br /><sub>Adam Stankiewicz</sub>](http://sheerun.net)<br />[💻](https://github.com/prettier/prettier-eslint-cli/commits?author=sheerun "Code") | [<img src="https://avatars3.githubusercontent.com/u/487068?v=3" width="100px;"/><br /><sub>Stephen John Sorensen</sub>](http://www.stephenjohnsorensen.com/)<br />[💻](https://github.com/prettier/prettier-eslint-cli/commits?author=spudly "Code") |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| [<img src="https://avatars0.githubusercontent.com/u/11560964?v=3" width="100px;"/><br /><sub>Gandem</sub>](https://github.com/Gandem)<br />[💻](https://github.com/prettier/prettier-eslint-cli/commits?author=Gandem "Code") [⚠️](https://github.com/prettier/prettier-eslint-cli/commits?author=Gandem "Tests") | [<img src="https://avatars0.githubusercontent.com/u/129991?v=3" width="100px;"/><br /><sub>Matteo Ronchi</sub>](https://github.com/cef62)<br />[🐛](https://github.com/prettier/prettier-eslint-cli/issues?q=author%3Acef62 "Bug reports") [💻](https://github.com/prettier/prettier-eslint-cli/commits?author=cef62 "Code") | [<img src="https://avatars2.githubusercontent.com/u/61787?v=3" width="100px;"/><br /><sub>Benoît Zugmeyer</sub>](https://github.com/BenoitZugmeyer)<br />[💻](https://github.com/prettier/prettier-eslint-cli/commits?author=BenoitZugmeyer "Code") [⚠️](https://github.com/prettier/prettier-eslint-cli/commits?author=BenoitZugmeyer "Tests") | [<img src="https://avatars0.githubusercontent.com/u/5038030?v=3" width="100px;"/><br /><sub>Charlike Mike Reagent</sub>](https://i.am.charlike.online)<br />[💻](https://github.com/prettier/prettier-eslint-cli/commits?author=tunnckoCore "Code") [⚠️](https://github.com/prettier/prettier-eslint-cli/commits?author=tunnckoCore "Tests") | [<img src="https://avatars0.githubusercontent.com/u/10954870?v=3" width="100px;"/><br /><sub>Dion Dirza</sub>](https://github.com/diondirza)<br />[🐛](https://github.com/prettier/prettier-eslint-cli/issues?q=author%3Adiondirza "Bug reports") | [<img src="https://avatars0.githubusercontent.com/u/3297808?v=3" width="100px;"/><br /><sub>mrm007</sub>](https://github.com/mrm007)<br />[🐛](https://github.com/prettier/prettier-eslint-cli/issues?q=author%3Amrm007 "Bug reports") [💻](https://github.com/prettier/prettier-eslint-cli/commits?author=mrm007 "Code") |
<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors][all-contributors] specification. Contributions of any kind welcome!

## LICENSE

MIT

[yarn]: https://yarnpkg.com/
[npm]: https://www.npmjs.com/
[node]: https://nodejs.org
[build-badge]: https://img.shields.io/travis/prettier/prettier-eslint-cli.svg?style=flat-square
[build]: https://travis-ci.org/prettier/prettier-eslint-cli
[coverage-badge]: https://img.shields.io/codecov/c/github/prettier/prettier-eslint-cli.svg?style=flat-square
[coverage]: https://codecov.io/github/prettier/prettier-eslint-cli
[dependencyci-badge]: https://dependencyci.com/github/prettier/prettier-eslint-cli/badge?style=flat-square
[dependencyci]: https://dependencyci.com/github/prettier/prettier-eslint-cli
[version-badge]: https://img.shields.io/npm/v/prettier-eslint-cli.svg?style=flat-square
[package]: https://www.npmjs.com/package/prettier-eslint-cli
[downloads-badge]: https://img.shields.io/npm/dm/prettier-eslint-cli.svg?style=flat-square
[npm-stat]: http://npm-stat.com/charts.html?package=prettier-eslint-cli&from=2016-04-01
[license-badge]: https://img.shields.io/npm/l/prettier-eslint-cli.svg?style=flat-square
[license]: https://github.com/prettier/prettier-eslint-cli/blob/master/other/LICENSE
[prs-badge]: https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square
[prs]: http://makeapullrequest.com
[donate-badge]: https://img.shields.io/badge/$-support-green.svg?style=flat-square
[donate]: http://kcd.im/donate
[coc-badge]: https://img.shields.io/badge/code%20of-conduct-ff69b4.svg?style=flat-square
[coc]: https://github.com/prettier/prettier-eslint-cli/blob/master/other/CODE_OF_CONDUCT.md
[roadmap-badge]: https://img.shields.io/badge/%F0%9F%93%94-roadmap-CD9523.svg?style=flat-square
[roadmap]: https://github.com/prettier/prettier-eslint-cli/blob/master/other/ROADMAP.md
[examples-badge]: https://img.shields.io/badge/%F0%9F%92%A1-examples-8C8E93.svg?style=flat-square
[examples]: https://github.com/prettier/prettier-eslint-cli/blob/master/other/EXAMPLES.md
[github-watch-badge]: https://img.shields.io/github/watchers/prettier/prettier-eslint-cli.svg?style=social
[github-watch]: https://github.com/prettier/prettier-eslint-cli/watchers
[github-star-badge]: https://img.shields.io/github/stars/prettier/prettier-eslint-cli.svg?style=social
[github-star]: https://github.com/prettier/prettier-eslint-cli/stargazers
[twitter]: https://twitter.com/intent/tweet?text=Check%20out%20prettier-eslint-cli!%20https://github.com/prettier/prettier-eslint-cli%20%F0%9F%91%8D
[twitter-badge]: https://img.shields.io/twitter/url/https/github.com/prettier/prettier-eslint-cli.svg?style=social
[emojis]: https://github.com/kentcdodds/all-contributors#emoji-key
[all-contributors]: https://github.com/kentcdodds/all-contributors
[prettier-eslint]: https://github.com/prettier/prettier-eslint
[npm scripts]: https://docs.npmjs.com/misc/scripts
[package scripts]: https://github.com/kentcdodds/p-s
[glob]: https://github.com/isaacs/node-glob
