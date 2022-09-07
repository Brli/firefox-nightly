# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Contributor: Ionut Biru <ibiru@archlinux.org>
# Contributor: Jakub Schmidtke <sjakub@gmail.com>

pkgname=firefox
pkgver=106.0a1.20220907.49a271c42001
pkgrel=1
pkgdesc="Standalone web browser from mozilla.org"
arch=(x86_64)
license=(MPL GPL LGPL)
url="https://www.mozilla.org/firefox/"
depends=(gtk3 libxt mime-types dbus-glib ffmpeg nss ttf-font libpulse)
makedepends=(unzip zip diffutils yasm mesa imake inetutils xorg-server-xvfb
             autoconf2.13 rust clang llvm jack nodejs cbindgen nasm
             python-setuptools python-zstandard lld dump_syms
             mercurial breezy python-dulwich rsync)
optdepends=('networkmanager: Location detection via available WiFi networks'
            'libnotify: Notification integration'
            'pulseaudio: Audio support'
            'speech-dispatcher: Text-to-Speech'
            'hunspell-en_US: Spell checking, American English'
            'xdg-desktop-portal: Screensharing with Wayland')
#conflicts=(firefox-i18n-zh-tw)
#replaces=(firefox-i18n-zh-tw)
options=(!emptydirs !makeflags !strip !lto !debug)
_moz_revision=49a271c42001f7bc5b9efadfc3e0b66dc0b67777
source=(hg+https://hg.mozilla.org/mozilla-central#revision=$_moz_revision
        hg+https://hg.mozilla.org/l10n-central/zh-TW
        git+https://github.com/openSUSE/firefox-maintenance.git
        librewolf-patch::git+https://gitlab.com/librewolf-community/browser/source.git
        git+https://github.com/Brli/firefox-trunk.git
        https://dev.gentoo.org/~juippis/mozilla/patchsets/firefox-104-patches-02j.tar.xz
        0028-bmo-1559213-fix-system-av1-libs.patch
        fix_csd_window_buttons.patch zstandard-0.18.0.diff
        firefox.desktop identity-icons-brand.svg)
sha256sums=('SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'a5514822831057b694ccc615fceae1119756542f335ec846cd589cc34682ba62'
            'a5ccfaf882eb8201d5253885a3b3a36ff4fb7491274483eef85efa8c0fe21de6'
            'e08d0bc5b7e562f5de6998060e993eddada96d93105384960207f7bdf2e1ed6e'
            'a6857ad2f2e2091c6c4fdcde21a59fbeb0138914c0e126df64b50a5af5ff63be'
            'ca27cd74a8391c0d5580d2068696309e4086d05d9cd0bd5c42cf5e4e9fa4d472'
            'a9b8b4a0a1f4a7b4af77d5fc70c2686d624038909263c795ecc81e0aec7711e9')
validpgpkeys=('14F26682D0916CDD81E37B6D61B7B526D98F0353') # Mozilla Software Releases <release@mozilla.com>

# Google API keys (see http://www.chromium.org/developers/how-tos/api-keys)
# Note: These are for Arch Linux use ONLY. For your own distribution, please
# get your own set of keys. Feel free to contact foutrelis@archlinux.org for
# more information.
_google_api_key=AIzaSyDwr302FpOSkGRpLlUpPThNTDPbXcIn_FM

# Mozilla API keys (see https://location.services.mozilla.com/api)
# Note: These are for Arch Linux use ONLY. For your own distribution, please
# get your own set of keys. Feel free to contact heftig@archlinux.org for
# more information.
_mozilla_api_key=e05d56db0a694edc8b5aaebda3f2db6a

pkgver() {
  cd mozilla-central
  _pkgver=$(cat browser/config/version.txt)
  printf "${_pkgver}.%s.%s" "$(date -u +%Y%m%d)" "$(hg identify -i)" | sed 's/\+//g'
}

