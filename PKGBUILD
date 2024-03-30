# Maintainer: Levente Polyak <anthraxx[at]archlinux[dot]org>
# Maintainer: Daniel M. Capella <polyzen@archlinux.org>
# Contributor: Jelle van der Waa <jelle@vdwaa.nl>

# Check compatibility with jedi on potential version bumps
_pkgbase=parso
pkgname=python-parso
pkgver=0.8.3
pkgrel=2
epoch=1
pkgdesc="Python parser that supports error recovery and round-trip parsing for different Python versions"
arch=('any')
url="https://github.com/davidhalter/parso"
license=('MIT')
depends=('python')
makedepends=(
  'python-build'
  'python-installer'
  'python-setuptools'
  'python-sphinx'
  'python-wheel'
)
checkdepends=('python-pytest')
source=("https://github.com/davidhalter/parso/archive/v$pkgver/$_pkgbase-$pkgver.tar.gz")
sha512sums=('7874460053662d40c2cfcf0015e330e1c8201eeb07682e2079a636db553a82dc25b70b14ad0f0c82fb817f655359d695101a17f616abf9a39f49c61ae5fd49b1')
b2sums=('8942408e27198075c92ef51f7a191cc7781eb3a5110119b91fd95f86e13ebe2fbfee11022a2bfec794150f60b3af8c4d5f324cb011703cd581c17f92232ae5bf')

build() {
  cd "$_pkgbase-$pkgver"
  python -m build --wheel --skip-dependency-check --no-isolation
  sphinx-build -b text docs docs/_build/text
  sphinx-build -b man docs docs/_build/man
}

check() {
  cd "$_pkgbase-$pkgver"
  # test_python_excetion_matches broke with 3.10 due to exception formatting changes.
  # https://github.com/davidhalter/parso/issues/192
  pytest test -k 'not test_python_exception_matches'
}

package() {
  cd "$_pkgbase-$pkgver"
  python -m installer --destdir="$pkgdir" dist/*.whl
  install -Dm 644 CHANGELOG.rst README.rst docs/_build/text/*.txt -t "$pkgdir/usr/share/doc/$pkgname"
  install -Dm 644 docs/_build/man/parso.1 "$pkgdir/usr/share/man/man1/$pkgname.1"

  # Symlink license file
  local site_packages=$(python -c "import site; print(site.getsitepackages()[0])")
  install -d "$pkgdir"/usr/share/licenses/$pkgname
  ln -s "$site_packages"/$_pkgbase-$pkgver.dist-info/LICENSE.txt \
    "$pkgdir"/usr/share/licenses/$pkgname/LICENSE.txt
}

# vim: ts=2 sw=2 et:
