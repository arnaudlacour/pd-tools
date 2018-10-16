# Description
This project captures files that can make day to day operations on the Ping tool a bit easier

# Assumptions
The main assumption that is made by these tools is that `config/tools.properties` file is populated such that ldpasearch, ldapmodify and ldapdelete will not prompt for connection information.

# Examples
## Listing (similar to Unix ls)
Getting the base entry:
```$ bin/ldap-ls
dc=example,dc=com
```

Listing children entries:
```$ bin/ldap-ls ou=people,dc=example,dc=com
uid=user.0,ou=People,dc=example,dc=com
uid=user.1,ou=People,dc=example,dc=com
uid=user.2,ou=People,dc=example,dc=com
uid=user.3,ou=People,dc=example,dc=com
uid=user.4,ou=People,dc=example,dc=com
uid=user.5,ou=People,dc=example,dc=com
uid=user.6,ou=People,dc=example,dc=com
uid=user.7,ou=People,dc=example,dc=com
```

Listing with details:
This output will include creation and modification information if available for each listing.
```
$ bin/ldap-ls -l ou=people,dc=example,dc=com
uid=user.0,ou=People,dc=example,dc=com
uid=user.1,ou=People,dc=example,dc=com
    updated by:cn=Directory Manager,cn=Root DNs,cn=config
    updated at:20181016152050.176Z
uid=user.2,ou=People,dc=example,dc=com
uid=user.3,ou=People,dc=example,dc=com
uid=user.4,ou=People,dc=example,dc=com
uid=user.5,ou=People,dc=example,dc=com
uid=user.6,ou=People,dc=example,dc=com
uid=user.7,ou=People,dc=example,dc=com
```

## Getting an entry (similar to Unix cat)
Getting an entry should be simple, and it is:
```$ bin/ldap-cat dc=example,dc=com

dn: dc=example,dc=com
objectClass: top
objectClass: domain
dc: example

```
You can also get your entry as JSON as a convenience to parsing data with jq for example
```$bin/ldap-cat --json dc=example,dc=com

{ "result-type":"entry",
  "dn":"dc=example,dc=com",
  "attributes":[ { "name":"objectClass",
                   "values":[ "top",
                              "domain" ] },
                 { "name":"dc",
                   "values":[ "example" ] } ] }
```

## Updating an entry (using vi)

## Removing an entry (similar to Unix rm)
```$ bin/ldap-rm uid=user.7,ou=People,dc=example,dc=com

Arguments from tool properties file:  --hostname localhost --port 1389 --bindDN cn=administrator --bindPassword *****

Processing DELETE request for uid=user.7,ou=People,dc=example,dc=com
DELETE operation successful for DN uid=user.7,ou=People,dc=example,dc=com
```
