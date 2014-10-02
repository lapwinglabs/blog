```
title: Hacker's Guide to Setting up Your Mac
tags: [mac, hacker, bash]
date: September 30, 2014
slug: hacker-guide-to-setting-up-your-mac
excerpt: Setting up your Mac the Hacker way
author: Matthew Mueller
```

Hackers obsess over automation. We want robots to do the grunt work so we can focus on the fun stuff. One area that's ripe for automation that hasn't seen much attention lately is setting up your computer.

Today I want to show you some techniques to apply automation to the setup of your Mac. The goal of this post is to automate 80% of the bootstrapping, allowing you to setup a new Mac in a matter of hours, not days.

![terminal](https://raw.githubusercontent.com/lapwinglabs/blog/master/assets/hacker-guide-to-setting-up-your-mac/terminal.png)

## Previous Work

There has been previous work done in this area to automate your Mac's setup. [Boxen](https://boxen.github.com/) is probably the most notable. Boxen is Github's solution to keeping their teams running similar environments so there aren't as many inconsistencies across boxes. Boxen is a great solution for more mature companies with devops teams
, but what about the small startups or the lone hackers? We need a more suitable solution for them.

## Our toolbox

This blog post will make use of the following open source tools to automate your Mac:

- Installing Binaries with [homebrew](http://brew.sh/)
- Installing Apps with [homebrew cask](http://caskroom.io/)
- Backing up and Restoring Configuration with [mackup](https://github.com/lra/mackup)
- Solid Mac defaults for hackers using [osx-for-hackers.sh](https://gist.github.com/brandonb927/3195465) (modified)
- Bringing it all together with [dots](https://github.com/matthewmueller/dots)

## Installing Binaries with Homebrew

Homebrew is a community-driven package installer and an essential tool for every hacker's toolkit. Homebrew automates the setup, compiling and linking of binaries. It also makes updates and uninstalling a binary a breeze.

This is the first thing you should install on a fresh mac. Drop this snippet in a bash script to make sure homebrew gets installed:

```bash
# Check for Homebrew,
# Install if we don't have it
if test ! $(which brew); then
  echo "Installing homebrew..."
  ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
fi

# Update homebrew recipes
brew update
```

The next thing you should do is update the unix tools you already have on your mac. This is more relevant than ever since the recent ["Shellshock"](http://goo.gl/zElPKk) debacle.

Here's a snippet to update these unix tools:

```bash
# Install GNU core utilities (those that come with OS X are outdated)
brew install coreutils

# Install GNU `find`, `locate`, `updatedb`, and `xargs`, g-prefixed
brew install findutils

# Install Bash 4
brew install bash

# Install more recent versions of some OS X tools
brew tap homebrew/dupes
brew install homebrew/dupes/grep
```

You'll also need to update the `$PATH` in your `~/.bash_profile` in order to use these tools over their Mac counterparts:

```bash
$PATH=$(brew --prefix coreutils)/libexec/gnubin:$PATH
```

This establishes a solid foundation for your Mac. You should also install other tools using Homebrew to improve your workflow. Here's what I install:

```bash
brew install graphicsmagick
brew install webkit2png
brew install rename
brew install zopfli
brew install ffmpeg
brew install python
brew install sshfs
brew install trash
brew install node
brew install tree
brew install ack
brew install hub
brew install git
```

After you're done, you should clean everything up with:

```bash
brew cleanup
```

## Installing Apps with Homebrew Cask

[Homebrew Cask](http://caskroom.io/) is an extension for Homebrew that allows you to automate the installation of Mac Apps and Fonts.

After you have homebrew installed, you'll want to tap and install Homebrew Cask:

```bash
brew tap phinze/homebrew-cask
brew install brew-cask
```

The number of apps you can install with Cask is enormous and growing every day. You can take a look at what applications are installable in their [caskroom/homebrew-cask](https://github.com/caskroom/homebrew-cask/tree/master/Casks) repo or you can search for applications from the CLI:

```bash
brew cask search /google-chrome/
```

Everyone's choice of apps will be different, but here is the script I use to install my favorite apps:

```bash
# Apps
apps=(
  alfred
  dropbox
  google-chrome
  qlcolorcode
  screenflick
  slack
  transmit
  appcleaner
  firefox
  hazel
  qlmarkdown
  seil
  spotify
  vagrant
  arq
  flash
  iterm2
  qlprettypatch
  shiori
  sublime-text3
  virtualbox
  atom
  flux
  mailbox
  qlstephen
  sketch
  tower
  vlc
  cloudup
  font-m-plus
  nvalt
  quicklook-json
  skype
  transmission
)

# Install apps to /Applications
# Default is: /Users/$user/Applications
echo "installing apps..."
brew cask install --appdir="/Applications" ${apps[@]}
```

If you want to install beta versions of things like Chrome Canary or Sublime Text 3, you'll need to tap the `versions` cask:

```bash
brew tap caskroom/versions
```

### Attention Alfred users

One thing you may notice if you're an [Alfred](http://www.alfredapp.com/) user is that you cannot actually launch these apps from Alfred because the actual location of the app is not in `/Applications` but in `/opt/homebrew-cask/Caskroom/`.

To add this path to Alfred, you can run the following command:

```bash
brew cask alfred link
```

Voila!

### Bonus: Installing Fonts like a Boss

Cask can also be used to automatically download and install fonts. In order to enable this, you'll need to tap the `fonts` cask:

```bash
brew tap caskroom/fonts
```

The font recipes are prefixed by `font-*`, so if you want to download [Roboto](http://www.google.com/fonts/specimen/Roboto), try searching for `font-roboto`:

```bash
brew cask search /font-roboto/
```

Here's how I install fonts:

```bash
# fonts
fonts=(
  font-m-plus
  font-clear-sans
  font-roboto
)

# install fonts
echo "installing fonts..."
brew cask install ${fonts[@]}
```

You can find a full list of the fonts in the [caskroom/homebrew-fonts](https://github.com/caskroom/homebrew-fonts/tree/master/Casks) repo.

## Mackup

[Mackup](https://github.com/lra/mackup) is a community-driven tool for backing up and restoring system and application settings. You can find the list of applications it supports in the [lra/mackup](https://github.com/lra/mackup/tree/master/mackup/applications) repo.

I haven't had much luck installing Mackup using Homebrew (on Yosemite), but it's easy enough to install with python's `pip`:

```bash
pip install mackup
```

If `pip` is not available, you may need to install `python` with `brew install python`.

By default mackup saves your preferences to your Dropbox, so you'll want to setup Dropbox first. Once Dropbox is setup, backing up your settings is simple:

```bash
mackup backup
```

This command will look match your installed applications with it's recipes and symlink the settings files to `~/Dropbox/Mackup`.

To restore these settings on another Mac or a wiped Mac, simply run:

```bash
mackup restore
```

## osx-for-hackers.sh

[osx-for-hackers.sh](https://gist.github.com/brandonb927/3195465) is a script by [Brandon Brown](https://github.com/brandonb927) that is based on [Mathias Bynens](https://github.com/mathiasbynens)'s popular [dotfiles](https://github.com/mathiasbynens/dotfiles/blob/master/.osx).

This script optimizes your Mac's settings for hackability. It disables many of the annoying defaults Macs have, speeds up the keyboard repeat rate and window animations, and applies many other tweaks.

This script should not be run without prior examination. It's quite opinionated and intended to be modified. You can find the version I modified here:

https://gist.github.com/MatthewMueller/e22d9840f9ea2fee4716

This version makes the script more idempotent, removing a lot of the prompts that I'd like to handle in other places.

## dots(1)

[dots(1)](https://github.com/matthewmueller/dots) is a script I wrote to glue these concepts together. It's intended to be the first thing you install on your Mac (or Ubuntu server). It has no outside dependencies and works on many different distributions. To get the binary, simply run:

```
(mkdir -p /tmp/dots && cd /tmp/dots && curl -L# https://github.com/matthewmueller/dots/archive/master.tar.gz | tar zx --strip 1 && sh ./install.sh)
```

To boot up your Mac with sensible defaults, you can run:

```bash
dots boot osx
```

[dots(1)](https://github.com/matthewmueller/dots) is very much a work in progress, but I'm hoping to align the community's efforts around creating robust tools to quickly bootstrap new hacker-friendly machines.

## Conclusion

By setting up automation, you can get up and running on a new Mac faster. You will stay up to date with the latest security fixes and you can eliminate inconsistencies among your teammate's computers.

What are your favorite tools for automation? Leave a comment!

If you're interested in this kind of stuff or in our [other work](http://lapwinglabs.com/#work), you should [get in touch](mailto:hi@lapwinglabs.com).

Happy automating!
