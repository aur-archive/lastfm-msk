# Maintainer: Jarek Sedlacek <jareksedlacek@gmail.com>

pkgname=lastfm-msk
pkgver=1.5.4.27091.dfsg1.1
orgver=1.5.4.27091
pkgrel=7
pkgdesc="The Last.fm client with msk patchset"
arch=('i686' 'x86_64')
url="http://www.mehercule.net/staticpages/index.php/lastfm"
license=('GPL')
depends=('alsa-lib' 'libsm' 'libgpod' 'libmad' 'qt4>=4.3')
makedepends=('patch')
provides=('lastfm-client')
conflicts=('lastfm-client')
install=lastfm-msk.install
source=("http://ftp.de.debian.org/debian/pool/main/l/lastfm/lastfm_${orgver}+dfsg1.orig.tar.gz"
\
        "http://www.mehercule.net/lastfm/build-fixes.diff" \
        "http://www.mehercule.net/lastfm/gcc41.diff" \
        "http://www.mehercule.net/lastfm/reduce-linkage.diff" \
        "http://www.mehercule.net/lastfm/no-fingerprint-lib.diff" \
        "http://www.mehercule.net/lastfm/alsa-uses-qdebug.diff" \
        "http://www.mehercule.net/lastfm/check-soundcard-errors.diff" \
        "http://www.mehercule.net/lastfm/tray-icon-size.diff" \
        "http://www.mehercule.net/lastfm/hide-scrobbledir-option.diff" \
        "http://www.mehercule.net/lastfm/tray-volume.diff" \
        "http://www.mehercule.net/lastfm/set-locale.diff" \
        "http://www.mehercule.net/lastfm/set-firstrun-status.diff" \
        "http://www.mehercule.net/lastfm/qt45.diff" \
        "http://www.mehercule.net/lastfm/qt46.diff" \
        "http://www.mehercule.net/lastfm/qt47.diff" \
        "http://www.mehercule.net/lastfm/hide-loved-radio.diff" \
        "http://www.mehercule.net/lastfm/ipod-scrobble-fix.diff" \
        "http://www.mehercule.net/lastfm/sidebar-crash-fix.diff" \
        "http://www.mehercule.net/lastfm/dirpaths.diff" \
        "http://www.mehercule.net/lastfm/icons.tar.gz" \
        "http://www.mehercule.net/lastfm/trayicons22.tar.gz" \
        "http://www.mehercule.net/lastfm/multi-sound.diff" \
        "http://www.mehercule.net/lastfm/dbus.diff" \
        "http://www.mehercule.net/lastfm/tag-cloud.diff" \
        "http://www.mehercule.net/lastfm/browser-select.diff" \
        "http://www.mehercule.net/lastfm/no-cruft.diff" \
        "http://www.mehercule.net/lastfm/gcc47.patch" \
        "lastfm.desktop" "lastfm.protocol")

build() {

# cd ${srcdir}/last.fm-${orgver}
   cd ${srcdir}/lastfm-${orgver}+dfsg1

   patch -Np1 -i ../build-fixes.diff
   patch -Np1 -i ../gcc41.diff
   patch -Np1 -i ../gcc47.patch
# patch -Np1 -i ../reduce-linkage.diff // BROKEN
# patch -Np1 -i ../link-to-needed.diff // BROKEN
   patch -Np1 -i ../no-fingerprint-lib.diff
   patch -Np1 -i ../alsa-uses-qdebug.diff
   patch -Np1 -i ../check-soundcard-errors.diff
   patch -Np1 -i ../tray-icon-size.diff
   patch -Np1 -i ../hide-scrobbledir-option.diff
   patch -Np1 -i ../tray-volume.diff
   patch -Np1 -i ../set-locale.diff
   patch -Np1 -i ../set-firstrun-status.diff
   patch -Np1 -i ../qt45.diff
   patch -Np1 -i ../qt46.diff
   patch -Np1 -i ../qt47.diff
   patch -Np1 -i ../hide-loved-radio.diff
   patch -Np1 -i ../ipod-scrobble-fix.diff
   patch -Np1 -i ../sidebar-crash-fix.diff
   patch -Np1 -i ../dirpaths.diff

# patch -Np1 -i ../multi-sound.diff //BROKEN
   patch -Np1 -i ../dbus.diff
   patch -Np1 -i ../tag-cloud.diff
   patch -Np1 -i ../browser-select.diff
   patch -Np1 -i ../no-cruft.diff
   sed -i 's|glib/glist.h|glib.h|' src/mediadevices/ipod/IpodDevice.cpp

   qmake-qt4 LIBS+=-lX11

   ( cd i18n && lrelease *.ts )

   make

}

