# $OpenBSD: Makefile

COMMENT =	    Go implementation of the Ethereum 2.0 blockchain
VERSION =	    1.0.0-alpha.29
PKGNAME =	    prysm-$(VERSION:S/-/./)
#REVISION =	    0

GH_ACCOUNT =	    prysmaticlabs
GH_PROJECT =	    prysm
GH_TAGNAME =	    v$(VERSION)

CATEGORIES =	    net
HOMEPAGE =	    https://prysmaticlabs.com/
MAINTAINER =	    Alexander van der Meij <openbsd-ports@vandermeij.tech>

# GPL-3.0
PERMIT_PACKAGE =   Yes

SUBST_VARS =	    VARBASE

BUILD_DEPENDS =    lang/go14 \
		    security/bls-eth-go

# NOTE: have to use non-system lang/go for now so will
# have to look into all this fancy stuff later
#MAKE_ENV +=	    CGO_CFLAGS=-I${PREFIX}/include \
#		    CGO_LDFLAGS=-L${PREFIX}/lib
#MODULES =	    lang/go
#MODGO_TYPE =	    bin

do-build:
	cd ${WRKSRC}/beacon-chain && \
		CGO_LDFLAGS=-L${PREFIX}/lib go build -o prysm_beacon main.go && \
	cd ${WRKSRC}/slasher && \
		CGO_LDFLAGS=-L${PREFIX}/lib go build -o prysm_slasher main.go && \
	cd ${WRKSRC}/validator && \
		CGO_LDFLAGS=-L${PREFIX}/lib go build -o prysm_validator main.go

do-install:
	${INSTALL_PROGRAM} ${WRKSRC}/beacon-chain/prysm_beacon ${PREFIX}/bin/
	${INSTALL_PROGRAM} ${WRKSRC}/slasher/prysm_slasher ${PREFIX}/bin/
	${INSTALL_PROGRAM} ${WRKSRC}/validator/prysm_validator ${PREFIX}/bin/

.include <bsd.port.mk>
