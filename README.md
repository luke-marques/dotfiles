# Dotfiles

My dotfiles, managed by [chezmoi](https://www.chezmoi.io/). Includes automatic
package and font setup (for MacOS only currently).

## Install

1. Retrieve the **existing age private key** from
   [Bitwarden](https://bitwarden.com/). Save its complete contents to
   `~/.config/chezmoi/key`:

   ```sh
   mkdir -p "$HOME/.config/chezmoi"
   (umask 077; touch "$HOME/.config/chezmoi/key")
   chmod 600 "$HOME/.config/chezmoi/key"
   vim "$HOME/.config/chezmoi/key"
   ```

   The key decrypts the paid Berkeley Mono fonts using
   [age](https://www.chezmoi.io/user-guide/encryption/age/). Keep it out of Git.

2. **Work Mac:** install Homebrew manually first. **Personal Mac:** Homebrew is
   installed automatically if missing.

3. Install chezmoi and apply this repo; choose **personal** or **work** when
   prompted:

   ```sh
   sh -c "$(curl -fsLS https://get.chezmoi.io)" -- \
     -b "$HOME/.local/bin" \
     init --source "$HOME/.dotfiles" --apply \
     https://github.com/luke-marques/config.git
   ```

   [Official installation docs](https://www.chezmoi.io/install/#one-line-binary-install).
   [Brewfile](dots/dot_config/homebrew/Brewfile.tmpl) packages install
   automatically; work machines skip casks. Fonts install into
   `~/Library/Fonts`.

4. Add these lines to `~/.zshrc`, then open a new terminal:

   ```sh
   eval "$(/opt/homebrew/bin/brew shellenv)"
   export PATH="$HOME/.local/bin:$(go env GOPATH)/bin:$PATH"
   ```

5. Authenticate GitHub with `gh auth login`.

## Sync

```sh
chezmoi diff       # Preview changes
chezmoi apply      # Apply local repo changes
chezmoi update     # Pull from GitHub and apply
```

Edit managed files under `~/.dotfiles/dots/`, then commit and push to share
changes. Package changes in the Brewfile template are installed on the next
apply. To change the machine choice, run `chezmoi init --prompt`, then
`chezmoi apply`.