package(){
   cd "${srcdir}/lastfm-${orgver}+dfsg1"

   install -d ${pkgdir}/usr/{bin,share/applications,share/pixmaps,lib}
   install -d ${pkgdir}/usr/share/lastfm/{icons,buttons,i18n,services}
   install -d ${pkgdir}/usr/share/kde4/services

   install -m644 i18n/*.qm ${pkgdir}/usr/share/lastfm/i18n

   install -m644 ${srcdir}/lastfm.protocol ${pkgdir}/usr/share/kde4/services
   install -m644 ${srcdir}/lastfm.desktop ${pkgdir}/usr/share/applications

   cd bin

   install -m755 last.fm ${pkgdir}/usr/share/lastfm
   echo -e '#!/bin/sh\nexec /usr/share/lastfm/last.fm\n' > lastfm
   install -m755 lastfm ${pkgdir}/usr/bin

   install -m644 {libLastFmTools.so.1.0.0,libMoose.so.1.0.0} ${pkgdir}/usr/lib
   cp -a libLastFmTools.so libLastFmTools.so.1 libLastFmTools.so.1.0 \
         libMoose.so libMoose.so.1 libMoose.so.1.0 ${pkgdir}/usr/lib

   install -m644 services/* ${pkgdir}/usr/share/lastfm/services

   cd data
   install -m644 about_generic.png app_55.png logo.png no*.gif \
                 progress.mng progress_active.mng slider*.png \
                 speaker*.png watermark.png wizard_generic.png \
                 ${pkgdir}/usr/share/lastfm

   install -m644 buttons/*.png ${pkgdir}/usr/share/lastfm/buttons
   install -m644 icons/*.png ${pkgdir}/usr/share/lastfm/icons
   rm -f ${pkgdir}/usr/share/lastfm/icons/*profile24.png
   install -m644 ${srcdir}/user_*22.png ${pkgdir}/usr/share/lastfm/icons

   install -m644 icons/as.png ${pkgdir}/usr/share/pixmaps/lastfm.png

   cp -a ${srcdir}/icons ${pkgdir}/usr/share/

}
md5sums=('e3cb2da1596f8f7dcec2ce93a2a624c8'
         '488de729055fea7f85dfa3f83da2f57c'
         '5db7a038558c1a72d87f6c3c293e222f'
         '73ecc5f7cf769c404a7d7eb93e3d9cf0'
         '5d461b7a01e41fd1e3e9f2b9098fcb02'
         'd2aabc27ea59f8afc939a7a3018d91bf'
         'b6b4ceec7bf57f005102916a98e85eca'
         '04b0129e637a77d2625981f57f00c9e7'
         '087cff8111dc7b3515009a40e4b1453c'
         'c0f52f311cde53bee1cfbd0517ed99e0'
         'e14aa7c8c2afc02c211e0a5df2c9d494'
         '91c0f8a409399697b44fe8ef4384e350'
         '6a43e1d1aef20fe499ef04ffe07e20c3'
         'd56b0b21bb01f0630e792b7498ba2eb5'
         'c4300a389a1893b53133ed36da731303'
         'bda1ee83334381c3de9e97895107d257'
         'c0df7e112276dda80fa8703de2a8c44b'
         '5f6ad07f54e2bd1c8c86b1293bb2f106'
         '6410eae80c72645ef5630aee03ce40f2'
         'fb450128e3e759d5ee5db8e2cd5f1d45'
         'ad628454145b87babb8f164ff4b27b9b'
         'd54ddff88f824e803d50c8806a807dfe'
         'bb37eae763122e061e5b61ea4584a273'
         '61f9c5194af9fcad330e2cd3a619d9fb'
         'cd5f80f92a8b0ea4030778a8f376c7c5'
         '3d8222174885e338b70a6528e2a9a7e3'
         '00c74c4d1d10d77fb8511df5a2691388'
         'd0e5a0fb91180be4381f646f0eaa725c'
         '8d40a2ec0c2d071d53759d1b08a5efb6')
