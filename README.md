### Setup

1. Install Xcode, and the Xcode dev tools with `xcode-select --install`
2. Get vim and zsh to your liking:

```
  git clone https://github.com/kyletns/dotfiles.git ~/.dotfiles
  cd ~
  ln -s ~/.dotfiles/.zshrc
  ln -s ~/.dotfiles/.vimrc

  mkdir ~/.vim
  mkdir ~/.vim/backup
  mkdir ~/.vim/swap
  mkdir ~/.vim/undo

  git clone https://github.com/gmarik/Vundle.vim.git ~/.vim/bundle/Vundle.vim

  vim +PluginInstall +qall

  chsh -s $(which zsh)
  curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh
```


3. Brew bundle!

Install Homebrew:

```
  ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

4. Install all apps with Brew and Cask:

```
  brew tap homebrew/bundle
  brew bundle
```

5. Install [Groupmuse](https://github.com/kyletns/groupmuse/wiki/Setup)!


#### Additional notes

Vim

On my most recent upgrade (Catalina), Macvim broke, which I fixed by uninstalling vim + macvim via Homebrew, and leaving only the system vim in /usr/bin/vim. Then instead of using Homebrew, I just downloaded Macvim.app directly from their website, and added the `bin` folder within it's Package Contents to my PATH in `~/.zshrc`. Homebrew insisted on building it's own vim / it's own ruby, and kept pointing to the wrong folder and crashing upon open.

Postgres.app

Once setup, ensure that the version/directory in ~/.zshrc correctly appends
the postgres tools to your PATH. That provides db_create and many other tools.

Heroku setup

```
  heroku keys:add
  heroku git:remote -a groupmuse
```

Quick db copy

```
  heroku pg:backups capture  -a groupmuse
  curl -o production.dump `heroku pg:backups public-url -a groupmuse`
  dropdb groupmuse_development
  createdb groupmuse_development
  pg_restore --verbose --clean --no-acl --no-owner -h localhost -d groupmuse_development production.dump
  rdbm
```

Optional, safety first:

```
  rake scrub_database
  rm -P production.dump
```
