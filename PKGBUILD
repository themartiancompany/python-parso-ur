# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2024, 2025  Pellegrino Prevete
#
#    All rights reserved
#    ----------------------------------------------------------------------
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Affero General Public License for more details.
#
#    You should have received a copy of the GNU Affero General Public License
#    along with this program.  If not, see <https://www.gnu.org/licenses/>.

# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Truocolo <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
# Maintainer: Pellegrino Prevete (dvorak) <pellegrinoprevete@gmail.com>
# Maintainer: Pellegrino Prevete (dvorak) <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Maintainer: Levente Polyak <anthraxx[at]archlinux[dot]org>
# Maintainer: Daniel M. Capella <polyzen@archlinux.org>
# Contributor: Jelle van der Waa <jelle@vdwaa.nl>

# Check compatibility with jedi on potential version bumps
if [[ ! -v "_docs" ]]; then
  _docs="false"
fi
_pkg=parso
_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
pkgbase="${_py}-${_pkg}"
pkgname=(
  "${pkgbase}"
)
if [[ "${_docs}" == "true" ]]; then
  pkgname+=(
    "${pkgbase}-docs"
  )
fi
pkgver=0.8.4
pkgrel=3
epoch=1
_pkgdesc=(
  "Python parser that supports error"
  "recovery and round-trip parsing"
  "for different Python versions"
)
arch=(
  'any'
)
_http="https://github.com"
_ns="davidhalter"
url="${_http}/${_ns}/${_pkg}"
license=(
  'MIT'
)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
)
makedepends=(
  "${_py}-build"
  "${_py}-installer"
  "${_py}-setuptools"
  "${_py}-wheel"
)
if [[ "${_docs}" == "true" ]]; then
  makedepends+=(
    "${_py}-sphinx"
  )
fi
checkdepends=(
  "${_py}-pytest"
)
_url="${url}"
source=(
  "${_url}/archive/v${pkgver}/${_pkg}-${pkgver}.tar.gz"
)
sha512sums=(
  'da96f0ab6cfbcf2a54ee73262a672bb4d9720aa91fd884a8c17165d597eece97569b7ee87fd7ea1c0be663c0cb2930a66a03b4e305070f59f346485817607db3'
)
b2sums=(
  '5a8a81f64b20b52cd3349b7bc059621733debfaf5cc271f3e89423d63e4af67391f7740c34b450b2a91fafe34b8986926e8f7c4ca0b8600eafd0171c01e664b0'
)

build() {
  cd \
    "${_pkg}-${pkgver}"
  "${_py}" \
    -m \
      "build" \
      --wheel \
      --skip-dependency-check \
      --no-isolation
  if [[ "${_docs}" == "true" ]]; then
    sphinx-build \
      -b \
        "text" \
      "docs" \
      "docs/_build/text"
    sphinx-build \
      -b \
        "man" \
      "docs" \
      "docs/_build/man"
  fi
}

check() {
  cd \
    "${_pkg}-${pkgver}"
  # test_python_excetion_matches
  # broke with 3.10 due to exception formatting changes.
  # https://github.com/davidhalter/parso/issues/192
  pytest \
    test \
    -k \
      'not test_python_exception_matches'
}

package() {
  cd \
    "${_pkg}-${pkgver}"
  "${_py}" \
    -m \
      "installer" \
      --destdir="${pkgdir}" \
      "dist/"*".whl"
  install \
    -Dm644 \
    "README.rst" \
    "CHANGELOG.rst" \
    -t \
    "${pkgdir}/usr/share/doc/${pkgname}"
}

package_python-parso-docs() {
  local \
    _site_packages
  _site_packages="$( \
    "${_py}" \
      -c \
        "import site; print(site.getsitepackages()[0])")"
  install \
    -Dm644 \
    "docs/_build/text/"*".txt" \
    -t \
    "${pkgdir}/usr/share/doc/${pkgname}"
  install \
    -Dm644 \
    "docs/_build/man/${_pkg}.1" \
    "${pkgdir}/usr/share/man/man1/${pkgname}.1"
  # Symlink license file
  install \
    -d \
      "${pkgdir}/usr/share/licenses/${pkgname}"
  ln \
    -s "${_site_packages}/${_pkg}-${pkgver}.dist-info/LICENSE.txt" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE.txt"
}

# vim: ts=2 sw=2 et:
