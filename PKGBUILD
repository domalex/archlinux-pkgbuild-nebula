# Maintainer: Dominik Schmid <niggi@cyberdude.com>
pkgname='nebula'
pkgver=1.7.2
pkgrel=1
pkgdesc='A scalable overlay networking tool with focus on performance, simplicity and security'
arch=('any')
url='https://github.com/slackhq/nebula'
license=('MIT')
depends=()
makedepends=('go')
options=('!lto')
source=("${pkgname}-${pkgver}.tar.gz::${url}/archive/v${pkgver}.tar.gz")
sha256sums=('c4771ce6eb3e142f88f5f4c12443cfca140bf96b2746c74f9536bd1a362f3f88')
# Generate new checksums after altering PKGBUILD
# cd into folder of PKGBUILD and execute 'make -gfp PKGBUILD
# copy new checksum into PKGBUILD

build() {
  cd "${pkgname}-${pkgver}"

  export CGO_CPPFLAGS="${CPPFLAGS}"
  export CGO_CFLAGS="${CFLAGS}"
  export CGO_CXXFLAGS="${CXXFLAGS}"
  export CGO_LDFLAGS="${LDFLAGS}"
  export GOFLAGS="-buildmode=pie -trimpath -mod=readonly -modcacherw"

  for bin in nebula{,-cert,-service}
  do go build \
    -ldflags "-X main.Build=${pkgver}" \
    -o "${bin}" "./cmd/${bin}"
  done
}

check() {
  cd "${pkgname}-${pkgver}"

  go test -v ./...
}

package() {
  cd "${pkgname}-${pkgver}"

  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
  install -Dm644 dist/arch/nebula.service "${pkgdir}/usr/lib/systemd/system/nebula.service"

  for bin in nebula{,-cert,-service}
  do install -Dm755 "${bin}" "${pkgdir}/usr/bin/${bin}"
  done
}
