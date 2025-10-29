# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Contributor: Ionut Biru <ibiru@archlinux.org>
# Contributor: Jakub Schmidtke <sjakub@gmail.com>

pkgname=floorp
_pkgname=Floorp
_reverse_dns_pkgname=one.ablaze.floorp
_pkgsrc_runtime='floorp-runtime'
_firefox_ver=144.0
pkgver=12.4.0
pkgrel=1
pkgdesc="Firefox fork by Ryosuke Asano, a Japanese community"
arch=(x86_64)
license=(MPL GPL LGPL)
url="https://floorp.ablaze.one/"
depends=(
  dbus-glib
  ffmpeg
  gtk3
  libpulse
  libxt
  mime-types
  nss
  ttf-font
)
makedepends=(
  breezy
  cbindgen
  clang
  deno
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
  python-dulwich
  rsync
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
source=("git+https://github.com/Floorp-Projects/Floorp.git#tag=v$pkgver"
  "floorp-runtime::git+https://github.com/Floorp-Projects/Floorp-runtime#tag=daily-532"
  "floorp-projects.floorp-core::git+https://github.com/Floorp-Projects/Floorp-core.git"
  "git+https://github.com/openSUSE/firefox-maintenance.git"
  "https://dev.gentoo.org/~juippis/mozilla/patchsets/firefox-144-patches-02.tar.xz"
  fix_csd_window_buttons.patch
  0001-move-user-profile-to-XDG_CONFIG_HOME.patch
  0002-skip-creation-of-user-directory-extensions.patch
  floorp.desktop)
sha256sums=('SKIP'
  'SKIP'
  'SKIP'
  'SKIP'
  '63a2cd263b512ea6f9b487a7dcca3b7c673aeedf9c6dc0b7574bc64193515080'
  'e08d0bc5b7e562f5de6998060e993eddada96d93105384960207f7bdf2e1ed6e'
  '8b35735a3769e532ff856a3de849be1543c58397f6cbd1e29987a05dcfe615ee'
  '5ef41e4533a1023c12ed8e8b8305dd58b2a543ba659e64cffd5126586f7c2970'
  'f883a43af53f08e5b36ae89a643a2c32913a90c330e169d8b52f9158984dc092')
validpgpkeys=(
  # Mozilla Software Releases <release@mozilla.com>
  # https://blog.mozilla.org/security/2023/05/11/updated-gpg-key-for-signing-firefox-releases/
  14F26682D0916CDD81E37B6D61B7B526D98F0353
)
# implement official locale xpi
_url=https://archive.mozilla.org/pub/firefox/releases/$_firefox_ver
source+=(
  "firefox-$_firefox_ver-SHA512SUMS::$_url/SHA512SUMS"
  "firefox-$_firefox_ver-SHA512SUMS.asc::$_url/SHA512SUMS.asc"
)
sha256sums+=('SKIP'
             'SKIP')

_languages=(
  'ach         "Acholi"'
  'af          "Afrikaans"'
  'an          "Aragonese"'
  'ar          "Arabic"'
  'ast         "Asturian"'
  'az          "Azerbaijani"'
  'be          "Belarusian"'
  'bg          "Bulgarian"'
  'bn          "Bengali"'
  'br          "Breton"'
  'bs          "Bosnian"'
  'ca          "Catalan"'
  'ca-valencia "Catalan (Valencian)"'
  'cak         "Maya Kaqchikel"'
  'cs          "Czech"'
  'cy          "Welsh"'
  'da          "Danish"'
  'de          "German"'
  'dsb         "Lower Sorbian"'
  'el          "Greek"'
  'en-CA       "English (Canadian)"'
  'en-GB       "English (British)"'
  'en-US       "English (US)"'
  'eo          "Esperanto"'
  'es-AR       "Spanish (Argentina)"'
  'es-CL       "Spanish (Chile)"'
  'es-ES       "Spanish (Spain)"'
  'es-MX       "Spanish (Mexico)"'
  'et          "Estonian"'
  'eu          "Basque"'
  'fa          "Persian"'
  'ff          "Fulah"'
  'fi          "Finnish"'
  'fr          "French"'
  'fur         "Friulian"'
  'fy-NL       "Frisian"'
  'ga-IE       "Irish"'
  'gd          "Gaelic (Scotland)"'
  'gl          "Galician"'
  'gn          "Guarani"'
  'gu-IN       "Gujarati (India)"'
  'he          "Hebrew"'
  'hi-IN       "Hindi (India)"'
  'hr          "Croatian"'
  'hsb         "Upper Sorbian"'
  'hu          "Hungarian"'
  'hy-AM       "Armenian"'
  'ia          "Interlingua"'
  'id          "Indonesian"'
  'is          "Icelandic"'
  'it          "Italian"'
  'ja          "Japanese"'
  'ka          "Georgian"'
  'kab         "Kabyle"'
  'kk          "Kazakh"'
  'km          "Khmer"'
  'kn          "Kannada"'
  'ko          "Korean"'
  'lij         "Ligurian"'
  'lt          "Lithuanian"'
  'lv          "Latvian"'
  'mk          "Macedonian"'
  'mr          "Marathi"'
  'ms          "Malay"'
  'my          "Burmese"'
  'nb-NO       "Norwegian (Bokmål)"'
  'ne-NP       "Nepali"'
  'nl          "Dutch"'
  'nn-NO       "Norwegian (Nynorsk)"'
  'oc          "Occitan"'
  'pa-IN       "Punjabi (India)"'
  'pl          "Polish"'
  'pt-BR       "Portuguese (Brazilian)"'
  'pt-PT       "Portuguese (Portugal)"'
  'rm          "Romansh"'
  'ro          "Romanian"'
  'ru          "Russian"'
  'sat         "Santali"'
  'sc          "Sardinian"'
  'sco         "Scots"'
  'si          "Sinhala"'
  'sk          "Slovak"'
  'skr         "Saraiki"'
  'sl          "Slovenian"'
  'son         "Songhai"'
  'sq          "Albanian"'
  'sr          "Serbian"'
  'sv-SE       "Swedish"'
  'szl         "Silesian"'
  'ta          "Tamil"'
  'te          "Telugu"'
  'tg          "Tajik"'
  'th          "Thai"'
  'tl          "Tagalog"'
  'tr          "Turkish"'
  'trs         "Chicahuaxtla Triqui"'
  'uk          "Ukrainian"'
  'ur          "Urdu"'
  'uz          "Uzbek"'
  'vi          "Vietnamese"'
  'xh          "Xhosa"'
  'zh-CN       "Chinese (Simplified)"'
  'zh-TW       "Chinese (Traditional)"'
)

for _lang in "${_languages[@]}"; do
  _locale=${_lang%% *}
  _pkg=firefox-l10n-$_firefox_ver-$_locale.xpi
  source+=("$_pkg::$_url/linux-x86_64/xpi/$_locale.xpi")
  sha256sums+=('SKIP')
  noextract+=($_pkg)
done

verify() {
  cd "$SRCDEST"
  sed -n "s|  linux-x86_64/xpi/|  firefox-l10n-$_firefox_ver-|p" \
    firefox-$_firefox_ver-SHA512SUMS | sha512sum -c -
}

prepare() {
  mkdir mozbuild

  # put Floorp into Mozilla source tree "./floorp-runtime/noraneko"
  rm -rfv "$_pkgsrc_runtime/$pkgname"
  cp -rf "$srcdir/$_pkgname" "$_pkgsrc_runtime/noraneko"

  # fix locale compile error
  sed '/brand.dtd/d' -i "$_pkgsrc_runtime"/.github/assets/branding/*/locales/jar.mn

  # copy branding assets
  cp -rf "$_pkgsrc_runtime"/.github/assets/branding/* "$_pkgsrc_runtime"/browser/branding/

  sed -i 's|https://@MOZ_APPUPDATE_HOST@/update/6/%PRODUCT%/%VERSION%/%BUILD_ID%/%BUILD_TARGET%/%LOCALE%/%CHANNEL%/%OS_VERSION%/%SYSTEM_CAPABILITIES%/%DISTRIBUTION%/%DISTRIBUTION_VERSION%/update.xml|https://%NORA_UPDATE_HOST%update.xml|g' "$_pkgsrc_runtime/build/application.ini.in"

  # prepare firefox source
  cd "$_pkgsrc_runtime" || exit

  # prepare api keys
  cp "${srcdir}"/floorp-projects.floorp-core/apis/api-*-key ./

  # Fix js ICU compatibility error for icu-76.1
  # sed 's/icu-i18n/icu-uc &/' -i js/moz.configure

  msg 'Noraneko patch'
  local noraneko_patch=($(ls "$srcdir/$_pkgsrc_runtime/.github/patches/upstream/"))
  for src in "${noraneko_patch[@]}"; do
    msg "Applying patch $src"
    patch -Np1 -i "$srcdir/$_pkgsrc_runtime/.github/patches/upstream/$src"
  done

  msg 'Gentoo patch'
  rm -rf $srcdir/firefox-patches/00{21,22}*
  sed 's,%%PORTAGE_WORKDIR%%/wasi-sdk-%%WASI_SDK_VER%%-%%WASI_ARCH%%-linux,/usr,;
       s,%%WASI_SDK_LLVM_VER%%,21,g;
       s,wasm32-unknown-wasi,wasi,;
       s,libclang_rt.builtins.a,libclang_rt.builtins-wasm32.a,' -i "$srcdir"/firefox-patches/*-bgo-940031-wasm-support.patch
  local gentoo_patch=($(ls $srcdir/firefox-patches/))
  for src in "${gentoo_patch[@]}"; do
    msg "Applying patch $src..."
    patch -Np1 -i "$srcdir/firefox-patches/$src"
  done

  msg 'Opensuse Patch'
  # https://github.com/openSUSE/firefox-maintenance/blob/master/firefox/MozillaFirefox.spec
  local suse_patch=( # xulrunner/gecko patches
    # 'mozilla-kde.patch'
    'mozilla-ntlm-full-path.patch'
    # 'mozilla-aarch64-startup-crash.patch'
    # 'mozilla-fix-aarch64-libopus.patch'
    # 'mozilla-s390-context.patch'
    # 'mozilla-pgo.patch' # previous patch detected
    'mozilla-reduce-rust-debuginfo.patch'
    'mozilla-bmo1504834-part1.patch'
    # 'mozilla-bmo1504834-part3.patch'
    # 'mozilla-bmo1512162.patch'
    # 'mozilla-fix-top-level-asm.patch' # broken patch
    'mozilla-bmo849632.patch'
    'mozilla-bmo998749.patch'
    'mozilla-libavcodec58_91.patch'
    # 'mozilla-silence-no-return-type.patch'
    # 'mozilla-bmo531915.patch' # broken patch
    'one_swizzle_to_rule_them_all.patch'
    'svg-rendering.patch'
    # 'mozilla-partial-revert-1768632.patch'
    # 'mozilla-bmo1775202.patch'
    # 'mozilla-rust-disable-future-incompat.patch'
    # Firefox patches
    # 'firefox-kde.patch'
    'firefox-branded-icons.patch')
  for src in "${suse_patch[@]}"; do
    msg "Applying patch $src..."
    patch -Np1 -i "${srcdir}/firefox-maintenance/firefox/${src}"
  done

  # EVENT__SIZEOF_TIME_T does not exist on upstream libevent, see event-config.h.cmake
  sed -i '/CHECK_EVENT_SIZEOF(TIME_T, time_t);/d' ipc/chromium/src/base/message_pump_libevent.cc

  cat >../mozconfig <<END
ac_add_options --enable-application=browser
ac_add_options --disable-artifact-builds
ac_add_options --with-app-name=${pkgname}
ac_add_options --with-app-basename=${_pkgname}
mk_add_options MOZ_OBJDIR=${PWD@Q}/obj-artifact-build-output
ac_add_options --with-version-file-path=noraneko/static/gecko/config

ac_add_options --prefix=/usr
ac_add_options --enable-hardening
ac_add_options --enable-optimize="-O3 -march=x86-64-v3 -msse3 -mssse3 -msse4.1 -msse4.2 -mavx -maes -mpopcnt -mpclmul"
ac_add_options --enable-rust-simd
ac_add_options --enable-wasm-simd
ac_add_options --enable-linker=lld
ac_add_options --disable-elf-hack
ac_add_options --disable-bootstrap
# ac_add_options --enable-default-toolkit=cairo-gtk3-x11-wayland
ac_add_options --enable-default-toolkit=cairo-gtk3-wayland
ac_add_options --with-wasi-sysroot=/usr/share/wasi-sysroot

# Branding
export MOZILLA_OFFICIAL=1
ac_add_options --enable-update-channel=release
ac_add_options --with-branding=browser/branding/floorp-official
ac_add_options --with-distribution-id=org.archlinux
ac_add_options --with-unsigned-addon-scopes=app,system
ac_add_options --allow-addon-sideload

# Floorp environment variables
mk_add_options AUTOCLOBBER=1
export MOZ_INCLUDE_SOURCE_INFO=1
export MOZ_APP_REMOTINGNAME=${_pkgname}
unset MOZ_REQUIRE_SIGNING
unset MOZ_DATA_REPORTING
unset MOZ_TELEMETRY_REPORTING
export OPT_LEVEL="3"
export RUSTC_OPT_LEVEL="3"
export MOZ_PACKAGE_JSSHELL=1

# Keys
ac_add_options --with-mozilla-api-keyfile=${PWD@Q}/api-mozilla-key
ac_add_options --with-google-location-service-api-keyfile=${PWD@Q}/api-google-location-service-key
ac_add_options --with-google-safebrowsing-api-keyfile=${PWD@Q}/api-google-safe-browsing-key

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
ac_add_options --target=x86_64-pc-linux
ac_add_options --enable-av1
ac_add_options --enable-eme=widevine
ac_add_options --enable-jxl
ac_add_options --enable-audio-backends="pulseaudio,alsa,jack"
ac_add_options --enable-raw
ac_add_options --enable-sandbox
ac_add_options --enable-wasm-avx
ac_add_options --enable-webrtc
ac_add_options --disable-default-browser-agent
ac_add_options --disable-parental-controls
ac_add_options --enable-proxy-bypass-protection
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

  msg 'Injecting Floorp, before-mach step'
  pushd "$srcdir/$_pkgsrc_runtime/noraneko" || return
  export NODE_ENV=production
  deno install --allow-scripts
  deno task feles-build misc writeVersion
  deno task feles-build build --phase before-mach
  popd || return

  msg 'Apply packaging patches'
  git apply --verbose --ignore-space-change --ignore-whitespace .github/patches/packaging/*.patch

  local _firefox_ver="$(cat browser/config/version.txt)"
  sed "s,$,@${_firefox_ver}," -i noraneko/static/gecko/config/version.txt -i noraneko/static/gecko/config/version_display.txt
  # fix l10n path for non-firefox directory
  sed 's,{l}browser/branding/official,{l}browser/branding/floorp-official,' -i browser/locales/l10n.toml

  patch -Np1 -i "${srcdir}/0001-move-user-profile-to-XDG_CONFIG_HOME.patch"
  patch -Np1 -i "${srcdir}/0002-skip-creation-of-user-directory-extensions.patch"
}

build() {
  cd "$_pkgsrc_runtime" || exit

  export LIBGL_ALWAYS_SOFTWARE=true
  export MACH_BUILD_PYTHON_NATIVE_PACKAGE_SOURCE=pip
  export MOZ_BUILD_DATE="$(date -u${SOURCE_DATE_EPOCH:+d @$SOURCE_DATE_EPOCH} +%Y%m%d%H%M%S)"
  export MOZBUILD_STATE_PATH="$srcdir/mozbuild"
  export MOZ_ENABLE_FULL_SYMBOLS=0
  export MOZ_NOSPAM=1
  export RUSTFLAGS="-C debuginfo=1"
  export XDG_RUNTIME_DIR="${XDG_RUNTIME_DIR:-$srcdir/xdg}"
  LDFLAGS+=' -Wl,--undefined-version'
  install -dm700 "${XDG_RUNTIME_DIR:?}"

  # malloc_usable_size is used in various parts of the codebase
  CFLAGS="${CFLAGS/_FORTIFY_SOURCE=2/_FORTIFY_SOURCE=3}"
  CXXFLAGS="${CXXFLAGS/_FORTIFY_SOURCE=2/_FORTIFY_SOURCE=3}"

  # Breaks compilation since https://bugzilla.mozilla.org/show_bug.cgi?id=1896066
  CFLAGS="${CFLAGS/-fexceptions/}"
  CXXFLAGS="${CXXFLAGS/-fexceptions/}"

  # Compiler setting
  export AR=llvm-ar
  export CC=clang
  export CC_LD=lld
  export CXX=clang++
  export CXX_LD=lld
  export NM=llvm-nm
  export RANLIB=llvm-ranlib

  # Zen Browser flags
  export CFLAGS="$CFLAGS -O3 -ffp-contract=fast -march=x86-64-v3 -msse3 -mssse3 -msse4.1 -msse4.2 -mavx -mavx2 -mfma -maes -mpopcnt -mpclmul"
  export CPPFLAGS="$CPPFLAGS -O3 -ffp-contract=fast -march=x86-64-v3 -msse3 -mssse3 -msse4.1 -msse4.2 -mavx -mavx2 -mfma -maes -mpopcnt -mpclmul"
  export CXXFLAGS="$CXXFLAGS -O3 -flto=thin -ffp-contract=fast -march=x86-64-v3 -msse3 -mssse3 -msse4.1 -msse4.2 -mavx -mavx2 -mfma -maes -mpopcnt -mpclmul"
  export LDFLAGS="$LDFLAGS -Wl,-O3 -Wl,-mllvm,-fp-contract=fast -march=x86-64-v3"
  export RUSTFLAGS="$RUSTFLAGS -C target-cpu=x86-64-v3 -C target-feature=+sse4.1 -C target-feature=+avx2 -C codegen-units=1 -Clink-args=--icf=safe"

  # LTO needs more open files
  ulimit -n 4096

  # Do 3-tier PGO
  msg "Building instrumented browser..."
  cat >.mozconfig ../mozconfig - <<END
ac_add_options --enable-profile-generate=cross
export MOZ_ENABLE_FULL_SYMBOLS=1
END
  ./mach build

  # copy buildid2 to bin/browser
  cp noraneko/_dist/buildid2 obj-artifact-build-output/dist/bin/browser/

  msg "Profiling instrumented browser..."
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
    MOZ_REMOTE_SETTINGS_DEVTOOLS=1 \
    GTK_A11Y=none NO_AT_BRIDGE=1 dbus-run-session \
    xvfb-run -s "-screen 0 1920x1080x24 -nolisten local" \
    ./mach python build/pgo/profileserver.py

  stat -c "Profile data found (%s bytes)" merged.profdata
  test -s merged.profdata
  stat -c "Jar log found (%s bytes)" jarlog
  test -s jarlog

  msg "Removing instrumented browser..."
  ./mach clobber objdir

  msg "Building optimized browser..."
  cat >.mozconfig ../mozconfig - <<END
ac_add_options --enable-lto=cross
ac_add_options --enable-profile-use=cross
ac_add_options --with-pgo-profile-path=${PWD@Q}/merged.profdata
ac_add_options --with-pgo-jarlog=${PWD@Q}/jarlog
END
  ./mach build

  # copy buildid2 to bin/browser
  cp noraneko/_dist/buildid2 obj-artifact-build-output/dist/bin/browser/

  msg 'Injecting Floorp, after-mach step'
  pushd "$srcdir/$_pkgsrc_runtime/noraneko" || return
  deno task feles-build build --phase after-mach
  popd || return

  # avoid symlinks, which make the git apply fails
  rsync -aL obj-artifact-build-output/ obj-artifact-build-output_new/
  mv obj-artifact-build-output obj-artifact-build-output_old
  mv obj-artifact-build-output_new obj-artifact-build-output
  git apply --unsafe-paths --verbose noraneko/tools/patches/*.patch --directory obj-artifact-build-output/dist/bin

  bash noraneko/static/gecko/pref/override.sh obj-artifact-build-output/dist/bin/browser/defaults/preferences/firefox.js
}

package() {
  cd "$_pkgsrc_runtime" || exit
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

  local i theme=floorp-official
  for i in 16 22 24 32 48 64 128 256; do
    install -Dvm644 browser/branding/$theme/default$i.png \
      "$pkgdir/usr/share/icons/hicolor/${i}x${i}/apps/${_reverse_dns_pkgname}.png"
  done
  install -Dvm644 browser/branding/$theme/content/about-logo.png \
    "$pkgdir/usr/share/icons/hicolor/192x192/apps/${_reverse_dns_pkgname}.png"
  install -Dvm644 browser/branding/$theme/content/about-logo@2x.png \
    "$pkgdir/usr/share/icons/hicolor/384x384/apps/${_reverse_dns_pkgname}.png"
  install -Dvm644 browser/branding/$theme/content/about-logo.svg \
    "$pkgdir/usr/share/icons/hicolor/scalable/apps/${_reverse_dns_pkgname}.svg"

  install -Dvm644 "$srcdir/floorp.desktop" \
    "$pkgdir/usr/share/applications/${_reverse_dns_pkgname}.desktop"

  # Install a wrapper to avoid confusion about binary path
  install -Dvm755 /dev/stdin "$pkgdir/usr/bin/${_reverse_dns_pkgname}" <<END
#!/bin/sh
MOZ_LEGACY_HOME=0 /usr/lib/floorp/floorp-bin "\$@"
END

  # Replace duplicate binary with wrapper
  # https://bugzilla.mozilla.org/show_bug.cgi?id=658850
  ln -srfv "$pkgdir/usr/bin/${_reverse_dns_pkgname}" "$pkgdir/usr/lib/floorp/floorp"
  ln -srfv "$pkgdir/usr/bin/${_reverse_dns_pkgname}" "$pkgdir/usr/bin/floorp"

  # Use system certificates
  local nssckbi="$pkgdir/usr/lib/floorp/libnssckbi.so"
  if [[ -e $nssckbi ]]; then
    ln -srfv "$pkgdir/usr/lib/libnssckbi.so" "$nssckbi"
  fi

  # install locales
  for _lang in "${_languages[@]}"; do
    _locale=${_lang%% *}
    install -Dm644 $srcdir/firefox-l10n-$_firefox_ver-$_locale.xpi \
      "$pkgdir/usr/lib/$pkgname/browser/extensions/langpack-$_locale@firefox.mozilla.org.xpi"
  done

}

# vim:set sw=2 et:
