# Description
This project captures files that can make day to day operations on the Ping tool a bit easier

# Assumptions
The main assumption that is made by these tools is that `config/tools.properties` file is populated such that ldpasearch, ldapmodify and ldapdelete will not prompt for connection information.

Another assumption is that these tools be installed (copied) to the server root bin/ directory.

# Examples
## Listing (similar to Unix ls)
Getting the base entry:
``` 
$ bin/ldap-ls
dc=example,dc=com
```

Listing children entries:
```
$ bin/ldap-ls ou=people,dc=example,dc=com
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
```
$ bin/ldap-cat dc=example,dc=com

dn: dc=example,dc=com
objectClass: top
objectClass: domain
dc: example

```
You can also get your entry as JSON as a convenience to parsing data with jq for example
```
$bin/ldap-cat --json dc=example,dc=com

{ "result-type":"entry",
  "dn":"dc=example,dc=com",
  "attributes":[ { "name":"objectClass",
                   "values":[ "top",
                              "domain" ] },
                 { "name":"dc",
                   "values":[ "example" ] } ] }
```

## Updating an entry (using vi)
You can make edits in vi. Once saved with :wq the tool will allow you to review the computed modifications and give you a chance to evaluate whether you want the changes to be applied.

```
$ bin/ldap-vi uid=user.6,ou=People,dc=example,dc=com
Computed modifications:
dn: uid=user.6,ou=People,dc=example,dc=com
changetype: modify
delete: homePhone
homePhone: +1 538 763 1811
-
add: homePhone
homePhone: +1 538 763 1812

Would you like to apply those modifications? (y/n)y
# Arguments obtained from properties file '/home/ping/PingDirectory/config/tools.properties':
#      --hostname localhost
#      --port 1389
#      --bindDN cn=administrator
#      --bindPassword '***REDACTED***'

# Successfully connected to localhost:1389.

# Modifying entry uid=user.6,ou=People,dc=example,dc=com ...
# Result Code:  0 (success)

```

## Removing an entry (similar to Unix rm)
```
$ bin/ldap-rm uid=user.7,ou=People,dc=example,dc=com

Arguments from tool properties file:  --hostname localhost --port 1389 --bindDN cn=administrator --bindPassword *****

Processing DELETE request for uid=user.7,ou=People,dc=example,dc=com
DELETE operation successful for DN uid=user.7,ou=People,dc=example,dc=com
```
