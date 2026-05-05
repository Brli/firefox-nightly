# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Contributor: Ionut Biru <ibiru@archlinux.org>
# Contributor: Jakub Schmidtke <sjakub@gmail.com>

pkgname=firefox-nightly
pkgver=152.0a1.20260503.r3135.ga353242aeafd
pkgrel=1
pkgdesc="Fast, Private & Safe Web Browser - Nightly branch"
arch=(x86_64)
license=(
  GPL
  LGPL
  MPL
)
url="https://www.mozilla.org/firefox/"
depends=(
  dbus
  ffmpeg
  gtk3
  libpulse
  libxt
  mime-types
  nss
  ttf-font
)
makedepends=(
  cbindgen
  clang
  diffutils
  dump_syms
  imake
  jack
  lld
  llvm
  mercurial
  mesa
  nasm
  nodejs
  npm
  python
  rust
  unzip
  wasi-compiler-rt
  wasi-libc
  wasi-libc++
  wasi-libc++abi
  xorg-server-xvfb
  yasm
  zip
)
optdepends=(
  'hunspell-en_US: Spell checking, American English'
  'libnotify: Notification integration'
  'networkmanager: Location detection via available WiFi networks'
  'pulseaudio: Audio support'
  'speech-dispatcher: Text-to-Speech'
  'xdg-desktop-portal: Screensharing with Wayland'
)
options=(
  !debug
  !emptydirs
  !lto
  !makeflags
  !strip
)
_gentoo_patch=150-patches-02
source=(
        git+https://github.com/mozilla-firefox/firefox.git
        git+https://github.com/mozilla-l10n/firefox-l10n.git
        git+https://github.com/openSUSE/firefox-maintenance.git
        librewolf-settings::git+https://codeberg.org/librewolf/settings.git
        librewolf-patch::git+https://codeberg.org/librewolf/source.git
        arkenfox::git+https://github.com/arkenfox/user.js.git
        https://dev.gentoo.org/~juippis/mozilla/patchsets/firefox-${_gentoo_patch}.tar.xz
        firefox.desktop
        identity-icons-brand.svg
        org.mozilla.firefox-nightly.metainfo.xml
        0001-Install-under-remoting-name.patch
        0002-skip-creation-of-user-directory-extensions.patch
)
sha256sums=(
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'dbacf931b7f42dd05aa5d60766c0ed21692f3da47e15e30a948eaeecd347e32d'
            '5e13c1ba92819db099979579e2833d07438657e473e8831b9c654635d28ccf58'
            'a9b8b4a0a1f4a7b4af77d5fc70c2686d624038909263c795ecc81e0aec7711e9'
            '0488650eec53e2a565718e28dbbca4279250ad6bc7cbfdb449eeb349fbc22291'
            'ef63a12975f108f30b00bb3290d9ca76f311d8af9c1d5dfc0d8335ad57e8f77c'
            '5ef41e4533a1023c12ed8e8b8305dd58b2a543ba659e64cffd5126586f7c2970'
)
validpgpkeys=('14F26682D0916CDD81E37B6D61B7B526D98F0353') # Mozilla Software Releases <release@mozilla.com>

# Google API keys (see http://www.chromium.org/developers/how-tos/api-keys)
# Note: These are for Arch Linux use ONLY. For your own distribution, please
# get your own set of keys. Feel free to contact foutrelis@archlinux.org for
# more information.
_google_api_key=AIzaSyDwr302FpOSkGRpLlUpPThNTDPbXcIn_FM

pkgver() {
  cd firefox
  _pkgver=$(cat browser/config/version.txt)
  _date=$(git show -s --format=%ci | cut -d ' ' -f 1 | tr -d '-')
  _commit=$(git describe --abbrev --tags | sed 's,_,.,g;s,-,.,g' | cut -d '.' -f 5-)
  printf "%s.%s.%s" "${_pkgver}" "${_date}" "r${_commit}"
}

