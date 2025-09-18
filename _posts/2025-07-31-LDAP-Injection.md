---
layout: post
title: LDAP Injection
subtitle: LDAP allow attacker to bypass the authentication to retrieve user’s data or perform RCE
date: 2025-07-31 10:17
author: Ren Sie
comments: true
category: injection
thumbnail-img: /assets/img/githubpost/ldapi_thumb.png
share-img: /assets/img/Post_Cover.png
---
LDAPi is very similar to other injection (E.g., SQLi, NoSQL, OSi). It allow attacker to bypass the authentication to retrieve user’s data or perform RCE

### Workflow
1. Identify LDAP-Driven Inputs
   - Find where the app interacts with the directory  
    e.g., login forms, search boxes.
   - Suspect LDAP if the app authenticates against **Active Directory** or similar.

2. Inject Special Characters
   - Start with: `'`, `"`, `*`, `)`, `(|)`, `&`, `=` to break the logic or manipulate the LDAP query.
   - Watch for errors like:  
     `LDAPException`, `Unbalanced parentheses`, or **no filtering applied**.
    
### Test Bypass Scenarios
1. Bypass Authentication  
   Closes the first filter, injects an OR condition that always evaluates to true (`uid=*`), and completes the expression.
   ~~~
   admin*)(|(uid=*))
   # The OR clause |(uid=*) means “any user with any uid,” so the query could return all users, bypassing the password check.
   ~~~
2. Broaden Search / Information Disclosure
   This breaks the current condition and appends a filter for any object with any class.
   ~~~
   admin)(objectClass=*)
   # The query now returns all directory objects that have an objectClass attribute (which is basically everything), regardless of the password.
   ~~~
3. Break Filter Logic / Auth Bypass
   This is a malformed but effective payload to confuse or break LDAP query parsing.  
   It introduces a closing ) early, opens a new OR clause, and adds a wildcard match.
   ~~~
   *)(uid=))(|(uid=*
   # Depending on how the query is built, this can cause syntax errors or return multiple users, bypassing authentication or crashing the app (helpful in testing/debugging).
   ~~~

### Observe Server Behavior
- Authentication success with invalid creds
- Unfiltered search results
- LDAP-specific error messages
