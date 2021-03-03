# Laptop

Laptop is a script to set up a macOS laptop for web and mobile development.

It can be run multiple times on the same machine safely.
It installs, upgrades, or skips packages
based on what is already installed on the machine.

This project is forked & derived from the [thoughtbot/laptop](https://github.com/thoughtbot/laptop) project, but customized for my own use.

## Install

Download the script:

```sh
curl --remote-name https://raw.githubusercontent.com/lachlanjc/laptop/main/mac
```

Review the script (avoid running scripts you haven’t read!):

```sh
less mac
```

Execute the downloaded script:

```sh
sh mac 2>&1 | tee ~/laptop.log
```

Optionally, review the log:

```sh
less ~/laptop.log
```

Your last Laptop run will be saved to `~/laptop.log`.

## What it sets up

First, it shows hidden (dot)files on your Mac.

macOS tools:

- [Homebrew] for managing operating system libraries.

[homebrew]: http://brew.sh/

Unix tools:

- [Universal Ctags] for indexing files for vim tab completion
- [Git] for version control
- [OpenSSL] for Transport Layer Security (TLS)
- [RCM] for managing company and personal dotfiles
- [The Silver Searcher] for finding things in files
- [Tmux] for saving project state and switching between projects
- [Watchman] for watching for filesystem events
- [Zsh] as your shell

[universal ctags]: https://ctags.io/
[git]: https://git-scm.com/
[openssl]: https://www.openssl.org/
[rcm]: https://github.com/thoughtbot/rcm
[the silver searcher]: https://github.com/ggreer/the_silver_searcher
[tmux]: http://tmux.github.io/
[watchman]: https://facebook.github.io/watchman/
[zsh]: http://www.zsh.org/

Heroku tools:

- [Heroku CLI] and [Parity] for interacting with the Heroku API

[heroku cli]: https://devcenter.heroku.com/articles/heroku-cli
[parity]: https://github.com/thoughtbot/parity

GitHub tools:

- [GitHub CLI] for interacting with the GitHub API

[github cli]: https://cli.github.com/

Image tools:

- [ImageMagick] for cropping and resizing images
- [Guetzli] for optimizing images

Programming languages, package managers, and configuration:

- [Bundler] for managing Ruby libraries
- [Node.js] (latest LTS version) & [npm], for running apps and installing JavaScript packages, via [tj/n]
- [Ruby] (latest stable version) via [rbenv]
- [Yarn] for managing JavaScript packages

[bundler]: http://bundler.io/
[imagemagick]: http://www.imagemagick.org/
[guetzli]: https://github.com/google/guetzli
[node.js]: http://nodejs.org/
[npm]: https://www.npmjs.org/
[tj/n]: https://github.com/tj/n
[ruby]: https://www.ruby-lang.org/en/
[rbenv]: https://rbenv.org
[yarn]: https://yarnpkg.com/en/

Databases:

- [Postgres] for storing relational data
- [Redis] for storing key-value data

[postgres]: http://www.postgresql.org/
[redis]: http://redis.io/

Node.js tools:

- [Prettier](https://prettier.io) for formatting
- [Vercel](https://vercel.com) for deployment
- [npm-check-updates](https://www.npmjs.com/package/npm-check-updates) for bulk updating packages

Apps:

- 1Password
- AppCleaner
- BBEdit
- Figma
- GitHub Desktop
- Google Chrome
- Hey
- Hyper
- Notion
- Slack
- VSCode (with the VSCode Icons, Prettier, Vim, MDX, & Framer Syntax 2 plugins)
- Zoom

(It’s easy to remove these apps/install different ones by quickly editing the end of `mac` with TextEdit/Vim/etc.)

Other apps I sometimes add:

```sh
brew install "chronosync"
brew install "cocktail"
brew install "daisydisk"
```

It should take less than 15 minutes to install (depends on your machine).

## Customize in `~/.laptop.local`

Your `~/.laptop.local` is run at the end of the Laptop script.
Put your customizations there.

<details>
  <summary>Example customization file</summary>

```sh
#!/bin/sh

brew bundle --file=- <<EOF
brew "Caskroom/cask/dockertoolbox"
brew "go"
brew "ngrok"
brew "watch"
EOF

default_docker_machine() {
  docker-machine ls | grep -Fq "default"
}

if ! default_docker_machine; then
  docker-machine create --driver virtualbox default
fi

default_docker_machine_running() {
  default_docker_machine | grep -Fq "Running"
}

if ! default_docker_machine_running; then
  docker-machine start default
fi

fancy_echo "Cleaning up old Homebrew formulae ..."
brew cleanup
brew cask cleanup

if [ -r "$HOME/.rcrc" ]; then
  fancy_echo "Updating dotfiles ..."
  rcup
fi
```

</details>

Write your customizations such that they can be run safely more than once.
See the `mac` script for examples.

Laptop functions such as `fancy_echo` and
`gem_install_or_update`
can be used in your `~/.laptop.local`.

See the [original wiki](https://github.com/thoughtbot/laptop/wiki)
for more customization examples.

## Contributing

Edit the `mac` file.
Document in the `README.md` file.
Follow shell style guidelines by using [ShellCheck] and [Syntastic].

```sh
brew install shellcheck
```

[shellcheck]: http://www.shellcheck.net/about.html
[syntastic]: https://github.com/scrooloose/syntastic

## License

Laptop is © 2011-2020 thoughtbot, inc.
It is free software,
and may be redistributed under the terms specified in the [LICENSE] file.

[license]: LICENSE
