# Contributor: Bjoern Lindig <bjoern at googlemail dot com>
pkgname=mhwaveedit-git
pkgver=20120313
pkgrel=1
pkgdesc="A graphical program for editing, playing and recording sound files."
arch=('i686' 'x86_64')
url="https://gna.org/projects/mhwaveedit"
license=('GPL')
depends=('sdl' 'jack' 'gtk2')
makedepends=('git')
optdepends=('libpulse' 'esound')
provides=('mhwaveedit')
conflicts=('mhwaveedit')
replaces=('mhwaveedit')
install=
noextract=()
md5sums=() #generate with 'makepkg -g'

_gitroot="git://github.com/magnush/mhwaveedit.git"
_gitname="mhwaveedit"

build() {
  cd "$srcdir"
  msg "Connecting to GIT server...."

  if [ -d $_gitname ] ; then
    cd $_gitname && git pull origin
    msg "The local files are updated."
  else
    #mkdir $_gitname
    git clone $_gitroot $_gitname
  fi

  msg "GIT checkout done or server timeout"
  msg "Starting make..."

  rm -rf "$srcdir/$_gitname-build"
  #mkdir "$srcdir/$_gitname-build"
  git clone "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  cd "$srcdir/$_gitname-build"

  #
  # BUILD HERE
  #

  cd "$srcdir/$_gitname-build/docgen"
  bash gendocs.sh 
  cd "$srcdir/$_gitname-build/"
  aclocal -I m4
  autoheader
  automake
  autoconf
  export CFLAGS="$CFLAGS -laudiofile"
  ./configure --prefix=/usr
  make -C po mhwaveedit.pot-update
  make || return 1
  make DESTDIR="$pkgdir/" install
} 
