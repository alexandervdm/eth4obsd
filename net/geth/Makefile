# $OpenBSD: Makefile

# NOTE: based on port by Fabian Raetz at github.com/jasperla/openbsd-wip

COMMENT =	    go implementation of the ethereum protocol
VERSION =	    1.9.23
PKGNAME =	    geth-$(VERSION)
#REVISION =	    0

GH_ACCOUNT =	    ethereum
GH_PROJECT =	    go-ethereum
GH_TAGNAME =	    v$(VERSION)

CATEGORIES =	    net
HOMEPAGE =	    https://geth.ethereum.org/
MAINTAINER =	    Alexander van der Meij <openbsd-ports@vandermeij.tech>

WANTLIB +=	    c pthread

# LGPL-3.0
PERMIT_PACKAGE =    Yes

SUBST_VARS =	    VARBASE

BUILD_DEPENDS =     lang/go14

# NOTE: have to use non-system lang/go for now so will
# have to look into all this fancy stuff later
#MAKE_ENV +=	    CGO_CFLAGS=-I${PREFIX}/include \
#		    CGO_LDFLAGS=-L${PREFIX}/lib
#MODULES =	    lang/go
#MODGO_TYPE =	    bin

do-build:
	# additional binaries in go-ethereum are excluded at this time
	cd ${WRKSRC} && ${MAKE} geth

do-install:
	for b in geth ; do \
	    ${INSTALL_PROGRAM} ${WRKSRC}/build/bin/$$b ${PREFIX}/bin/; \
	done

.include <bsd.port.mk>
