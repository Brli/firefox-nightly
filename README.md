
# Upstream of files

## [Arch Linux Official Repo](https://github.com/archlinux/svntogit-packages/tree/packages/firefox/trunk)

You can find the code [here](https://github.com/archlinux/svntogit-packages/tree/packages/firefox/trunk)

`PKGBUILD` originated from official's, is later diverged into my own.

## [firefox-kde-opensuse-kde](https://gitlab.com/Thaodan/firefox-kde-opensuse)
```
fix_csd_window_buttons.patch
```

## [Firefox l10n](https://github.com/mozilla-l10n/firefox-l10n.git)

## [OpenSuSE](https://github.com/openSUSE/firefox-maintenance.git)

## [Gentoo](https://gitweb.gentoo.org/repo/gentoo.git/tree/www-client/firefox)

## [Ubuntu](https://code.launchpad.net/~mozillateam/firefox/firefox-trunk.head)

`firefox.desktop` originated from Ubuntu's `firefox.desktop.in`, edit to remove conditional clause and variables.

Other than above mentioned files, which has their own licenses and permissions, are licensed GPL v3.0.

# Building Process Investigation

## The Official way

- Genrate instrumented browser artifcial on routine (the 1st tier)

- Do PGO (the 2nd tier)

- Download artifacts and apply Floorp (noraneko) changes

- Compile the target browser (3-tier)

## The PKGBUILD

- Make Floorp branding blobs in place

- Apply Patch from Gentoo and OpenSuSE

- Apply noraneko upstream series patches, apply deno task for "before mach"

- Add Floorp specific options

- Build the somehow-vanilla instrumented browser (1-tier)

- Do PGO (2-tier)

- Apply Floorp (noraneko) package patches, apply deno task for "after mach"

- Build the Floorp-branded browser with PGO (3-tier)
