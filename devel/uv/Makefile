# ring-v0.17 does not support sparc64
NOT_FOR_ARCHS =	sparc64

COMMENT =	extremely fast Python package installer and resolver

GH_ACCOUNT =	astral-sh
GH_PROJECT =	uv
GH_TAGNAME =	0.1.42

# Rust crates downloaded via git
PUBGRUB_HASH =	0e684a874c9fb8f74738cd8875524c80e3d4820b
ASYNC_HASH =	1dcb40cfe1bf5325a6fd4bfcf9894db40241f585

DIST_TUPLE +=	github astral-sh pubgrub ${PUBGRUB_HASH} ${MODCARGO_VENDOR_DIR}
DIST_TUPLE +=	github charliermarsh rs-async-zip ${ASYNC_HASH} \
		${MODCARGO_VENDOR_DIR}

CATEGORIES =	devel

HOMEPAGE =	https://github.com/astral-sh/uv

MAINTAINER =	Laurent Cheylus <foxy@free.fr>

# Apache 2.0, MIT
PERMIT_PACKAGE =	Yes

BUILD_DEPENDS +=	devel/cargo-generate-vendor

LIB_DEPENDS +=	security/libssh2 devel/libgit2/libgit2 \
		archivers/bzip2 archivers/zstd

WANTLIB +=	${MODCARGO_WANTLIB} m crypto ssl git2 ssh2 bz2 zstd

MODULES =	devel/cargo

CONFIGURE_STYLE =	cargo

SEPARATE_BUILD =	Yes

SUBST_VARS +=	MODCARGO_VENDOR_DIR

post-patch:
	${SUBST_CMD} ${WRKSRC}/Cargo.toml

	${LOCALBASE}/bin/cargo-generate-vendor \
		${FULLDISTDIR}/astral-sh-pubgrub-${PUBGRUB_HASH}.tar.gz \
		${MODCARGO_VENDOR_DIR}/pubgrub-${PUBGRUB_HASH}
	${LOCALBASE}/bin/cargo-generate-vendor ${FULLDISTDIR}/charliermarsh-$\
		rs-async-zip-${ASYNC_HASH}.tar.gz \
		${MODCARGO_VENDOR_DIR}/rs-async-zip-${ASYNC_HASH}

# Generate shell completions
post-build:
	${MODCARGO_TARGET_DIR}/release/uv -n --generate-shell-completion bash \
		> ${WRKDIR}/uv.bash
	${MODCARGO_TARGET_DIR}/release/uv -n --generate-shell-completion fish \
		> ${WRKDIR}/uv.fish
	${MODCARGO_TARGET_DIR}/release/uv -n --generate-shell-completion zsh \
		> ${WRKDIR}/uv.zsh

do-install:
	${INSTALL_PROGRAM} ${MODCARGO_TARGET_DIR}/release/uv ${PREFIX}/bin/

	${INSTALL_DATA_DIR} ${PREFIX}/share/bash-completion/completions/
	${INSTALL_DATA} ${WRKDIR}/uv.bash \
		${PREFIX}/share/bash-completion/completions/uv
	${INSTALL_DATA_DIR} ${PREFIX}/share/fish/completions/
	${INSTALL_DATA} ${WRKDIR}/uv.fish ${PREFIX}/share/fish/completions/
	${INSTALL_DATA_DIR} ${PREFIX}/share/zsh/site-functions/
	${INSTALL_DATA} ${WRKDIR}/uv.zsh ${PREFIX}/share/zsh/site-functions/_uv

.include "crates.inc"

.include <bsd.port.mk>
