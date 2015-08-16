pkgname=mutt-sidebar-trash
_pkgname=mutt
pkgver=1.5.21
pkgrel=8
pkgdesc='Small but very powerful text-based mail client with sidebar, trash folder and purge message patches.'
url='http://www.mutt.org/'
license=('GPL')
backup=('etc/Muttrc')
arch=('i686' 'x86_64')
optdepends=('smtp-forwarder: to send mail')
depends=('gpgme' 'ncurses' 'openssl' 'libsasl' 'gdbm' 'libidn' 'mime-types' 'krb5')
provides=('mutt')
conflicts=('mutt')
source=("ftp://ftp.mutt.org/mutt/devel/${_pkgname}-${pkgver}.tar.gz"
        'pop-cachedir.patch'
        'crypt-gpgme.patch'
	'trash_folder.patch'
	'purge_message.patch'
	'sidebar.patch')

sha1sums=('a8475f2618ce5d5d33bff85c0affdf21ab1d76b9'
          '32dbcafe05e4a919fd00c6a1064c6ecd205e6eb1'
          '8cf0d4278aab1d162ab7f847f1b917327d630d98'
	  '9de16f379d7583a55e48623ea052cd28ed32eecf'
	  '4f894278370edfa1498c5121e0ab073d80500829'
	  '51002d856d6a6816e7971826e9b9bfb241840e61')

install=install

build() {
	cd "${srcdir}/${_pkgname}-${pkgver}"
	patch -i ../pop-cachedir.patch # FS#31536
	patch -p1 -i ../crypt-gpgme.patch # FS#31735

	patch -p1 -i "../trash_folder.patch"
	patch -p1 -i "../purge_message.patch"
	patch -p1 -i "../sidebar.patch"

	./configure \
		--prefix=/usr \
		--sysconfdir=/etc \
		--enable-gpgme \
		--enable-pop \
		--enable-imap \
		--enable-smtp \
		--enable-hcache \
		--with-curses=/usr \
		--with-regex \
		--with-gss=/usr \
		--with-ssl=/usr \
		--with-sasl \
		--with-idn \

	make
}

package() {
	cd "${srcdir}/${_pkgname}-${pkgver}"
	make DESTDIR="${pkgdir}" install

	rm "${pkgdir}"/usr/bin/{flea,muttbug}
	rm "${pkgdir}"/usr/share/man/man1/{flea,muttbug}.1
	rm "${pkgdir}"/etc/mime.types{,.dist}
	install -Dm644 contrib/gpg.rc "${pkgdir}"/etc/Muttrc.gpg.dist
}
