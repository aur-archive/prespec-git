# Maintainer: L.G. Sarmiento (Pico) <Luis.Sarmientop-ala-nuclear.lu.se>

pkgname=prespec-git
pkgver=20130402
pkgrel=1
pkgdesc=""
url="http://www.ikp.tu-darmstadt.de"
arch=('x86_64')
license=('GPL3')
depends=('adflite-git')
makedepends=('git' 'root')
optdepends=('go4: for histograming and condition editing')
options=(!libtool)
## If you plan to access the GSI machines you are supposed to know the password already.
_gitroot='agatagsi@lx-pool.gsi.de:git/prespec'
_gitname='prespec'

CMAKE="yes"
#CMAKE="no"

PULL="yes"
#PULL="no"

build() {
  if [ ${PULL} == "yes" ]; then
    if [ -d ${srcdir}/$_gitname ]; then
      cd ${srcdir}/$_gitname && git pull origin
      msg "The local files are updated."
    else
      cd ${srcdir}
      git clone --depth 1 $_gitroot $_gitname
    fi
    msg "GIT checkout done or server timeout"
  fi

#### CMAKE ####
  if [ ${CMAKE} == "yes" ]; then
    msg "Starting building with cmake..."

    if [ -d ${srcdir}/$_gitname-build ]; then
      rm -rf ${srcdir}/$_gitname-build
    fi

    mkdir ${srcdir}/$_gitname-build
    cd ${srcdir}/$_gitname-build
    cmake -DCMAKE_INSTALL_PREFIX=/usr \
      ${srcdir}/$_gitname
    make
  else
    #### AUTOTOOLS ####
    msg "Starting building with make..."
    if [ -d ${srcdir}/$_gitname-build2 ]; then
      rm -rf ${srcdir}/$_gitname-build2
    fi

    cp -r ${srcdir}/$_gitname ${srcdir}/$_gitname-build2 || return 1

    cd ${srcdir}/$_gitname-build2
    ./autogen.sh #&> autogen_autotool.log
    ./configure --prefix=/usr #&> configure_autotool.log
    make #&> make_autotool.log
  fi
}

package() {
  if [ ${CMAKE} == "yes" ]; then
    msg  "Starting cmake install"
    cd ${srcdir}/$_gitname-build
    make DESTDIR="$pkgdir" install
  else
    msg "Starting autotools install"
    cd ${srcdir}/$_gitname-build2
    make DESTDIR="$pkgdir/" install #&> install_autotool.log
  fi
}
