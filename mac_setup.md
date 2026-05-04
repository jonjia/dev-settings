## MacOS settings

- **Apple ID**: Sign in
- **MacOS update**: Settings -> General -> Software Update
- **Siri**: Disable Ask Siri
- **Trackpad**: Max tracking speed, tap to click, click = light, natural scroll = off
- **Dictionary lookup**: Trackpad -> Point & Click -> Look up & data detectors off
- **Displays**: Switch to Apple Display (p3-600) for slightly better battery life
- **Finder**: Show Library, show hidden files, show path to dir, prune excessive sidebar bookmarks
```bash
# show Library folder
chflags nohidden ~/Library

# show hidden files
defaults write com.apple.finder AppleShowAllFiles YES

# add pathbar to title
defaults write com.apple.finder _FXShowPosixPathInTitle -bool true

# restart finder
killall Finder;
```
- **Screenshots** (to clipboard): `CMD + SHIFT + F5` and change setting in “Option” menu. This lets me screenshot and paste into docs, chats, social media, etc directly (without saving a separate file). If I need to save it, open Preview and `CMD + N`.
- **Dock / MacOS 扩展坞**: Hide and show dock, reduce size, remove everything from the Dock except Finder, System Preferences, and Trash

## Basic developer tools

- **XDG config home**
```bash
launchctl setenv XDG_CONFIG_HOME "$HOME/.config"
```
- **Homebrew** (might take a while as it also installs Xcode)
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# add brew to default shell path
echo >> /Users/jiawenhui/.zprofile
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/jiawenhui/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"

# check for updates
brew update
```
- **Terminal**: Ghostty
```bash
brew install --cask ghostty
mkdir -p ~/.config/ghostty

cat > ~/.config/ghostty/config.ghostty <<'EOF'
theme = Misterioso
font-size = 16
window-inherit-working-directory = false
EOF
```
- **Shell**: Nushell
```bash
brew install nushell
mkdir -p ~/.config/nushell

cat > ~/.config/nushell/brew.nu <<'EOF'
$env.HOMEBREW_PREFIX = "/opt/homebrew"
$env.PATH = ($env.PATH | prepend "/opt/homebrew/bin" | prepend "/opt/homebrew/sbin")
EOF

cat > ~/.config/nushell/env.nu <<'EOF'
source ~/.config/nushell/brew.nu
EOF

cat > ~/.config/nushell/config.nu <<'EOF'
$env.config.show_banner = false
$env.PATH = ($env.PATH | prepend ($env.HOME | path join ".bun" "bin"))
$env.PATH = ($env.PATH | prepend "/opt/homebrew/opt/node@24/bin")

alias python = uv run python
alias python3 = uv run python
alias pip = uv pip
EOF
```
- **Htop**: A better top
```bash
brew install htop
```
- **New SSH keys**
```bash
ssh-keygen -t ed25519 -C "[email protected]"

eval "$(ssh-agent -s)"
touch ~/.ssh/config
open ~/.ssh/config

# add to config
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```

Also see the Github docs to [generate a new ssh key and add to ssh agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent), and [add a new ssh key to Github account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).

- **Git**
```bash
brew install git
git config --global init.defaultBranch main
git config --global user.name "jonjia"
git config --global user.email [email protected]
```

## Development

- **Zed**
```bash
brew install --cask zed
```

- **Chrome**
```bash
brew install --cask google-chrome
```

- **Python, JS**

```bash
# very fast python package manager
brew install uv
# Shell autocompletion
^uv --generate-shell-completion nushell | save -f ~/.config/nushell/completions/uv.nu
^uvx --generate-shell-completion nushell | save -f ~/.config/nushell/completions/uvx.nu

# .config/nushell/config.nu
source ~/.config/nushell/uv.nu
source ~/.config/nushell/uvx.nu

# install the latest python
uv python install 3.12
```


```bash
brew install node@24

# bun
brew install oven-sh/bun/bun
```

- **Docker**

```bash
brew install orbstack
```

## AI Coding

- **Codex**
```bash
brew install codex
```

- **Codex App**

```bash
brew install --cask codex-app
```

- **Typeless**

```bash
brew install --cask typeless
```
