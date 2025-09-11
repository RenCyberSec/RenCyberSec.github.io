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
