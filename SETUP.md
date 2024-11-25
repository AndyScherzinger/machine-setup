# Setup

## Development

### Signed git commits

- https://git-scm.com/download/win
- https://gpg4win.org/download.html
- https://neurotechnics.com/blog/configure-gpg-to-sign-git-commits-in-windows/

on cmd _not_ powershell (!):

- `gpg --import secret.gpg`
- `git config --global user.signingkey <key>`
- `git config --global commit.gpgsign true`
- `git config --global gpg.program "C:\Program Files (x86)\GnuPG\bin\gpg.exe"`

#### Useful commands

- all contributors: `git log --format='%aN <%aE>' | LC_ALL=C.UTF-8 sort -uf`
- execution right on file: `git update-index --chmod=+x npm-post-build.sh`

### Python

- https://www.python.org/downloads/windows/ (3.12.7)
- install reuse `pip install reuse`
- update reuse `pip install --upgrade reuse`
- update pip `python.exe -m pip install --upgrade pip`

### Android / Java

- https://aws.amazon.com/de/corretto/
- https://developer.android.com/studio?hl=en

### Useful commands

- `gradlew --write-verification-metadata sha256 help --export-keys`

### Phh / Web

- https://windows.php.net/download/
- https://getcomposer.org/download/
- https://gnuwin32.sourceforge.net/packages/make.htm + $PATH

### General dev tools

- https://getgreenshot.org/downloads/
- https://winmerge.org/downloads/
- Peek Screen recoder (via app store)

## General Tooling

- https://store.serif.com/en-gb/update/windows/designer/2/
- https://store.serif.com/en-gb/update/windows/photo/2/
- https://de.libreoffice.org/download/download/
- https://www.thunderbird.net/de/
- https://www.mozilla.org/de/firefox/
- https://www.google.com/intl/de_de/chrome/
- https://www.philips-hue.com/de-de/explore-hue/propositions/entertainment/sync-with-pc
- WinDirStat
- https://github.com/kee-org/keepassrpc/releases
- https://github.com/xatupal/KeeTheme/releases
- https://download.nextcloud.com/desktop/daily/windows/
