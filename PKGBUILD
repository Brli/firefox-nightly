# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Contributor: Ionut Biru <ibiru@archlinux.org>
# Contributor: Jakub Schmidtke <sjakub@gmail.com>

pkgname=floorp
pkgver=11.9.0
_esrver=115
pkgrel=1
pkgdesc="Firefox fork from Ablaze, a Japanese community"
arch=(x86_64)
license=(MPL GPL LGPL)
url="https://floorp.ablaze.one/"
depends=(gtk3 libxt mime-types dbus-glib ffmpeg nss ttf-font libpulse)
makedepends=(unzip zip diffutils yasm mesa imake inetutils xwayland-run weston
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
source=(https://github.com/Floorp-Projects/Floorp/archive/refs/tags/v${pkgver}.zip
        git+https://github.com/Floorp-Projects/Unified-l10n-central.git
        git+https://github.com/Floorp-Projects/Floorp-core.git#commit=cefaa9a
        hg+https://www.rosenauer.org/hg/mozilla#branch=firefox${_esrver}
        librewolf-patch::git+https://gitlab.com/librewolf-community/browser/source.git#tag=${_esrver}.0.2-2
        https://dev.gentoo.org/~juippis/mozilla/patchsets/firefox-${_esrver}esr-patches-08.tar.xz
        mozilla-kde.patch unity-menubar.patch
        0002-move-configuration-home-to-XDG_CONFIG_HOME.patch
        fix_csd_window_buttons.patch)
sha256sums=('176782b67c8c0e23d7a0ba55600f86625392bd91ca6154ab12e0c778f91e2266'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'db6e1e87a1b5f857be93a5a44303c5e4e36f94bf05036c8f99730457448f99db'
            'fb59151ae0bee183251d560dc3b04a47bde1b9aab9ee2d9fa251a15337d1eb11'
            '796d76d079e4e6e106146ceff17b603cfa1afadf4a06114681e734c8f9e8879f'
            'd00779111b7cd51213caa7358582507b964bba5c849d0a6d966cecd28b5d1ef3'
            'e08d0bc5b7e562f5de6998060e993eddada96d93105384960207f7bdf2e1ed6e')

prepare() {
  mkdir mozbuild
  mv -f mozilla-kde.patch unity-menubar.patch -t "${srcdir}/librewolf-patch/patches/unity_kde/"
  cd "Floorp-${pkgver}" || exit

  # patch for xdg home
  patch -Np1 -i "$srcdir/0002-move-configuration-home-to-XDG_CONFIG_HOME.patch"

  msg 'Tickle Git Submodule'
  cp -r "$srcdir/Floorp-core/"* floorp/
  cp -r "$srcdir/Unified-l10n-central/"* "floorp/browser/locales/l10n-central/"

  msg 'Gentoo patch'
  local gentoo_patch=($(ls $srcdir/firefox-patches/))
  for src in "${gentoo_patch[@]}"; do
    msg "Applying patch $src..."
    patch -Np1 < "$srcdir/firefox-patches/$src"
  done

  msg 'Opensuse Patch'
  # https://github.com/openSUSE/firefox-maintenance/blob/master/firefox/MozillaFirefox.spec
  local suse_patch=('mozilla-nongnome-proxies.patch'
                    # 'mozilla-kde.patch'
                    'mozilla-ntlm-full-path.patch'
                    'mozilla-aarch64-startup-crash.patch'
                    'mozilla-fix-aarch64-libopus.patch'
                    # 'mozilla-s390-context.patch'
                    # 'mozilla-pgo.patch' # previous patch detected
                    'mozilla-reduce-rust-debuginfo.patch'
                    # 'mozilla-bmo1005535.patch'
                    # 'mozilla-bmo1568145.patch'
                    # 'mozilla-bmo1504834-part1.patch'
                    # 'mozilla-bmo1504834-part3.patch'
                    'mozilla-bmo1512162.patch'
                    # 'mozilla-fix-top-level-asm.patch' # broken patch
                    'mozilla-bmo849632.patch'
                    'mozilla-bmo998749.patch'
                    # 'mozilla-s390x-skia-gradient.patch'
                    # 'mozilla-libavcodec58_91.patch' # We don't fallback-support ffmpeg
                    # 'mozilla-silence-no-return-type.patch'
                    # 'mozilla-bmo531915.patch' # broken patch
                    'one_swizzle_to_rule_them_all.patch'
                    'svg-rendering.patch'
                    # 'firefox-branded-icons.patch'
                    # 'firefox-kde.patch'
                    'mozilla-rust-disable-future-incompat.patch')
  for src in "${suse_patch[@]}"; do
    msg "Applying patch $src..."
    patch -Np1 -i "${srcdir}/mozilla/${src}"
  done


  msg 'librewolf patch'
  local librewolf_patch=(sed-patches/stop-undesired-requests.patch
                         ui-patches/remove-snippets-from-home.patch
                         unity_kde/mozilla-kde.patch
                         unity_kde/firefox-kde.patch
                         unity_kde/unity-menubar.patch)
  for src in "${librewolf_patch[@]}"; do
    msg "Applying patch $src..."
    patch -Np1 -i "${srcdir}/librewolf-patch/patches/$src"
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
ac_add_options --enable-optimize='-O3'
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
ac_add_options --disable-default-browser-agent
ac_add_options --disable-parental-controls
ac_add_options --enable-proxy-bypass-protection
ac_add_options --disable-alsa
ac_add_options --disable-jack
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
  cd "Floorp-${pkgver}" || exit

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
END
  ./mach build

  msg "Profiling instrumented browser..."
  ./mach package
  LLVM_PROFDATA=llvm-profdata \
    JARLOG_FILE="$PWD/jarlog" \
    wlheadless-run -c weston --width=1920 --height=1080 \
    -- ./mach python build/pgo/profileserver.py

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
ac_add_options --with-l10n-base=${srcdir}/Unified-l10n-central
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
  cd "Floorp-${pkgver}" || exit
  DESTDIR="$pkgdir" ./mach install

  local pref="$pkgdir/usr/lib/floorp/browser/defaults/preferences"
  install -Dvm644 /dev/stdin "$pref/vendor.js" <<END
// Use system-provided dictionaries
pref("spellchecker.dictionary_path", "/usr/share/hunspell");

// Don't disable extensions in the application directory
pref("extensions.autoDisableScopes", 11);
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

  install -Dvm644 "$srcdir/Floorp-$pkgver/.github/floorp-debian.desktop" \
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
