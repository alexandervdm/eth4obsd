# $OpenBSD: Makefile

COMMENT =	    Ethereum 2.0 Rust based client maintained by Sigma Prime.
VERSION =	    0.2.9
#REVISION =	    0

GH_ACCOUNT =	    sigp
GH_PROJECT =	    lighthouse
GH_TAGNAME =	    v$(VERSION)

CATEGORIES =	    net
HOMEPAGE =	    https://lighthouse.sigmaprime.io/
MAINTAINER =	    Alexander van der Meij <openbsd-ports@vandermeij.tech>

# APACHE-2.0
PERMIT_PACKAGE =    Yes

SUBST_VARS =	    VARBASE
USE_GMAKE =	    Yes

BUILD_DEPENDS =    lang/rust \
		    devel/cmake \
		    devel/protobuf-c

CONFIGURE_ENV =    CARGO_HOME=${WRKBUILD}/cargo-home

do-build:
	cd ${WRKSRC} && OPENSSL_NO_VENDOR=1 \
		RUSTFLAGS="-C default_linker_libraries" \
		${GMAKE}

do-install:
	${INSTALL_PROGRAM} ${WRKBUILD}/target/release/lighthouse ${PREFIX}/bin/

.include <bsd.port.mk>