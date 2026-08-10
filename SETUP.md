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
- `git config --global gpg.program "C:\Program Files\GnuPG\bin\gpg.exe"`

#### Useful git commands

- all contributors: `git log --format='%aN <%aE>' | LC_ALL=C.UTF-8 sort -uf`
- execution right on file: `git update-index --chmod=+x npm-post-build.sh`
- cleanup checkout: `git remote prune origin` plus `git gc`
- delete local branches gone on the remote: `git prune-gone` (global alias, see [Git aliases](#git-aliases))
- push tag: `git push releases tag <tag_name>` with `releases` being the nc-releases org/repo

### Git aliases

Global aliases live in `~/.gitconfig`, so they work in every repo and every shell
(PowerShell, cmd, git-bash) without any shell profile setup. `!` aliases run through
git's bundled sh, so Unix tools like `awk` and `xargs` are available even when
invoked from PowerShell.

`git prune-gone` — prunes stale remote-tracking refs, then deletes all local
branches whose upstream is `[gone]` (branch deleted on the remote, e.g. after a
merged PR). Uses `git branch -d`, so only branches git considers merged are
deleted — squash-merged branches must be removed manually with `git branch -D`.

Added via (git-bash):

```bash
git config --global alias.prune-gone '!git fetch --prune && git for-each-ref --format '\''%(refname:short) %(upstream:track)'\'' refs/heads | awk '\''$2 == "[gone]" {print $1}'\'' | xargs -r git branch -d'
```

Resulting entry in `~/.gitconfig` (can also be pasted there directly instead):

```ini
[alias]
    prune-gone = "!git fetch --prune && git for-each-ref --format '%(refname:short) %(upstream:track)' refs/heads | awk '$2 == \"[gone]\" {print $1}' | xargs -r git branch -d"
```

### Python

- https://www.python.org/downloads/windows/
- install reuse `pip install reuse`
- update reuse `pip install --upgrade reuse`
- install zizmor (GH action checker) `pip install zizmor` (https://docs.zizmor.sh/quickstart/)
- update zizmor `pip install --upgrade zizmor`
- update pip `python.exe -m pip install --upgrade pip`

*Run the above commands after initial app installation setup via UniGet*

### Android / Java

- https://aws.amazon.com/de/corretto/
- https://developer.android.com/studio?hl=en

### Useful gradle commands

- `gradlew --write-verification-metadata sha256,pgp help --export-keys`
- `gradle --console=plain --no-build-cache --dependency-verification lenient -q --write-verification-metadata sha256,pgp dependencies --export-keys`
- `gradlew spotbugsDebug --console=plain --dependency-verification lenient -q --write-verification-metadata sha256,pgp dependencies --export-keys`

### Useful drone commands

- `drone sign nextcloud/android --save`
- `drone sign nextcloud/talk-android --save`

### Useful web dev commands

- `git clone https://github.com/juliusknorr/nextcloud-docker-dev`
- `cd nextcloud-docker-dev`
- `./bootstrap.sh`

---

- `sudo apt-get install -y php8.3-xml`
- `sudo apt-get install -y php8.3-gd`
- `sudo apt-get install -y php8.3-ext-gd`
- `sudo apt install composer`
- `sudo apt install npm`
- `npm install`
- `npm run dev`
- `composer install`
- `composer install --no-dev`
- `docker compose down -v`
- `docker compose up -d nextcloud`
- `cd ../../../..`
- `cd workspace/server/apps-extra/`
- `sudo apt-get update && sudo apt-get upgrade`
- `docker compose exec nextcloud occ background-job:list --class 'OCA\ShareReview\BackgroundJob\GenerateReportJob'`

Fix docker checkout for all branches accessible:

- `git config remote.origin.fetch '+refs/heads/*:refs/remotes/origin/*'`
- `git fetch --all`
- `git submodule update --recursive --remote`

Activte debug:

- `docker compose exec --user www-data nextcloud php occ config:system:set loglevel --value 0 --type integer`

### PHP / Web

- https://windows.php.net/download/
- https://getcomposer.org/download/
- https://github.com/coreybutler/nvm-windows / https://learn.microsoft.com/en-us/windows/dev-environment/javascript/nodejs-on-windows
- https://gnuwin32.sourceforge.net/packages/make.htm + $PATH

### General dev tools

- https://getgreenshot.org/downloads/
- https://winmerge.org/downloads/
- https://github.com/suzuki-shunsuke/pinact (GH action pinning)
- https://docs.zizmor.sh/quickstart/ (GH action scanning/fixing)
- Peek Screen recoder (via app store)
- `winget install --id Microsoft.DevHome -e` for https://github.com/microsoft/devhome
- Update winget, get latest `.msixbundle` from https://github.com/microsoft/winget-cli/releases/

### Composer, CycloneDX & parlay CLIs

Command-line dependency tooling: `composer` (PHP), `cyclonedx-npm`,
the `cyclonedx` .NET CLI, and `parlay`. Prereqs covered above: PHP
(winget `PHP.PHP.8.5`), Node/npm, `jq` (`D:\Tools\jq`). No .NET SDK and no
Go toolchain are required — the `cyclonedx` CLI and `parlay` ship prebuilt
native binaries. Install layout mirrors `jq`: one folder per tool under
`D:\Tools\`. Verified 2026-07-10 with composer 2.10.2, cyclonedx-php-composer
6.2.0, cyclonedx-npm 6.0.0, cyclonedx CLI 0.32.0, parlay 0.11.0.

1. PHP config (Composer needs the `openssl`, `zip`, `curl`, `mbstring`,
   `fileinfo` extensions; the winget PHP loads no `php.ini` by default). Create
   a standalone config dir and point PHP at it with `PHPRC`:

   ```powershell
   $phpDir = Split-Path (Get-Command php).Source
   $cfg = 'D:\Tools\php-config'; New-Item -ItemType Directory -Force $cfg | Out-Null
   @"
   extension_dir = "$phpDir\ext"
   extension=openssl
   extension=zip
   extension=curl
   extension=mbstring
   extension=fileinfo
   memory_limit = -1
   "@ | Set-Content "$cfg\php.ini" -Encoding utf8
   # verify: php -m should now list openssl, zip, curl, mbstring, fileinfo
   ```

2. Composer (+ optional CycloneDX PHP plugin; the plugin is blocked until
   allow-listed):

   ```powershell
   $env:PHPRC = 'D:\Tools\php-config'
   $dst = 'D:\Tools\composer'; New-Item -ItemType Directory -Force $dst | Out-Null
   iwr https://getcomposer.org/installer -OutFile "$dst\composer-setup.php"
   php "$dst\composer-setup.php" --install-dir="$dst" --filename=composer.phar
   Remove-Item "$dst\composer-setup.php"
   php "$dst\composer.phar" global config --no-plugins allow-plugins.cyclonedx/cyclonedx-php-composer true
   php "$dst\composer.phar" global require cyclonedx/cyclonedx-php-composer
   ```

   To call `composer` from git-bash, add a wrapper (no extension) it can exec;
   add a `composer.bat` for cmd/pwsh:

   ```powershell
   "#!/usr/bin/env bash`nexport PHPRC=`"D:/Tools/php-config`"`nexec php `"D:/Tools/composer/composer.phar`" `"`$@`"" `
     -replace "`r`n","`n" | Set-Content -NoNewline 'D:\Tools\composer\composer' -Encoding ascii
   "@echo off`r`nphp `"%~dp0composer.phar`" %*" | Set-Content 'D:\Tools\composer\composer.bat' -Encoding ascii
   ```

3. cyclonedx-npm (global npm package): `npm install -g @cyclonedx/cyclonedx-npm`

4. cyclonedx CLI — native binary, no .NET SDK:

   ```powershell
   $dst = 'D:\Tools\cyclonedx'; New-Item -ItemType Directory -Force $dst | Out-Null
   $a = (irm https://api.github.com/repos/CycloneDX/cyclonedx-cli/releases/latest).assets |
        Where-Object name -eq 'cyclonedx-win-x64.exe'
   iwr $a.browser_download_url -OutFile "$dst\cyclonedx.exe"
   ```

5. parlay — native Go binary, no Go toolchain:

   ```powershell
   $dst = 'D:\Tools\parlay'; New-Item -ItemType Directory -Force $dst | Out-Null
   $a = (irm https://api.github.com/repos/snyk/parlay/releases/latest).assets |
        Where-Object { $_.name -match 'Windows_x86_64\.zip$' } | Select-Object -First 1
   iwr $a.browser_download_url -OutFile "$dst\parlay.zip"
   Expand-Archive "$dst\parlay.zip" $dst -Force
   ```

6. Make them permanent — set `PHPRC` and add the tool dirs to the User `PATH`
   (idempotent; no admin needed), **then open a new terminal** — the change only
   applies to shells started afterwards. git-bash inherits the Windows User
   `PATH`, so a fresh git-bash picks the tools up too.

   ```powershell
   [Environment]::SetEnvironmentVariable('PHPRC','D:\Tools\php-config','User')
   $p = [Environment]::GetEnvironmentVariable('Path','User').TrimEnd(';')
   foreach ($d in 'D:\Tools\composer','D:\Tools\cyclonedx','D:\Tools\parlay') {
     if ($p.Split(';') -notcontains $d) { $p += ";$d" }
   }
   [Environment]::SetEnvironmentVariable('Path',$p,'User')
   ```

   Verify in the new shell: `composer --version; cyclonedx --version; cyclonedx-npm --version; parlay --version; jq --version`

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
