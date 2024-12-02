# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Caleb Maclennan <caleb@alerque.com>
# Contributor: Kyle Keen <keenerd@gmail.com>
# Contributor: Lukas Fleischer <lfleischer@archlinux.org>
# Contributor: Eric Johnson <eric@coding-zone.com>

_os="$( \
  uname \
    -o)"
pkgname=words
pkgver=2.1
pkgrel=8
pkgdesc="A collection of International 'words' files for /usr/share/dict"
arch=(
  any
)
_url='https://ftp.gnu.org/gnu/aspell/dict'
url="${_url}/0index.html"
license=(
  "GPL-2.0-only"
  "LicenseRef-Ispell"
  "LicenseRef-SCOWL"
)
makedepends=(
  "aspell"
  "glibc"
)
if [[ "${_os}" == "Android" ]]; then
  makedepends+=(
    "iconv"
  )
fi
install=words.install
source=(
  "${_url}/en/aspell6-en-2020.12.07-0.tar.bz2"{,.sig}
  "${_url}/ca/aspell6-ca-2.1.5-1.tar.bz2"{,.sig}
  "${_url}/fi/aspell6-fi-0.7-0.tar.bz2"{,.sig}
  "${_url}/fr/aspell-fr-0.50-3.tar.bz2"
  "${_url}/de-alt/aspell6-de-alt-2.1-1.tar.bz2"{,.sig}
  "${_url}/it/aspell6-it-2.2_20050523-0.tar.bz2"{,.sig}
  "${_url}/de/aspell-de-0.50-2.tar.bz2"
  "${_url}/es/aspell6-es-1.11-2.tar.bz2"{,.sig})
sha256sums=(
  '4c8f734a28a088b88bb6481fcf972d0b2c3dc8da944f7673283ce487eac49fb3'
  'SKIP'
  'ebdae47edf87357a4df137dd754737e6417452540cb1ed34b545ccfd66f165b9'
  'SKIP'
  'f8d7f07b4511e606eb56392ddaa76fd29918006331795e5942ad11b510d0a51d'
  'SKIP'
  'f9421047519d2af9a7a466e4336f6e6ea55206b356cd33c8bd18cb626bf2ce91'
  '36d13c6c743a6b1ff05fb1af79134e118e5a94db06ba40c076636f9d04158c73'
  'SKIP'
  '3b19dc709924783c8d87111aa9653dc6c000e845183778abee750215d83aaebd'
  'SKIP'
  'f1b6f23d694fc12da193de5d5d2232797e87aecf684d8aa5872d83176eeb84ba'
  'ad367fa1e7069c72eb7ae37e4d39c30a44d32a6aa73cedccbd0d06a69018afcc'
  'SKIP'
)
validpgpkeys=(
  "78DEC3DB0AEA763951322BBF71C636695B147849"
  "31A768B0808E4BA619B48F81B6D9D0CC38B327D7"
)

# sort specified from FS#47262
_merge_and_sort() {
  local \
    _out="${1}" \
    _file="${2}" \
    _files=() \
    _all=()
  shift \
    2
  _files=(
    "$@"
  )
  _all=(
    "${_file}"
  )
  if [[ "${_files[*]}" != "" ]]; then
    for _f in "${_files[@]}"; do
      _all+=(
        "${_f}"*
      )
    done
  fi
  cat \
    "${_all[@]}" | \
    sort \
      -u | \
      LC_ALL=C \
      sort \
        -df > \
        "${_out}"
}

build() {
  local \
    _cr \
    _wl \
    _ver
  mkdir \
    -p \
    "${srcdir}/${pkgname}"
  cd \
    "${srcdir}/${pkgname}"
  find \
    "${srcdir}" \
    -name \
      '*.cwl' \
    -not \
    -path \
      "${srcdir}/${pkgname}/*" \
    -exec \
      cp \
        -u \
	'{}' \
	'./' \;
  preunzip \
    *.cwl
  for _wl in *.wl; do
    iconv \
      --from-code=ISO-8859-1 \
      --to-code=UTF-8 \
      "${_wl}" | \
      cut \
        -d \
	  '/' \
	-f \
	  1 | \
	LC_ALL=C \
	sort \
	  -df > \
	  ${_wl}.utf8
  done
  rm \
    *.wl
  mkdir \
    -p \
    "copy"
    # while read cr; do
    #     ver=$(cut -d '-' -f 2 <<< "$cr")
    #     cp "$cr" "copy/copyright-$ver"
    # done <<< "$(find "$srcdir" -name 'Copyright')"
  while read _cr; do
    _ver="$( \
      echo \
        "${_cr}" | \
        rev | \
          cut \
            -d \
              '-' \
            -f \
              2 |
            rev)"
    cp \
      "${_cr}" \
      "copy/copyright-${_ver}"
  done <<< "$( \
    find \
      "${srcdir}" \
      -name \
        'Copyright')"
  chmod \
    644 \
    "copy/"*
  # locale specific sort for other languages?
  # sort specified from FS#47262
  ls
  _merge_and_sort \
    "us-merged" \
    "en-common.wl.utf8" \
    "en_US"
  _merge_and_sort \
    "gb-merged" \
    "en-common.wl.utf8" \
    "en_GB"
  _merge_and_sort \
    "de-merged" \
    "de-only.wl.utf8" \
    "de_"
  _merge_and_sort \
    "it-merged" \
    "it.wl.utf8"
  _merge_and_sort \
    "es-merged" \
    "es.wl.utf8"
  _merge_and_sort \
    "fi-merged" \
    "fi.wl.utf8"
  _merge_and_sort \
    "fr-merged" \
    "fr-40-only.wl.utf8" \
    "fr_"
  _merge_and_sort \
    "ca-merged" \
    "ca-common.wl.utf8"
}

package() {
  cd \
    "$srcdir/$pkgname"
  install \
    -Dm644 \
    "us-merged" \
    "${pkgdir}/usr/share/dict/american-english"
  install \
    -Dm644 \
    "gb-merged" \
    "${pkgdir}/usr/share/dict/british-english"
  ln \
    -s \
    "american-english" \
    "${pkgdir}/usr/share/dict/usa"
  ln \
    -s \
    "british-english" \
    "${pkgdir}/usr/share/dict/british"
  install \
    -Dm644 \
    "it-merged" \
    "${pkgdir}/usr/share/dict/italian"
  install \
    -Dm644 \
    "fr-merged" \
    "${pkgdir}/usr/share/dict/french"
  install \
    -Dm644 \
    "fi-merged" \
    "${pkgdir}/usr/share/dict/finnish"
  install \
    -Dm644 \
    "ca-merged" \
    "${pkgdir}/usr/share/dict/catala"
  ln \
    -s \
    "catala" \
    "${pkgdir}/usr/share/dict/catalan"
  install \
    -Dm644 \
    "de-merged" \
    "${pkgdir}/usr/share/dict/ngerman"
  install \
    -Dm644 \
    "de-alt.wl.utf8" \
    "${pkgdir}/usr/share/dict/ogerman"
  ln \
    -s \
    "ngerman" \
    "${pkgdir}/usr/share/dict/german"
  install \
    -d \
    "${pkgdir}/usr/share/licenses/${pkgname}/"
  cp \
    "copy/"* \
    "${pkgdir}/usr/share/licenses/${pkgname}/"
}