prepare() {
  mkdir mozbuild
  cd firefox

  # Revert NSS requirement
  # sed 's,nss >= 3.101,nss >= 3.100,' -i build/moz.configure/nss.configure
  # Revert ICU requirement
  # sed 's,icu-i18n >= 76.1,icu-i18n >= 75.1,' -i js/moz.configure

  # Fix js ICU compatibility error for icu-76.1
  # sed 's/icu-i18n/icu-uc &/' -i js/moz.configure

  # Don't let the branding override our remoting name
  sed '/^MOZ_APP_REMOTINGNAME=/d' -i browser/branding/nightly/configure.sh

  msg 'Gentoo patch'
  rm -rf $srcdir/firefox-patches/*musl*
  rm -rf $srcdir/firefox-patches/00{08,23,27,28}*
  sed 's,%%PORTAGE_WORKDIR%%/wasi-sdk-%%WASI_SDK_VER%%-%%WASI_ARCH%%-linux,/usr,;
       s,%%WASI_SDK_LLVM_VER%%,22,;' -i "$srcdir/firefox-patches"/*-bgo-940031-wasm-support.patch
  local gentoo_patch=($(ls $srcdir/firefox-patches/))
  for src in "${gentoo_patch[@]}"; do
    msg "Applying patch $src..."
    patch -Np1 -i "$srcdir/firefox-patches/$src"
  done

  msg 'opensuse patch'
  # https://github.com/openSUSE/firefox-maintenance/blob/master/firefox/MozillaFirefox.spec
  local suse_patch=(
    'firefox-branded-icons.patch')
  for src in "${suse_patch[@]}"; do
    msg "Applying patch $src..."
    patch -Np1 -i "${srcdir}/firefox-maintenance/firefox/$src"
  done

  msg 'librewolf patch'
  local librewolf_patch=(
                         # 'custom-ubo-assets-bootstrap-location.patch'
                         'disable-data-reporting-at-compile-time.patch'
                         'fullpage-translations.patch'
                         'remove-openai.patch'
                         'remove-pingsender.patch'
                         'xdg-dir.patch'
                        )
  for src in "${librewolf_patch[@]}"; do
    msg "Applying patch $src..."
    patch -Np1 -i "${srcdir}/librewolf-patch/patches/$src"
  done

  # EVENT__SIZEOF_TIME_T does not exist on upstream libevent, see event-config.h.cmake
  sed -i '/CHECK_EVENT_SIZEOF(TIME_T, time_t);/d' ipc/chromium/src/base/message_pump_libevent.cc

  echo -n "$_google_api_key" >google-api-key

  cat >../mozconfig <<END
ac_add_options --enable-application=browser
ac_add_options --with-app-name=${pkgname}
mk_add_options MOZ_OBJDIR=${PWD@Q}/obj

ac_add_options --prefix=/usr
ac_add_options --enable-release
ac_add_options --enable-hardening
ac_add_options --enable-wasm-avx
ac_add_options --enable-optimize="-O3 -march=x86-64-v3 -msse3 -mssse3 -msse4.1 -msse4.2 -mavx -maes -mpopcnt -mpclmul"
ac_add_options --enable-rust-simd
ac_add_options --enable-wasm-simd
ac_add_options --enable-linker=lld
ac_add_options --disable-elf-hack
ac_add_options --disable-bootstrap
ac_add_options --with-wasi-sysroot=/usr/share/wasi-sysroot
# ac_add_options --without-wasm-sandboxed-libraries

# Branding
ac_add_options --with-branding=browser/branding/nightly
ac_add_options --enable-update-channel=nightly
ac_add_options --with-distribution-id=org.archlinux
ac_add_options --with-unsigned-addon-scopes=app,system
ac_add_options --allow-addon-sideload
export RUSTC_OPT_LEVEL=3
export MOZILLA_OFFICIAL=1
export MOZ_APP_REMOTINGNAME=$pkgname
export MOZ_REQUIRE_SIGNING=1
unset MOZ_TELEMETRY_REPORTING
unset MOZ_DATA_REPORTING

# Keys
ac_add_options --with-google-location-service-api-keyfile=${PWD@Q}/google-api-key
ac_add_options --with-google-safebrowsing-api-keyfile=${PWD@Q}/google-api-key

# System libraries
ac_add_options --with-system-nspr
ac_add_options --without-system-nss
ac_add_options --with-system-jpeg
ac_add_options --with-system-webp
ac_add_options --with-system-zlib
ac_add_options --with-system-libevent
ac_add_options --with-system-libvpx
ac_add_options --with-system-harfbuzz
ac_add_options --enable-system-ffi
ac_add_options --enable-system-pixman

# Features
ac_add_options --enable-sandbox
ac_add_options --enable-audio-backends='alsa,pulseaudio,jack'
ac_add_options --enable-jxl
ac_add_options --disable-necko-wifi
ac_add_options --disable-crashreporter
ac_add_options --disable-updater
ac_add_options --disable-tests
ac_add_options --target=x86_64-pc-linux
END

  # Fake mozilla version
  echo '149.0' > config/milestone.txt

  # Desktop file
  sed "s,@MOZ_APP_NAME@,${pkgname},g" -i "${srcdir}/firefox.desktop"

  msg 'Apply personal patches'
  local local_patch=($(ls $srcdir/*.patch))
  for src in "${local_patch[@]}"; do
    msg "Applying patch $src..."
    patch -Np1 -i "$src"
  done

  # Remove patched rust file checksums
  sed 's/\("files":{\)[^}]*/\1/' -i \
    third_party/rust/*/.cargo-checksum.json
}

build() {
  cd firefox

  export MOZ_NOSPAM=1
  export MOZBUILD_STATE_PATH="$srcdir/mozbuild"
  export MOZ_ENABLE_FULL_SYMBOLS=1
  export MOZ_BUILD_DATE="$(date -u${SOURCE_DATE_EPOCH:+d @$SOURCE_DATE_EPOCH} +%Y%m%d%H%M%S)"
  export LIBGL_ALWAYS_SOFTWARE=true
  export MACH_BUILD_PYTHON_NATIVE_PACKAGE_SOURCE=pip
  export CC=clang
  export CXX=clang++

  # malloc_usable_size is used in various parts of the codebase
  CFLAGS="${CFLAGS/_FORTIFY_SOURCE=2/_FORTIFY_SOURCE=3}"
  CXXFLAGS="${CXXFLAGS/_FORTIFY_SOURCE=2/_FORTIFY_SOURCE=3}"

  # Breaks compilation since https://bugzilla.mozilla.org/show_bug.cgi?id=1896066
  CFLAGS="${CFLAGS/-fexceptions/}"
  CXXFLAGS="${CXXFLAGS/-fexceptions/}"

  # LTO needs more open files
  ulimit -n 4096

  # Do 3-tier PGO
  msg "Building instrumented browser..."
  cat >.mozconfig ../mozconfig - <<END
ac_add_options --enable-profile-generate=cross
END
  ./mach build --priority normal

  msg "Profiling instrumented browser..."
  ./mach npm ci --prefix tools/terser
  ./mach package
  LLVM_PROFDATA=llvm-profdata \
    JARLOG_FILE="$PWD/jarlog" \
    MOZ_DISABLE_CONTENT_SANDBOX=1 \
    MOZ_DISABLE_GMP_SANDBOX=1 \
    MOZ_DISABLE_GPU_SANDBOX=1 \
    MOZ_DISABLE_RDD_SANDBOX=1 \
    MOZ_DISABLE_SOCKET_PROCESS_SANDBOX=1 \
    MOZ_DISABLE_UTILITY_SANDBOX=1 \
    MOZ_DISABLE_VR_SANDBOX=1 \
    GTK_A11Y=none NO_AT_BRIDGE=1 dbus-run-session \
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
ac_add_options --with-l10n-base=$srcdir/firefox-l10n
END
  ./mach build --priority normal

  msg 'Building locales'
  ./mach package
  #  ./mach build chrome-zh-TW
  #   export MOZ_CHROME_MULTILOCALE="ar cs da de el en-US en-GB es-ES fr hu id it ja ko lt nl nn-NO pl pt-BR pt-PT ru sv-SE th tr uk vi zh-CN zh-TW"
  export MOZ_CHROME_MULTILOCALE="en-US zh-TW"
  for AB_CD in $MOZ_CHROME_MULTILOCALE; do
    msg "Building locales $AB_CD"
    ./mach build chrome-$AB_CD
  done
}

