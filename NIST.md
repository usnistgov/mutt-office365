How to enable Mutt to do directory lookups against NIST LDAP
============================================================

By installing and configuring a few additional components you can get NIST directory
lookups working. (This will not work off-campus however since NIST LDAP servers are
not reachable off campus.)

# Install some additional packages

On Ubuntu 18.04:

```
apt install w3m lbdb libnet-ldap-perl libio-socket-ssl-perl finger abook urlview
```

You're on your own for other distros and OSes.

# Configure lbdb

lbdb will be making direct LDAP queries to the NIST LDAP server. It can also fall back on
other lookups (like abook, Mutt's aliases file, and the system `getent` output).

In your home directory you'll be making a `.lbdb` directory with two files:

## .lbdb/lbdbrc
```
METHODS="m_abook m_getent m_muttalias m_ldap"
MAIL_DOMAIN_NAME=nist.gov
MUTT_DIRECTORY="$HOME/.mutt"
MUTTALIAS_FILES=".muttrc .mail_aliases muttrc aliases"
LDAP_NICKS="nist"
```

## .lbdb/ldap.rc
```
%ldap_server_db = (
  'nist'          => ['people.ndir.nist.gov', 'ou=people,dc=ndir,dc=nist,dc=gov',
                      'cn mail', 'cn mail', '${mail}', '${cn}', '', 1],
);
$ldap_bind_dn = '';
$ldap_bind_pw = '';
$ldap_tls = 0;
$ldap_sasl_mech = '';
1;
```

