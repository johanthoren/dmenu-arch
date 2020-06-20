# -*- mode: shell-script;-*-
# Maintainer: Johan Thorén <johan@thoren.xyz>
pkgname=dmenu-jt
_pkgname=dmenu
builddir="$(pwd)"
pkgver=4.9.6.g9b38fda
pkgrel=1
pkgdesc="A dynamic menu for X - Patched by Johan Thorén"
url="http://tools.suckless.org/dmenu/"
arch=('i686' 'x86_64')
license=('MIT')
options=(zipman)
depends=('sh' 'libxinerama' 'libxft')
makedepends=('git')
provides=('dmenu')
conflicts=('dmenu')
source=(dmenu-caseinsensitive.diff
        dmenu-fuzzymatch.diff
        dmenu-fuzzyhighlight.diff
        "$_pkgname::git+https://git.suckless.org/dmenu")
md5sums=(
    '45abef30f540d2c800294255a26c9397'
    '0e35d7b332e741efe0cd989bc36419f8'
    'b4c6d5b94d0fa4f6e92bd93567607ebf'
    'SKIP')

pkgver() {
    cd $_pkgname
    git describe --tags --long | sed 's/-/./g'
}

prepare() {
  cd $_pkgname
  echo "Adding caseinsensitive patch:"
  patch --forward --strip=1 --input="${srcdir}/dmenu-caseinsensitive.diff"
  echo ""

  echo "Adding fuzzymatch patch:"
  patch --forward --strip=1 --input="${srcdir}/dmenu-fuzzymatch.diff"
  echo ""

  echo "Adding fuzzyhighlight patch:"
  patch --forward --strip=1 --input="${srcdir}/dmenu-fuzzyhighlight.diff"
  echo ""

  # This package provides a mechanism to provide a custom config.h. Multiple
  # configuration states are determined by the presence of two files in
  # $_pkgname:
  #
  # config.h  config.def.h  state
  # ========  ============  =====
  # absent    absent        Initial state. The user receives a message on how
  #                         to configure this package.
  # absent    present       The user was previously made aware of the
  #                         configuration options and has not made any
  #                         configuration changes. The package is built using
  #                         default values.
  # present                 The user has supplied his or her configuration. The
  #                         file will be copied to $srcdir and used during
  #                         build.
  #
  # After this test, config.def.h is copied from $srcdir/src/st to $srcdir to
  # provide an up to date template for the user.
  if [ -e "${builddir}/config.h" ]
  then
    cp "${builddir}/config.h" "${srcdir}/${_pkgname}/config.h"
  elif [ ! -e "${builddir}/config.def.h" ]
  then
    msg='This package can be configured in config.h. Move the config.def.h '
    msg+='that was just placed into the package directory to config.h and '
    msg+='modify it to change the configuration. Or just leave it alone to '
    msg+='continue to use default values.'
    warning "$msg"
  fi
  cp "${srcdir}/${_pkgname}/config.def.h" "${builddir}/config.def.h"
}

build() {
  cd $_pkgname
  make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
  cd $_pkgname
  make PREFIX=/usr DESTDIR="$pkgdir" install
  install -m644 -D LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -m644 -D README "$pkgdir/usr/share/doc/$pkgname/README"
}

# vim:set ts=2 sw=2 et:
