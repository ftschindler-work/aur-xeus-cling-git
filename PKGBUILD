# Maintainer: Felix Schindler <aur at felixschindler dot net>
# Contributor: Sebastian Gsänger <sebastian_gsaenger@web.de>

pkgname=xeus-cling-git
_pkgname=xeus-cling
pkgver=0.10.0.r3.g7197375
pkgrel=1
pkgdesc="A C++ jupyter kernel based on xeus and cling"
arch=('x86_64')
url="https://github.com/jupyter-xeus/xeus-cling"
license=('BSD')
provides=("$_pkgname=$pkgver")
conflicts=("$_pkgname")
depends=('xtl' 'xproperty' 'xeus' 'nlohmann-json'
         'jupyter' 'jupyter-widgetsnbextension'
         'cling-dev-git' 'pugixml' 'cxxopts')
makedepends=('cmake' 'cppzmq')
source=("${_pkgname}::git+https://github.com/jupyter-xeus/${_pkgname}.git#branch=master"
        "0001-Rename-kernels.patch"
        "0002-Use-llvm-from-cling.patch")
md5sums=('SKIP'
         'e03c81c3701e4f0dea249955051c2ddf'
         '30e86ddf8e4996352eb456e12ec686bb')

pkgver() {
  # According to https://wiki.archlinux.org/index.php/VCS_package_guidelines,
  # we are aiming for RELEASE.rREVISION (or rather RELEASE.rREVISION.GIT_HASH).
  cd "${srcdir}"/${pkgname%-git}
  git describe --tags | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
    cd $_pkgname

    local src
    for src in "${source[@]}"; do
        src="${src%%::*}"
        src="${src##*/}"
        [[ $src = *.patch ]] || continue
        echo "Applying patch $src..."
        patch -Np1 < "../$src"
    done
}

build() {
    cd $_pkgname
    mkdir -p build
    cd build

    cmake \
        -DCMAKE_INSTALL_PREFIX=/usr \
        -DCMAKE_BUILD_TYPE=Release \
        -DClang_DIR=/opt/cling/lib/cmake/clang \
        ..

    cmake --build .
}

package() {
    cd $_pkgname

    install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$_pkgname/LICENSE"

    cd "build"

    DESTDIR="$pkgdir" cmake --build . -t install
}
