# FreeBSD port of OpenBSD ldapd

ldapd is a daemon which implements version 3 of the LDAP protocol.

A running ldapd process can be controlled using the ldapctl(8) utility.

http://cvsweb.openbsd.org/cgi-bin/cvsweb/src/usr.sbin/ldapd/

## Installation

### Requirements
* libressl

```
git submodule update --init
make
```

## Test

```
pw useradd ldap -d /nonexistent -m -c ldap -s /usr/sbin/nologin
pkg install openldap26-client p5-perl-ldap
cd src/regress/usr.sbin/ldapd/
make
```

## Usage

`ldapd -f etc/examples/ldapd.conf`
