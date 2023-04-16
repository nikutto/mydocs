# LDAP

## Summary

LDAP is Lightweight Directory Acceess Protocol.
So, it is actually protocol.
Data are stored in directory server (such as Active Directory by Windows).
Directory servers which can talk LDAP is called LDAP server.
LDAP server is a kind of No-SQL database.

### Usage

- Auth
  - Kerberos Authentication
  - GitLab

### Pros

- Open standard protocol
    - Using No-SQL make client implementation specific to its No-SQL.
    - RDBS is better than No-SQL from this perspective, but it still use different implementation among vendors.
        - (There is ODBC. It solves most part of this problem, I think.)
    - Defined in RFC-4511.
- Still evolving 
    - LDAP is not out of date technique
    - LDAP is not HTTP-based and have long-lived connection
        - Good performance
- Lightweight
    - LDAP is lightweight version of X.500
    - LDAP is also lightweight compared to modern protocols
- Secure
    - LDAP is often used as authentication repository, and it needs strong encryption for stored data.
    - Fine-grained access control

## Reference

- ldap.com: https://ldap.com/
- openldap: https://www.openldap.org/
