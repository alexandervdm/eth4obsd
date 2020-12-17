# $OpenBSD: Makefile

COMMENT =	    Go implementation of the Ethereum 2.0 blockchain
VERSION =	    1.0.5
PKGNAME =	    prysm-$(VERSION)
#REVISION =	    1

GH_ACCOUNT =	    prysmaticlabs
GH_PROJECT =	    prysm
GH_TAGNAME =	    v$(VERSION)

CATEGORIES =	    net
HOMEPAGE =	    https://prysmaticlabs.com/
MAINTAINER =	    Alexander van der Meij <openbsd-ports@vandermeij.tech>

# GPL-3.0
PERMIT_PACKAGE =    Yes

SUBST_VARS =	    VARBASE

BUILD_DEPENDS =     lang/go-vdm \
		    security/bls-eth-go-binary

PRYSM_CGO=CGO_LDFLAGS=-L${PREFIX}/lib CGO_CFLAGS="-D__BLST_PORTABLE__"
PRYSM_TAGS=blst_enabled

pre-build:
	sed -i 's,Unknown,${GH_TAGNAME},' ${WRKSRC}/shared/version/version.go
	sed -i 's,Local build,OpenBSD,' ${WRKSRC}/shared/version/version.go

do-build:
	cd ${WRKSRC}/beacon-chain && \
		${PRYSM_CGO} go build -tags ${PRYSM_TAGS} -o prysm_beacon main.go && \
	cd ${WRKSRC}/slasher && \
		${PRYSM_CGO} go build -tags ${PRYSM_TAGS} -o prysm_slasher main.go && \
	cd ${WRKSRC}/validator && \
		${PRYSM_CGO} go build -tags ${PRYSM_TAGS} -o prysm_validator main.go

do-install:
	${INSTALL_PROGRAM} ${WRKSRC}/beacon-chain/prysm_beacon ${PREFIX}/bin/
	${INSTALL_PROGRAM} ${WRKSRC}/slasher/prysm_slasher ${PREFIX}/bin/
	${INSTALL_PROGRAM} ${WRKSRC}/validator/prysm_validator ${PREFIX}/bin/

	${INSTALL_DATA_DIR} ${PREFIX}/share/examples/prysm/
	${INSTALL_DATA} ${FILESDIR}/*.yaml ${PREFIX}/share/examples/prysm/

do-test:
	cd ${WRKSRC} && \
		${PRYSM_CGO} go test -v -tags ${PRYSM_TAGS} ./validator/...

.include <bsd.port.mk>
