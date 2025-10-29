# Setup

## Development

### Activate sym link support

1. Enable "Developer Mode" in Windows 10/11, gives `mklink` permissions (System->Developer Mode _or_ Terminal Settings)
2. Ensure symlinks are enabled in git with (at least) one of
  - System setting: check the checkbox when installing msysgit
  - Global setting: `git config --global core.symlinks true`
  - Local setting: `git config core.symlinks true`

*And not overridden by a lower level config set to false.*

### Signed git commits

- https://git-scm.com/download/win
- https://gpg4win.org/download.html
- https://neurotechnics.com/blog/configure-gpg-to-sign-git-commits-in-windows/

on cmd _not_ powershell (!):

- `gpg --import secret.gpg`
- `git config --global user.signingkey <key>`
- `git config --global commit.gpgsign true`
- `git config --global gpg.program "C:\Program Files (x86)\GnuPG\bin\gpg.exe"`

#### Useful git commands

- all contributors: `git log --format='%aN <%aE>' | LC_ALL=C.UTF-8 sort -uf`
- execution right on file: `git update-index --chmod=+x npm-post-build.sh`
- cleanup checkout: `git remote prune origin` plus `git gc`

### Python

- https://www.python.org/downloads/windows/
- install reuse `pip install reuse`
- update reuse `pip install --upgrade reuse`
- install zizmor (GH action checker) `pip install zizmor` (https://docs.zizmor.sh/quickstart/)
- update zizmor `pip install --upgrade zizmor`
- update pip `python.exe -m pip install --upgrade pip`

### Android / Java

- https://aws.amazon.com/de/corretto/
- https://developer.android.com/studio?hl=en

### Useful gradle commands

- `gradlew --write-verification-metadata sha256 help --export-keys`
- `gradlew spotbugsDebug --console=plain --dependency-verification lenient -q --write-verification-metadata sha256,pgp dependencies --export-keys`

### Useful drone commands

- `drone sign nextcloud/android --save`

### PHP / Web

- https://windows.php.net/download/
- https://getcomposer.org/download/
- https://github.com/coreybutler/nvm-windows
- https://gnuwin32.sourceforge.net/packages/make.htm + $PATH

### General dev tools

- https://getgreenshot.org/downloads/
- https://winmerge.org/downloads/
- Peek Screen recoder (via app store)
- `winget install --id Microsoft.DevHome -e` for https://github.com/microsoft/devhome
- Update winget, get latest `.msixbundle` from https://github.com/microsoft/winget-cli/releases/

## General Tooling

- https://store.serif.com/en-gb/update/windows/designer/2/
- https://store.serif.com/en-gb/update/windows/photo/2/
- https://github.com/marticliment/UniGetUI
- https://de.libreoffice.org/download/download/
- https://www.thunderbird.net/de/
- https://www.mozilla.org/de/firefox/
- https://www.google.com/intl/de_de/chrome/
- https://www.philips-hue.com/de-de/explore-hue/propositions/entertainment/sync-with-pc
- https://windirstat.net/download.html `winget install -e --id WinDirStat.WinDirStat`
- https://github.com/kee-org/keepassrpc/releases
- https://github.com/xatupal/KeeTheme/releases
- https://download.nextcloud.com/desktop/daily/windows/
- `winget install Nextcloud.Talk.Beta`
- https://docs.drone.io/cli/install/
