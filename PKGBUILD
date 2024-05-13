# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Contributor: Ionut Biru <ibiru@archlinux.org>
# Contributor: Jakub Schmidtke <sjakub@gmail.com>

pkgname=floorp
_pkgname=Floorp
pkgver=11.12.2.r83.gaaba23d
_esrver=115
pkgrel=1
pkgdesc="Firefox fork from Ablaze, a Japanese community"
arch=(x86_64)
license=(MPL GPL LGPL)
url="https://floorp.ablaze.one/"
depends=(gtk3 libxt mime-types dbus-glib ffmpeg nss ttf-font libpulse)
makedepends=(unzip zip diffutils yasm mesa imake inetutils xorg-server-xvfb
             autoconf2.13 rust clang llvm jack nodejs cbindgen nasm
             lld dump_syms wasi-compiler-rt wasi-libc wasi-libc++ wasi-libc++abi
             mercurial breezy python-dulwich rsync)
optdepends=('networkmanager: Location detection via available WiFi networks'
            'libnotify: Notification integration'
            'pulseaudio: Audio support'
            'speech-dispatcher: Text-to-Speech'
            'hunspell-en_US: Spell checking, American English'
            'xdg-desktop-portal: Screensharing with Wayland')
options=(!emptydirs !makeflags !strip !lto !debug)
source=(git+https://github.com/Floorp-Projects/Floorp.git#branch=ESR115
        floorp-projects.unified-l10n-central::git+https://github.com/Floorp-Projects/Unified-l10n-central.git
        floorp-projects.floorp-core::git+https://github.com/Floorp-Projects/Floorp-core.git
        floorp-projects.private-components::git+https://github.com/Floorp-Projects/Floorp-private-components.git
        hg+https://www.rosenauer.org/hg/mozilla#branch=firefox${_esrver}
        https://dev.gentoo.org/~juippis/mozilla/patchsets/firefox-${_esrver}esr-patches-10.tar.xz
        fix_csd_window_buttons.patch
        0002-move-configuration-home-to-XDG_CONFIG_HOME.patch
        bmo1866829-Replace-obsolete-distutils.patch)
sha256sums=('SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'dbd1897cddfa835223a2f8e0810f2505250526fa7005ae4471b56d5a1b16cd75'
            'e08d0bc5b7e562f5de6998060e993eddada96d93105384960207f7bdf2e1ed6e'
            'b66df8ac8e6054577fa11df866cd1c94b2f36b30ba06bbe60df608cde8c2fbf5'
            '6952f93889acb514e3b06e251ea901df88c39b429da9677cd5547d90a8b6c73e')

