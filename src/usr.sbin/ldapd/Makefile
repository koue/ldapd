#	$OpenBSD: Makefile,v 1.17 2020/09/19 09:46:04 tb Exp $

LOCALBASE?=	/usr/local
EVENTDIR?=	${.CURDIR}/../../../libevent
IMSGDIR?=	${.CURDIR}/../../../libimsg

PROG=		ldapd
MAN=		ldapd.8 ldapd.conf.5
SRCS=		log.c logmsg.c control.c \
		util.c ldapd.c ldape.c conn.c attributes.c namespace.c \
		btree.c filter.c search.c parse.y \
		auth.c modify.c index.c evbuffer_tls.c \
		validate.c uuid.c schema.c imsgev.c syntax.c matching.c

.PATH: ${.CURDIR}/../../lib/libutil
SRCS+=		ber.c

.PATH: ${.CURDIR}/../../lib/libc/gen
SRCS+=		auth_subr.c authenticate.c

LDADD=		${EVENTDIR}/src/lib/libevent/libevent.a
LDADD+= 	${IMSGDIR}/src/lib/libutil/libimsg.a

.ifdef SSLDIR
LDADD+= 	${SSLDIR}/tls/.libs/libtls.a \
		${SSLDIR}/ssl/.libs/libssl.a \
		${SSLDIR}/crypto/.libs/libcrypto.a
.else
LDADD+= 	/usr/local/lib/libtls.a \
		/usr/local/lib/libssl.a \
		/usr/local/lib/libcrypto.a
.endif

LDADD+= 	-lcrypt -lz -lutil
DPADD=		${LIBEVENT} ${LIBTLS} ${LIBSSL} ${LIBCRYPTO} ${LIBZ} ${LIBUTIL}
CFLAGS+=	-I${.CURDIR} -g
CFLAGS+=	-Wall -Wstrict-prototypes -Wmissing-prototypes
CFLAGS+=	-Wmissing-declarations
CFLAGS+=	-Wshadow -Wpointer-arith -Wcast-qual
CFLAGS+=	-Wsign-compare -Werror=implicit-function-declaration
CFLAGS+=	-I${EVENTDIR}/src/lib/libevent \
		-I${IMSGDIR}/src/lib/libutil \
		-I${.CURDIR}/../../lib/libutil \
		-I${.CURDIR}/../../include

.ifdef SSLDIR
CFLAGS+=	-I${SSLDIR}/include
.else
CFLAGS+=	-I${LOCALBASE}/include
.endif

CFLAGS+=	-D__dead=''

CLEANFILES+=	y.tab.h parse.c

SCHEMA_FILES=	bsd.schema \
		core.schema \
		inetorgperson.schema \
		nis.schema

distribution:
	for i in ${SCHEMA_FILES}; do \
		${INSTALL} -C -o root -g wheel -m 0644 ${.CURDIR}/schema/$$i ${DESTDIR}/etc/ldap/; \
	done

.include <bsd.prog.mk>
