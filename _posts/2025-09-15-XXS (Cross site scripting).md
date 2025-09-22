---
layout: post
title: XXS (Cross site scripting)
subtitle: Victim visit malicious URL/site to trigger the payload made of JavaScript. Causing their session to be controlled or data to be stolen
date: 2025-09-15 19:15
author: Ren Sie
comments: true
category: script-input
thumbnail-img: /assets/img/githubpost/xxs_thumb.png
---

{: .box-note}
**Note:** 💡 During the testing, use `print()` `prompt("text")` instead of `alert()` since `alert()` is commonly detected and filtered. <br> When testing for XXS, test for html injection first because it’s more likely to work
E.g., \<h1\>test\</h1\>

### Checklist
<details markdown="1">
  <summary>Click me to expand checklist</summary>  

1. **Is input reflected in the response?**
2. **Can we inject HTML?**
   - E.g., `https[://]victim[.]com/search?user=<img src=x onerror=prompt("XSS")>`
4. **Any weaknesses in the Content Security Policy (CSP)?**
   - Use of unsafe directives which allow execution of inline scripts or eval() functions, bypassing CSP protections. <br> E.g.,`Content-Security-Policy: script-src 'self' 'unsafe-inline' 'unsafe-eval';`
   - Allowing broad sources or wildcards in directives (e.g., script-src), which permits potentially untrusted external scripts to run. <br> E.g., `Content-Security-Policy: script-src *;`
   - Inclusion of compromised or vulnerable third-party domains in trusted sources, such as JSONP endpoints that can be exploited to inject malicious scripts. <br> E.g., `https[://]third-party_domain[.]com/jsonp?callback=prompt("xss is available!")`
   - Omitting strict directives for resources like object-src or failing to restrict nonces and hashes properly which can allow script injection.
   - Weak or predictable nonces (e.g., 'nonce-12345') that attackers can guess or reproduce to bypass CSP restrictions.
6. **Can we use events (e.g. onload, onerror)?**
7. **Are there any filtered or escaped characters?**
8. **Is your input stored and then later rendered?**
9. **Can you inject into non-changing values (e.g. usernames)?**
10. **Any input collected from a third party (e.g. account information)?**
11. Is the version of the framework or dependency vulnerable?

</details>

### Reflected XXS
Inject payload into the HTTP request. And once the web server receive the request, it will response with the payload to sender’s browser. It relies on URL to make user fall for the attack.

### Dom-Based XXS
Client-side has vulnerable JavaScript. It doesn’t involve with the server-side, completely rely on client’s browser to execute the payload.
One quick check is turn on the developer tool, input and execute a simple prompt or alert script, see if there’s any network interaction with the server.

{: .box-note}
**Note:** 🚨 Inspecting Network page in the developer tab while sending input. If there’s no external communication happens, it indicates it is Dom-Based XXS.

#### Page Redirect
User could inject the following code into a vulnerable website, causing an automatic redirect to a malicious site. When a victim loads the page with this injected script, they will be automatically redirected to <$url>.
~~~
src=x onerror="window.location.href='<$url>'"
~~~

### Store
Payload is stored in the database, and victim’s browser will retrieve it once it visit the page. For instance:  
1. In the attacker’s browser leave html injection comment: \<h1\>test\</h1\>.
2. Visit the same page from victim’s browser

![store_xxs.png](/assets/img/githubpost/xxs_1.png)