pkgver() {
  cd "$_pkgname" || exit
  git describe --long --tags --abbrev=7 | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
  mkdir mozbuild
  cd "$_pkgname" || exit

  patch -Np1 -i "$srcdir/0002-move-configuration-home-to-XDG_CONFIG_HOME.patch"
  patch -Np1 -i "${srcdir}/bmo1866829-Replace-obsolete-distutils.patch"

  msg 'Tickle Git Submodule'
  (
    rm -rf "floorp"
    ln -sf "../floorp-projects.floorp-core" "floorp"
    cd "$srcdir/floorp-projects.floorp-core"
    local _submodules=('floorp-projects.unified-l10n-central'::'browser/locales/l10n-central'
                       'floorp-projects.private-components'::'Floorp-private-components')
    for _module in "${_submodules[@]}" ; do
      git submodule init "${_module##*::}"
      git submodule set-url "${_module##*::}" "$srcdir/${_module%%::*}"
      git -c protocol.file.allow=always submodule update "${_module##*::}"
    done
  )

  # clear forced startup pages
  sed -E 's&^\s*pref\("startup\.homepage.*$&&' -i "browser/branding/official/pref/firefox-branding.js"

  msg 'Gentoo patch'
  local gentoo_patch=($(ls $srcdir/firefox-patches/))
  for src in "${gentoo_patch[@]}"; do
    msg "Applying patch $src..."
    patch -Np1 < "$srcdir/firefox-patches/$src"
  done

  msg 'Opensuse Patch'
  # https://github.com/openSUSE/firefox-maintenance/blob/master/firefox/MozillaFirefox.spec
  local suse_patch=(# xulrunner/gecko patches
                    'mozilla-nongnome-proxies.patch'
                    # 'mozilla-kde.patch'
                    'mozilla-ntlm-full-path.patch'
                    'mozilla-aarch64-startup-crash.patch'
                    'mozilla-fix-aarch64-libopus.patch'
                    # 'mozilla-s390-context.patch'
                    # 'mozilla-pgo.patch' # previous patch detected
                    'mozilla-reduce-rust-debuginfo.patch'
                    'mozilla-bmo1504834-part1.patch'
                    'mozilla-bmo1504834-part3.patch'
                    'mozilla-bmo1512162.patch'
                    # 'mozilla-fix-top-level-asm.patch' # broken patch
                    'mozilla-bmo849632.patch'
                    'mozilla-bmo998749.patch'
                    'mozilla-libavcodec58_91.patch'
                    'mozilla-silence-no-return-type.patch'
                    # 'mozilla-bmo531915.patch' # broken patch
                    'one_swizzle_to_rule_them_all.patch'
                    'svg-rendering.patch'
                    'mozilla-partial-revert-1768632.patch'
                    'mozilla-bmo1775202.patch'
                    'mozilla-rust-disable-future-incompat.patch'
                    # Firefox patches
                    # 'firefox-kde.patch'
                    'firefox-branded-icons.patch')
  for src in "${suse_patch[@]}"; do
    msg "Applying patch $src..."
    patch -Np1 -i "${srcdir}/mozilla/${src}"
  done

  # EVENT__SIZEOF_TIME_T does not exist on upstream libevent, see event-config.h.cmake
  sed -i '/CHECK_EVENT_SIZEOF(TIME_T, time_t);/d' ipc/chromium/src/base/message_pump_libevent.cc

  cat >../mozconfig <<END
ac_add_options --enable-application=browser
ac_add_options --with-app-name=floorp
ac_add_options --with-app-basename=Floorp
mk_add_options MOZ_OBJDIR=${PWD@Q}/obj

ac_add_options --prefix=/usr
ac_add_options --enable-hardening
ac_add_options --enable-optimize
ac_add_options --enable-rust-simd
ac_add_options --enable-linker=lld
ac_add_options --disable-elf-hack
ac_add_options --disable-bootstrap
ac_add_options --enable-default-toolkit=cairo-gtk3-wayland
ac_add_options --with-wasi-sysroot=/usr/share/wasi-sysroot
# ac_add_options --without-wasm-sandboxed-libraries

# Branding
ac_add_options --enable-update-channel=nightly
ac_add_options --with-branding=browser/branding/official
ac_add_options --with-distribution-id=org.archlinux
ac_add_options --with-unsigned-addon-scopes=app,system
ac_add_options --allow-addon-sideload

# Floorp environment variables
export MOZ_INCLUDE_SOURCE_INFO=1
export MOZ_APP_REMOTINGNAME=floorp
unset MOZ_REQUIRE_SIGNING
unset MOZ_DATA_REPORTING
unset MOZ_TELEMETRY_REPORTING

# Keys
ac_add_options --with-google-location-service-api-keyfile=${PWD@Q}/floorp/apis/api-google-location-service-key
ac_add_options --with-google-safebrowsing-api-keyfile=${PWD@Q}/floorp/apis/api-google-safe-browsing-key
ac_add_options --with-mozilla-api-keyfile=${PWD@Q}/floorp/apis/api-mozilla-key

# System libraries
ac_add_options --with-system-nspr
ac_add_options --with-system-nss
ac_add_options --with-system-jpeg
ac_add_options --with-system-webp
ac_add_options --with-system-zlib
ac_add_options --with-system-libvpx
ac_add_options --with-system-harfbuzz
ac_add_options --with-system-libevent
ac_add_options --with-system-icu
ac_add_options --enable-system-ffi
ac_add_options --enable-system-av1
ac_add_options --enable-system-pixman

# Features
ac_add_options --enable-av1
ac_add_options --enable-eme=widevine
ac_add_options --enable-jxl
ac_add_options --enable-pulseaudio
ac_add_options --enable-raw
ac_add_options --enable-sandbox
ac_add_options --enable-webrtc
ac_add_options --enable-private-components
ac_add_options --disable-default-browser-agent
ac_add_options --disable-parental-controls
ac_add_options --enable-proxy-bypass-protection
ac_add_options --enable-alsa
ac_add_options --enable-jack
ac_add_options --disable-crashreporter
ac_add_options --disable-updater
ac_add_options --disable-tests

# Debugging
ac_add_options --disable-debug
ac_add_options --disable-debug-symbols
ac_add_options --disable-debug-js-modules
ac_add_options --enable-strip
ac_add_options --enable-install-strip
export STRIP_FLAGS="--strip-debug --strip-unneeded"
END

  # Remove patched rust file checksums
  sed 's/\("files":{\)[^}]*/\1/' -i \
    third_party/rust/*/.cargo-checksum.json
}

build() {
  cd "$_pkgname" || exit

  export MOZ_NOSPAM=1
  export MOZBUILD_STATE_PATH="$srcdir/mozbuild"
  export MOZ_ENABLE_FULL_SYMBOLS=0
  export MOZ_BUILD_DATE="$(date -u${SOURCE_DATE_EPOCH:+d @$SOURCE_DATE_EPOCH} +%Y%m%d%H%M%S)"
  export MACH_BUILD_PYTHON_NATIVE_PACKAGE_SOURCE=pip
  export XDG_RUNTIME_DIR="${XDG_RUNTIME_DIR:-$srcdir/xdg}"
  export LIBGL_ALWAYS_SOFTWARE=true
  install -dm700 "${XDG_RUNTIME_DIR:?}"

  # LTO needs more open files
  ulimit -n 4096

  # Do 3-tier PGO
  msg "Building instrumented browser..."
  cat >.mozconfig ../mozconfig - <<END
ac_add_options --enable-profile-generate=cross
export MOZ_ENABLE_FULL_SYMBOLS=1
END
  ./mach build

  msg "Profiling instrumented browser..."
  ./mach package
  LLVM_PROFDATA=llvm-profdata \
    JARLOG_FILE="$PWD/jarlog" \
    xvfb-run -s "-screen 0 1920x1080x24 -nolisten local" \
    ./mach python build/pgo/profileserver.py

  stat -c "Profile data found (%s bytes)" merged.profdata
  stat -c "Jar log found (%s bytes)" jarlog


  msg "Removing instrumented browser..."
  ./mach clobber

  msg "Building optimized browser..."
  cat >.mozconfig ../mozconfig - <<END
ac_add_options --enable-lto=cross
ac_add_options --enable-profile-use=cross
ac_add_options --with-pgo-profile-path=${PWD@Q}/merged.profdata
ac_add_options --with-pgo-jarlog=${PWD@Q}/jarlog
# L10n
ac_add_options --with-l10n-base=${PWD@Q}/floorp/browser/locales/l10n-central
END
  ./mach build
  
  msg 'Building locales'
  ./mach package
  export MOZ_CHROME_MULTILOCALE="en-US ja zh-TW"
  for AB_CD in $MOZ_CHROME_MULTILOCALE; do
    msg "Building locales $AB_CD"
    ./mach build chrome-$AB_CD
  done
}

package() {
  cd "$_pkgname" || exit
  DESTDIR="$pkgdir" ./mach install

  local pref="$pkgdir/usr/lib/floorp/browser/defaults/preferences"
  install -Dvm644 /dev/stdin "$pref/vendor.js" <<END
// Use system-provided dictionaries
pref("spellchecker.dictionary_path", "/usr/share/hunspell");

// Don't disable extensions in the application directory
pref("extensions.autoDisableScopes", 11);

// Enable GNOME Shell search provider
pref("browser.gnome-search-provider.enabled", true);

// Enable JPEG XL images
pref("image.jxl.enabled", true);

// Prevent about:config warning
pref("browser.aboutConfig.showWarning", false);

// Prevent telemetry notification
pref("services.settings.main.search-telemetry-v2.last_check", $(date +%s));
END

  install -Dvm644 /dev/stdin "$pref/gentoo.js" <<END
/* gentoo ebuild */
pref("gfx.x11-egl.force-enabled", false);
sticky_pref("gfx.font_rendering.graphite.enabled", true);
pref("media.gmp-gmpopenh264.autoupdate", false);
pref("media.gmp-widevinecdm.autoupdate", false);

/* gentoo-default-prefs.js */
pref("general.smoothScroll",               true);
pref("general.autoScroll",                 false);
pref("browser.urlbar.hideGoButton",        true);
pref("accessibility.typeaheadfind",        true);
pref("browser.shell.checkDefaultBrowser",  false);
pref("browser.EULA.override",              true);
pref("general.useragent.locale",           "chrome://global/locale/intl.properties");
pref("intl.locale.requested",              "");
/* Disable DoH by default */
pref("network.trr.mode",                   5);
/* Disable use of Mozilla Normandy service by default */
pref("app.normandy.enabled",               false);
END

  install -Dvm644 /dev/stdin "$pref/media_decoding.js" <<END
/* gentoo-hwaccel-prefs.js-r2 */
/* Force hardware accelerated rendering due to USE=hwaccel */
pref("gfx.webrender.all",                  true);
pref("layers.acceleration.force-enabled",  true);
sticky_pref("media.hardware-video-decoding.enabled", true);
pref("webgl.force-enabled",                true);

// hardware acceleration
sticky_pref("media.ffmpeg.vaapi.enabled", true);
sticky_pref("media.hardware-video-decoding.force-enabled", true);
sticky_pref("media.navigator.mediadatadecoder_vpx_enabled", true);
END

  install -Dvm644 /dev/stdin "$pref/kde.js" <<END
// KDE.js
pref("browser.preferences.instantApply", false);
pref("browser.backspace_action", 0);

// Enable XDG Desktop Portal by defualt
pref("widget.use-xdg-desktop-portal.file-picker", 1);
pref("widget.use-xdg-desktop-portal.location", 1);
pref("widget.use-xdg-desktop-portal.mime-handler", 1);
pref("widget.use-xdg-desktop-portal.open-uri", 1);
pref("widget.use-xdg-desktop-portal.settings", 1);
END

  install -Dvm644 /dev/stdin "$pref/disable_typeahead.js" <<END
sticky_pref("accessibility.typeaheadfind", false);
sticky_pref("accessibility.typeaheadfind.autostart", false);
sticky_pref("accessibility.typeaheadfind.manual", false);
sticky_pref("accessibility.typeaheadfind.flashBar", 0);
END

  local distini="$pkgdir/usr/lib/floorp/distribution/distribution.ini"
  install -Dvm644 /dev/stdin "$distini" <<END
[Global]
id=archlinux
version=1.0

[Preferences]
app.distributor=archlinux
app.distributor.channel=floorp
END

  local i theme=official
  for i in 16 22 24 32 48 64 128 256; do
    install -Dvm644 browser/branding/$theme/default$i.png \
      "$pkgdir/usr/share/icons/hicolor/${i}x${i}/apps/floorp.png"
  done
  install -Dvm644 browser/branding/$theme/content/about-logo.png \
    "$pkgdir/usr/share/icons/hicolor/192x192/apps/floorp.png"
  install -Dvm644 browser/branding/$theme/content/about-logo@2x.png \
    "$pkgdir/usr/share/icons/hicolor/384x384/apps/floorp.png"
  install -Dvm644 browser/branding/$theme/content/about-logo.svg \
    "$pkgdir/usr/share/icons/hicolor/scalable/apps/floorp.svg"

  install -Dvm644 "$srcdir/$_pkgname/.github/floorp-debian.desktop" \
    "$pkgdir/usr/share/applications/floorp.desktop"

  # Install a wrapper to avoid confusion about binary path
  install -Dvm755 /dev/stdin "$pkgdir/usr/bin/floorp" <<END
#!/bin/sh
exec /usr/lib/floorp/floorp "\$@"
END

  # Replace duplicate binary with wrapper
  # https://bugzilla.mozilla.org/show_bug.cgi?id=658850
  ln -srfv "$pkgdir/usr/bin/floorp" "$pkgdir/usr/lib/floorp/floorp-bin"

  # Use system certificates
  local nssckbi="$pkgdir/usr/lib/floorp/libnssckbi.so"
  if [[ -e $nssckbi ]]; then
    ln -srfv "$pkgdir/usr/lib/libnssckbi.so" "$nssckbi"
  fi
}

# vim:set sw=2 et:
