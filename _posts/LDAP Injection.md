# LDAP Injection

LDAPi is very similar to other injection (E.g., SQLi, NoSQL, OSi). It allow attacker to bypass the authentication to retrieve user’s data or perform RCE

# Workflow

## Identify LDAP-Driven Inputs

- Find where the app interacts with the directory
e.g., login forms, search boxes).
- Suspect LDAP if the app authenticates against **Active Directory** or similar.

## Inject Special Characters

- Start with: `'`, `"`, `*`, `)`, `(|)`, `&`, `=` to break the logic or manipulate the LDAP query.
- Watch for errors like:
    
    `LDAPException`, `Unbalanced parentheses`, or **no filtering applied**.
    

## Test Bypass Scenarios

```
admin*)(|(uid=*))
admin)(objectClass=*)
*)(uid=*))(|(uid=*
```

## Observe Server Behavior

- Authentication success with invalid creds
- Unfiltered search results
- LDAP-specific error messages