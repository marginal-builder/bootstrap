# Bootstrap

Set up a new Mac from scratch.

## TL;DR Run the installer

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/marginal-builder/bootstrap/main/install.sh)"
```

The installer opens a browser twice for a GitHub device-code prompt: once to log
in and upload this machine's SSH key, and once to hand the key-management scopes
back afterwards so the everyday token keeps only the default permissions.

The script is idempotent, so you can re-run it as often as you want once it's
installed.

The installer asks for your password once at the start, then installs a temporary
`/etc/sudoers.d/bootstrap-nopasswd` rule so the rest of the run you never have to
enter your password again. The rule is removed when the script exits.

## Before the first run

- **Desktops:** create the Desktops listed in `spaces-items.txt` in Mission
  Control (four, by default). `spaces-setup.sh` pauses until they exist.
- **Mail:** off by default. Add Gmail, iCloud, or Exchange accounts in
  **System Settings → Internet Accounts**. For custom IMAP accounts, create a
  Bitwarden folder called `bootstrap-mail` with one login per account (username =
  email address, custom field `host` = mail server), uncomment the mail block in
  `install.sh`, and run with `SETUP_MAIL=1`.
- **Bitwarden:** only needed if you enable the custom IMAP mail step.

## What it installs

- Homebrew packages from `Brewfile` (a lean dev set plus Firefox, Ghostty, Figma
  and Bitwarden)
- SSH key and signed Git commits
- macOS preferences, Dock layout, Space assignments
- Firefox `user.js` and `userChrome.css`
- FigmaAgent login-item block
- Your `dotfiles` repo, linked into `~/.config`

## After the run

1. Restart the Mac to load the key remapping and Firefox settings.
2. Open a new terminal window so the new shell config loads.
3. Add Gmail, iCloud, or Exchange accounts in
   **System Settings → Internet Accounts**.
4. Optional: add more packages to `Brewfile` and run
   `brew bundle --file ~/git/bootstrap/Brewfile`.

## Customization

The files worth editing for your own needs:

- `Brewfile`
- `install.sh`'s settings at the top of the file
- `spaces-items.txt` (Desktop number and bundle ID)
- `dock-items.txt`
- `firefox/user.js` and `firefox/userChrome.css`

### Optional steps

Each step can be turned off with an environment variable when running the
installer:

```sh
RUN_SOFTWAREUPDATE=0 SETUP_SPACES=0 \
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/marginal-builder/bootstrap/main/install.sh)"
```

Available flags: `RUN_SOFTWAREUPDATE`, `SETUP_DOTFILES`, `SETUP_PREFERENCES`,
`SETUP_DOCK`, `SETUP_SPACES`, `SETUP_FIREFOX`, `SETUP_FIGMA`, `SETUP_MAIL`
(off by default), `SETUP_SKHD` (off by default).