prepare() {
  mkdir mozbuild
  mv zh-TW mozbuild/
  cd mozilla-central

  # Unbreak build with python-zstandard 0.18.0
  patch -Np1 -i ../zstandard-0.18.0.diff

  msg 'Gentoo patch'
  # local gentoo_patch=($(ls $srcdir/firefox-patches/))
  local gentoo_patch=('0004-bmo-847568-Support-system-harfbuzz.patch'
                      '0005-bmo-847568-Support-system-graphite2.patch'
                      '0006-bmo-1559213-Support-system-av1.patch'
                      '0008-bmo-1516803-Fix-building-sandbox.patch'
                      '0018-bmo-1516081-Disable-watchdog-during-PGO-builds.patch'
                      '0019-bmo-1516803-force-one-LTO-partition-for-sandbox-when.patch'
                      '0021-libaom-Use-NEON_FLAGS-instead-of-VPX_ASFLAGS-for-lib.patch'
                      '0022-build-Disable-Werror.patch'
                      '0023-LTO-Only-enable-LTO-for-Rust-when-complete-build-use.patch'
                      '0024-Enable-FLAC-on-platforms-without-ffvpx-via-ffmpeg.patch'
                      '0025-bmo-1670333-OpenH264-Fix-decoding-if-it-starts-on-no.patch'
                      '0026-bmo-1663844-OpenH264-Allow-using-OpenH264-GMP-decode.patch'
                      '0027-bgo-816975-fix-build-on-x86.patch'
                      # '0028-bmo-1559213-fix-system-av1-libs.patch' # edited
                      '0029-bmo-1196777-Set-GDK_FOCUS_CHANGE_MASK.patch')
                      # '0032-bmo-1773336-disable_audio_thread_priority_default_features.patch' # upstreamed
                      # '0032-p05-bmo-1776724-build-wayland-only-D150485.patch' # upstreamed
                      # '0036-vaapi-fixes.patch')
  patch -Np1 -i "$srcdir/0028-bmo-1559213-fix-system-av1-libs.patch"
  for src in "${gentoo_patch[@]}"; do
    msg "Applying patch $src..."
    patch -Np1 < "$srcdir/firefox-patches/$src"
  done

  msg 'opensuse patch'
  # https://github.com/openSUSE/firefox-maintenance/blob/master/firefox/MozillaFirefox.spec
  local suse_patch=('mozilla-nongnome-proxies.patch'
                    # 'mozilla-kde.patch'
                    'mozilla-ntlm-full-path.patch'
                    # 'mozilla-aarch64-startup-crash.patch'
                    # 'mozilla-fix-aarch64-libopus.patch'
                    # 'mozilla-s390-context.patch'
                    # 'mozilla-pgo.patch'
                    'mozilla-reduce-rust-debuginfo.patch'
                    'mozilla-bmo1005535.patch'
                    'mozilla-bmo1568145.patch'
                    'mozilla-bmo1504834-part1.patch'
                    'mozilla-bmo1504834-part3.patch'
                    'mozilla-bmo1512162.patch'
                    # 'mozilla-fix-top-level-asm.patch'
                    'mozilla-bmo849632.patch'
                    'mozilla-bmo998749.patch'
                    # 'mozilla-s390x-skia-gradient.patch'
                    # 'mozilla-libavcodec58_91.patch'
                    # 'mozilla-silence-no-return-type.patch'
                    # 'mozilla-bmo531915.patch'
                    'one_swizzle_to_rule_them_all.patch'
                    'svg-rendering.patch'
                    # 'firefox-kde.patch'
                    'firefox-branded-icons.patch')
  for src in "${suse_patch[@]}"; do
    msg "Applying patch $src..."
    patch -Np1 -i "${srcdir}/firefox-maintenance/firefox/$src"
  done

  msg 'librewolf patch'
  local librewolf_patch=('faster-package-multi-locale.patch'
                         'unity-menubar.patch')
  for src in "${librewolf_patch[@]}"; do
    msg "Applying patch $src..."
    patch -Np1 -i "${srcdir}/librewolf-patch/patches/$src"
  done

  msg 'ubuntu patch'
  local ubuntu_patch=('dont-checkout-locales.patch'
                      'use-system-icupkg.patch')
  for src in "${ubuntu_patch[@]}"; do
   msg "Applying patch $src..."
   patch -Np1 -i "${srcdir}/firefox-trunk/debian/patches/$src"
  done

  # EVENT__SIZEOF_TIME_T does not exist on upstream libevent, see event-config.h.cmake
  sed -i '/CHECK_EVENT_SIZEOF(TIME_T, time_t);/d' ipc/chromium/src/base/message_pump_libevent.cc
  
  echo -n "$_google_api_key" >google-api-key
  echo -n "$_mozilla_api_key" >mozilla-api-key

  cat >../mozconfig <<END
ac_add_options --enable-application=browser
ac_add_options --with-app-name=$pkgname
mk_add_options MOZ_OBJDIR=${PWD@Q}/obj

ac_add_options --prefix=/usr
ac_add_options --enable-release
ac_add_options --enable-hardening
ac_add_options --enable-optimize
ac_add_options --enable-rust-simd
ac_add_options --enable-linker=lld
ac_add_options --disable-elf-hack
ac_add_options --disable-bootstrap
# ac_add_options --with-wasi-sysroot=/usr/share/wasi-sysroot
ac_add_options --without-wasm-sandboxed-libraries

# Branding
ac_add_options --enable-official-branding
ac_add_options --enable-update-channel=release
ac_add_options --with-distribution-id=org.archlinux
ac_add_options --with-unsigned-addon-scopes=app,system
ac_add_options --allow-addon-sideload
export MOZILLA_OFFICIAL=1
export MOZ_APP_REMOTINGNAME=${pkgname//-/}
export MOZ_REQUIRE_SIGNING=1
unset MOZ_TELEMETRY_REPORTING

# Keys
ac_add_options --with-google-location-service-api-keyfile=${PWD@Q}/google-api-key
ac_add_options --with-google-safebrowsing-api-keyfile=${PWD@Q}/google-api-key
ac_add_options --with-mozilla-api-keyfile=${PWD@Q}/mozilla-api-key

# System libraries
ac_add_options --with-system-nspr
ac_add_options --with-system-nss
ac_add_options --with-system-jpeg
ac_add_options --with-system-webp
ac_add_options --with-system-zlib
ac_add_options --with-system-libvpx
ac_add_options --with-system-harfbuzz
ac_add_options --with-system-graphite2
ac_add_options --with-system-libevent
ac_add_options --with-system-icu
ac_add_options --enable-system-ffi
ac_add_options --enable-system-av1
ac_add_options --enable-system-pixman

# Features
ac_add_options --enable-default-toolkit=cairo-gtk3-wayland
ac_add_options --disable-alsa
ac_add_options --disable-jack
ac_add_options --disable-necko-wifi
ac_add_options --disable-crashreporter
ac_add_options --disable-updater
ac_add_options --disable-tests
END

  # Fake mozilla version
  echo '104.0.2' > config/milestone.txt

  # Desktop file
  sed "/^%%/d;/@MOZ_DISPLAY_NAME@/d;s,@MOZ_APP_NAME@,$pkgname,g" -i "${srcdir}/firefox.desktop"

  # remove checksum for files patched
  sed 's/\("files":{\)[^}]*/\1/' -i \
    third_party/rust/{audioipc-client,audioipc,audioipc-server}/.cargo-checksum.json
}

build() {
  cd mozilla-central

  export MOZ_NOSPAM=1
  export MOZBUILD_STATE_PATH="$srcdir/mozbuild"
  export MOZ_ENABLE_FULL_SYMBOLS=0
  export MACH_BUILD_PYTHON_NATIVE_PACKAGE_SOURCE=system

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
    xvfb-run -s "-screen 0 1920x1080x24 -nolisten local" \
    ./mach python build/pgo/profileserver.py

  stat -c "Profile data found (%s bytes)" merged.profdata
  test -s merged.profdata

  stat -c "Jar log found (%s bytes)" jarlog
  test -s jarlog

  msg "Removing instrumented browser..."
  ./mach clobber

  msg "Building optimized browser..."
  cat >.mozconfig ../mozconfig - <<END
ac_add_options --enable-lto=cross
ac_add_options --enable-profile-use=cross
ac_add_options --with-pgo-profile-path=${PWD@Q}/merged.profdata
ac_add_options --with-pgo-jarlog=${PWD@Q}/jarlog

# L10n
ac_add_options --with-l10n-base=$srcdir/mozbuild
END
  ./mach build
  
  msg 'Building locales'
  ./mach package
  export MOZ_CHROME_MULTILOCALE="zh-TW"
  for AB_CD in $MOZ_CHROME_MULTILOCALE; do
    msg "Building locales $AB_CD"
    ./mach build chrome-$AB_CD
  done
}

package() {
  cd mozilla-central
  DESTDIR="$pkgdir" ./mach install

  local pref="$pkgdir/usr/lib/$pkgname/browser/defaults/preferences"
  install -Dvm644 /dev/stdin "$pref/vendor.js" <<END
// Use system-provided dictionaries
pref("spellchecker.dictionary_path", "/usr/share/hunspell");

// Don't disable extensions in the application directory
pref("extensions.autoDisableScopes", 11);

// UA override
// pref("general.useragent.override", "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.5060.134 Safari/537.36 Edg/103.0.1264.71");
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
sticky_pref("media.ffvpx.enabled", false);
sticky_pref("media.hardware-video-decoding.force-enabled", true);
sticky_pref("media.navigator.mediadatadecoder_vpx_enabled", true);
sticky_pref("media.rdd-ffvpx.enabled", false);
sticky_pref("media.rdd-vpx.enabled", false);
sticky_pref("media.rdd-ffmpeg.enabled", false);
END

  install -Dvm644 /dev/stdin "$pref/kde.js" <<END
// KDE.js
// pref("browser.preferences.instantApply", false);
// pref("browser.backspace_action", 0);
END

  install -Dvm644 /dev/stdin "$pref/disable_typeahead.js" <<END
sticky_pref("accessibility.typeaheadfind", false);
sticky_pref("accessibility.typeaheadfind.autostart", false);
sticky_pref("accessibility.typeaheadfind.manual", false);
sticky_pref("accessibility.typeaheadfind.flashBar", 0);
END

  local distini="$pkgdir/usr/lib/$pkgname/distribution/distribution.ini"
  install -Dvm644 /dev/stdin "$distini" <<END
[Global]
id=archlinux
version=1.0
about=Mozilla Firefox for Arch Linux

[Preferences]
app.distributor=archlinux
app.distributor.channel=$pkgname
app.partner.archlinux=archlinux
END

  local i theme=official
  for i in 16 22 24 32 48 64 128 256; do
    install -Dvm644 browser/branding/$theme/default$i.png \
      "$pkgdir/usr/share/icons/hicolor/${i}x${i}/apps/$pkgname.png"
  done
  install -Dvm644 browser/branding/$theme/content/about-logo.png \
    "$pkgdir/usr/share/icons/hicolor/192x192/apps/$pkgname.png"
  install -Dvm644 browser/branding/$theme/content/about-logo@2x.png \
    "$pkgdir/usr/share/icons/hicolor/384x384/apps/$pkgname.png"
  install -Dvm644 browser/branding/$theme/content/about-logo.svg \
    "$pkgdir/usr/share/icons/hicolor/scalable/apps/$pkgname.svg"
  install -Dvm644 ../identity-icons-brand.svg \
    "$pkgdir/usr/share/icons/hicolor/symbolic/apps/$pkgname-symbolic.svg"

  install -Dvm644 $srcdir/$pkgname.desktop \
    "$pkgdir/usr/share/applications/$pkgname.desktop"

  # Install a wrapper to avoid confusion about binary path
  install -Dvm755 /dev/stdin "$pkgdir/usr/bin/$pkgname" <<END
#!/bin/sh
exec /usr/lib/$pkgname/$pkgname "\$@"
END

  # Replace duplicate binary with wrapper
  # https://bugzilla.mozilla.org/show_bug.cgi?id=658850
  ln -srfv "$pkgdir/usr/bin/$pkgname" "$pkgdir/usr/lib/$pkgname/$pkgname-bin"

  # Use system certificates
  local nssckbi="$pkgdir/usr/lib/$pkgname/libnssckbi.so"
  if [[ -e $nssckbi ]]; then
    ln -srfv "$pkgdir/usr/lib/libnssckbi.so" "$nssckbi"
  fi

  # Install zh-TW locale
  #local xpi_dest="$pkgdir/usr/lib/$pkgname/browser/extensions"
  #install -Dvm644 "$srcdir/firefox-105.0a1.zh-TW.langpack.xpi" "$xpi_dest/langpack-zh-TW@firefox.mozilla.org.xpi"
}

# vim:set sw=2 et:
