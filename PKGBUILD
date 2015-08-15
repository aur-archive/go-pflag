# Maintainer: Alexander R�dseth <rodseth@gmail.com>

pkgname=go-pflag
pkgver=r20120510
pkgrel=1
pkgdesc="Parsing flags the GNU way"
arch=('x86_64' 'i686')
url="http://github.com/ogier/pflag"
license=('GPL')
makedepends=('go')
options=('!strip' '!emptydirs')
_gourl=github.com/ogier/pflag

build() {
  cd "$srcdir"
  source /etc/profile.d/go.sh

  rm -rf build
  mkdir -p build/go
  cd build/go

  for f in "$GOROOT/"*; do
    ln -s "$f"
  done

  rm pkg
  mkdir pkg
  cd pkg

  for f in "$GOROOT/pkg/"*; do
    ln -s "$f"
  done

  if [ "$CARCH" = "x86_64" ]; then
    platform=linux_amd64
  else
    platform=linux_386
  fi

  rm "$platform"
  mkdir "$platform"
  cd "$platform"

  for f in "$GOROOT/pkg/$platform/"*.h; do
    ln -s "$f"
  done

  export GOROOT="$srcdir/build/go"
  export GOPATH="$srcdir/build"

  go get -fix "$_gourl"
}

package() {
  cd "$srcdir"
  source /etc/profile.d/go.sh

  # Package go package files
  for f in "$srcdir/build/go/pkg/"* "$srcdir/build/pkg/"*; do
    # If it's a directory
    if [ -d "$f" ]; then
      cd "$f"
      mkdir -p "$pkgdir/$GOROOT/pkg/`basename $f`"
      for z in *; do
        # Check if the directory name matches
        if [ "$z" == `echo $_gourl | cut -d/ -f1` ]; then
          cp -r $z "$pkgdir/$GOROOT/pkg/`basename $f`"
        fi
      done
      cd ..
    fi
  done

  # Package source files
  if [ -d "$srcdir/build/src" ]; then
    mkdir -p "$pkgdir/$GOROOT/src/pkg"
    cp -r "$srcdir/build/src/"* "$pkgdir/$GOROOT/src/pkg/"
    find "$pkgdir" -depth -type d -name .git -exec rm -r {} \;
  fi
}

# vim:set ts=2 sw=2 et:
