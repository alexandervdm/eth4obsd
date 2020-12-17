# $OpenBSD: Makefile

COMMENT =	    pre-build static library bls386_256 with BLS-ETH=1
VERSION =	    20201019
DISTNAME =	    bls-eth-go-binary
PKGNAME =	    $(DISTNAME)-$(VERSION)

GH_ACCOUNT =	    herumi
GH_PROJECT =	    bls-eth-go-binary
GH_COMMIT =	    4b463a10c225efa46f6d7525ef44822e4b5b7b05

CATEGORIES =	    security net
HOMEPAGE =	    https://github.com/herumi/bls-eth-go-binary
MAINTAINER =	    Alexander van der Meij <openbsd-ports@vandermeij.tech>

#BSD
PERMIT_PACKAGE =   Yes

do-build:

# see https://github.com/herumi/bls-eth-go-binary/issues/31
do-install:
	cd ${WRKSRC} && \
	${INSTALL_DATA} bls/lib/linux/amd64/libbls384_256.a ${PREFIX}/lib

.include <bsd.port.mk>
