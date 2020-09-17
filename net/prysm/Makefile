# $OpenBSD: Makefile

COMMENT =	    Go implementation of the Ethereum 2.0 blockchain
VERSION =	    1.0.0-alpha.25
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

COMPONENTS = beacon-chain slasher validator

do-build:
.for C in ${COMPONENTS}
	cd ${WRKSRC}/${C} && ${MODGO_CMD} CGO_LDFLAGS=-L${PREFIX}/lib go build -o ${C} main.go
.endfor

do-install:
.for C in ${COMPONENTS}
	${INSTALL_PROGRAM} ${WRKSRC}/${C}/${C} ${PREFIX}/bin/
.endfor

.include <bsd.port.mk>