package() {
  cd firefox
  DESTDIR="$pkgdir" ./mach install

  # install arkenfox user.js
  install -Dvm644 "$srcdir/arkenfox/user.js" "$pkgdir/usr/lib/${pkgname}/browser/defaults/preferences/arkenfox.js"

  # install librewolf policy
  install -Dvm644 "$srcdir/librewolf-settings/distribution/policies.json" "$pkgdir/usr/lib/${pkgname}/distribution/policies.json"

  local pref="$pkgdir/usr/lib/${pkgname}/browser/defaults/preferences"
  install -Dvm644 /dev/stdin "$pref/vendor.js" <<END
// Default download to /tmp
pref("browser.download.folderList", 2);
pref("browser.download.dir", "/tmp");

// Use system-provided dictionaries
pref("spellchecker.dictionary_path", "/usr/share/hunspell");

// Don't disable extensions in the application directory
pref("extensions.autoDisableScopes", 11);

// Enable GNOME Shell search provider
pref("browser.gnome-search-provider.enabled", true);

// UA override to latest Edge
pref("general.useragent.override", "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/144.0.0.0 Safari/537.36 Edg/144.0.0.0");

// Scale UI
// pref("layout.css.devPixelsPerPx",    "1.2");

// Enable new vedrtical tab
pref("browser.engagement.sidebar-button.has-used", true);
pref("browser.toolbarbuttons.introduced.sidebar-button", true);
pref("sidebar.backupState", "{\"command\":\"\",\"panelOpen\":false,\"launcherWidth\":53,\"launcherExpanded\":false,\"launcherVisible\":true}");
pref("sidebar.new-sidebar.has-used", true);
pref("sidebar.revamp", true);
pref("sidebar.verticalTabs", true);
pref("sidebar.visibility", "expand-on-hover");

// Experimental content blocker
pref("privacy.trackingprotection.content.protection.enabled", true);
pref("privacy.trackingprotection.content.protection.enabled", "https://easylist.to/easylist/easylist.txt|https://easylist.to/easylist/easyprivacy.txt|https://easylist-downloads.adblockplus.org/easylistchina.txt|https://raw.githubusercontent.com/cjx82630/cjxlist/master/cjx-annoyance.txt");
END

  install -Dvm644 /dev/stdin "$pref/gentoo.js" <<END
/* gentoo ebuild */
pref("gfx.x11-egl.force-enabled", true);
sticky_pref("gfx.font_rendering.graphite.enabled", true);
pref("media.gmp-gmpopenh264.autoupdate", true);
pref("media.gmp-widevinecdm.autoupdate", true);

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
pref("media.ffvpx.enabled", true);
END
  install -Dvm644 /dev/stdin "$pref/nvidia.js" <<END
// nvidia
pref("widget.dmabuf.force-enabled", true);
pref("media.rdd-ffmpeg.enabled",    true);
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

  local distini="$pkgdir/usr/lib/${pkgname}/distribution/distribution.ini"
  install -Dvm644 /dev/stdin "$distini" <<END
[Global]
id=archlinux
version=1.0
about=Mozilla Firefox (Nightly version) for Arch Linux

[Preferences]
app.distributor=archlinux
app.distributor.channel=${pkgname}
app.partner.archlinux=archlinux
END

  # Link up system ONNX runtime
  ln -srv "$pkgdir/usr/lib/libonnxruntime.so" -t "$pkgdir/usr/lib/$pkgname"

  local i theme=nightly
  for i in 16 22 24 32 48 64 128 256; do
    install -Dvm644 browser/branding/$theme/default$i.png \
      "$pkgdir/usr/share/icons/hicolor/${i}x${i}/apps/${pkgname}.png"
  done
  install -Dvm644 browser/branding/$theme/content/about-logo.png \
    "$pkgdir/usr/share/icons/hicolor/192x192/apps/${pkgname}.png"
  install -Dvm644 browser/branding/$theme/content/about-logo@2x.png \
    "$pkgdir/usr/share/icons/hicolor/384x384/apps/${pkgname}.png"
  install -Dvm644 browser/branding/$theme/content/about-logo.svg \
    "$pkgdir/usr/share/icons/hicolor/scalable/apps/${pkgname}.svg"

  install -Dvm644 ../identity-icons-brand.svg \
    "$pkgdir/usr/share/icons/hicolor/symbolic/apps/${pkgname}-symbolic.svg"

  install -Dvm644 $srcdir/firefox.desktop \
    "$pkgdir/usr/share/applications/${pkgname}.desktop"

  install -Dvm644 "$srcdir/org.mozilla.$pkgname.metainfo.xml" -t "$pkgdir/usr/share/metainfo"

  # Install a wrapper to avoid confusion about binary path
  install -Dvm755 /dev/stdin "$pkgdir/usr/bin/${pkgname}" <<END
#!/bin/sh
MOZ_LEGACY_HOME=0 exec /usr/lib/${pkgname}/${pkgname}-bin "\$@"
END

  # Replace duplicate binary with wrapper
  # https://bugzilla.mozilla.org/show_bug.cgi?id=658850
  ln -srfv "$pkgdir/usr/bin/${pkgname}" "$pkgdir/usr/lib/${pkgname}/${pkgname}"

  # Use system certificates
  local nssckbi="$pkgdir/usr/lib/${pkgname}/libnssckbi.so"
  if [[ -e $nssckbi ]]; then
    ln -srfv "$pkgdir/usr/lib/libnssckbi.so" "$nssckbi"
  fi

  local sprovider="$pkgdir/usr/share/gnome-shell/search-providers/$pkgname.search-provider.ini"
  install -Dvm644 /dev/stdin "$sprovider" <<END
[Shell Search Provider]
DesktopId=$pkgname.desktop
BusName=org.mozilla.${pkgname//-/_}.SearchProvider
ObjectPath=/org/mozilla/${pkgname//-/_}/SearchProvider
Version=2
END
}

# vim:set sw=2 sts=-1 et:
