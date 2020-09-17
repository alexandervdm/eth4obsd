# $OpenBSD: Makefile

COMMENT =	    generates static library bls386_256 with BLS-ETH=1
VERSION =	    20200722
DISTNAME =	    bls-eth-go
PKGNAME =	    $(DISTNAME)-$(VERSION)

GH_ACCOUNT =	    herumi
GH_PROJECT =	    bls-eth-go-binary
GH_COMMIT =	    41fc56eba7b48e65e410ef11cf5042d62e887017

CATEGORIES =	    security net
HOMEPAGE =	    https://github.com/herumi/bls-eth-go-binary
MAINTAINER =	    Alexander van der Meij <openbsd-ports@vandermeij.tech>

#BSD
PERMIT_PACKAGE =   Yes

WANTLIB +=	    gmpxx
LIB_DEPENDS =	    devel/gmp,-cxx \
		    archivers/unzip
COMPILER =	    base-clang

USE_GMAKE =	    Yes

# NOTE: keep this matched with revisions in bazel build script
BLS_COMMIT =	    5e2af1489a06eddb58869b66a33aa855b27bb41c# 20200721
MCL_COMMIT =	    d4d9091e540f30bccc0493ec246dc03c7fea26dd# 20200721
BLS_SRC =	    https://github.com/herumi/bls/archive/${BLS_COMMIT}.zip
MCL_SRC =	    https://github.com/herumi/mcl/archive/${MCL_COMMIT}.zip

post-extract:
	mkdir ${WRKSRC}/imported && \
	cd ${WRKSRC}/imported && \
	ftp ${BLS_SRC}; unzip ${BLS_COMMIT}.zip; mv bls-${BLS_COMMIT} bls && \
	ftp ${MCL_SRC}; unzip ${MCL_COMMIT}.zip; mv mcl-${MCL_COMMIT} mcl

do-install:
	cd ${WRKSRC} && \
	${INSTALL_DATA} bls/lib/openbsd/amd64/libbls384_256.a ${PREFIX}/lib

post-clean:
	cd ${WRCSRC} && rm -f imported

.include <bsd.port.mk>
