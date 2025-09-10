---
layout: post
title: XXE (XML External Entities) Injection
subtitle: How malicious user can abouse External Entities to perform XXE
date: 2025-07-30 12:43
author: Ren Sie
comments: true
category: injection
---

By default, XML has predefined entities to represent special symbols (like `&lt;` for `<`), but **XML External Entities (XXE)** allow users to define **custom entities**. These custom entities can reference external resources, such as local files or URLs. This flexibility can be exploited to read sensitive files on the server, or run RCE.  
The method isn’t as popular as others because of the patches. But if comes across with application that access and passing XML, worth trying!

{: .box-note}
**Note:** 💡 Try pass XML data to API endpoint. They sometimes accept XML data besides JSON data.

## XML
Some Apps use XML to transfer data. XML has its default entities, which define the way of representing data or special characters. For instance: 
- `&amp;` → `&`
- `&lt;` → `<`
- `&gt;` → `>`

### **External Entities**
These are custom-defined entities that reference external resources, such as _local files_ or _external URLs_. Thus, we can use this function to read file and RCE.
They can be defined using the `<!ENTITY>` syntax in XML like this:
~~~
xml:
<!ENTITY entity_name SYSTEM "path_to_file_or_url">
~~~
An example of malicious XML can be:
The entity `xxe` in the `creds` document reference the `/etc/passwd`. When the file path (external URL) is passed, it will be places at `&xxe;`.
~~~
xml:
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE creds [
<!ELEMENT creds ANY >
<!ENTITY xxe SYSTEM "file:///etc/passwd" >]>
<creds><user>&xxe;</user><password>pass</password></creds>
~~~

## Prevention
1. Disable XML external entity
2. Use JSON instead of XML
3. Validate and Sanitize XML Input (with XSD)
4. Restrict Network/File Access (OS or Container Level)
