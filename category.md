---
layout: page
title: Post
---
### Red Team
<details markdown="1">
<summary><strong> CTF </strong></summary>

{% assign threat_posts = site.posts | where: "category", "penetration-testing" %}
{% for post in threat_posts %}
- **{{ post.date | date: "%b %d, %Y" }}** - [{{ post.title }}]({{ post.url }})
{% endfor %}

</details>
---

<details markdown="1">
<summary><strong> Web Application Testing </strong></summary>

  <details markdown="1">
  <summary><strong> Injection </strong></summary>
  
  {% assign threat_posts = site.posts | where: "category", "Injection" %}
  {% for post in threat_posts %}
  - **{{ post.date | date: "%b %d, %Y" }}** - [{{ post.title }}]({{ post.url }})
  {% endfor %}
  
  </details>

  <details markdown="1">
  <summary><strong> Script Input </strong></summary>
  
  {% assign threat_posts = site.posts | where: "category", "script-input" %}
  {% for post in threat_posts %}
  - **{{ post.date | date: "%b %d, %Y" }}** - [{{ post.title }}]({{ post.url }})
  {% endfor %}
  
  </details>

  <details markdown="1">
  <summary><strong> Request Forgery </strong></summary>
  
  {% assign threat_posts = site.posts | where: "category", "request-forgery" %}
  {% for post in threat_posts %}
  - **{{ post.date | date: "%b %d, %Y" }}** - [{{ post.title }}]({{ post.url }})
  {% endfor %}
  
  </details>

  <details markdown="1">
  <summary><strong> User Interaction </strong></summary>
  
  {% assign threat_posts = site.posts | where: "category", "user-interaction" %}
  {% for post in threat_posts %}
  - **{{ post.date | date: "%b %d, %Y" }}** - [{{ post.title }}]({{ post.url }})
  {% endfor %}
  
  </details>

  <details markdown="1">
  <summary><strong> Authentication Bypass </strong></summary>
  
  {% assign threat_posts = site.posts | where: "category", "authentication-bypass" %}
  {% for post in threat_posts %}
  - **{{ post.date | date: "%b %d, %Y" }}** - [{{ post.title }}]({{ post.url }})
  {% endfor %}
  
  </details>

  <details markdown="1">
  <summary><strong> Web Configuration </strong></summary>
  
  {% assign threat_posts = site.posts | where: "category", "web-configuration" %}
  {% for post in threat_posts %}
  - **{{ post.date | date: "%b %d, %Y" }}** - [{{ post.title }}]({{ post.url }})
  {% endfor %}
  
  </details>


</details>
---

### Blue Team
<details markdown="1">
<summary><strong> Threat Hunting / Endpoint Investigation</strong></summary>

{% assign threat_posts = site.posts | where: "category", "threat-hunting" %}
{% for post in threat_posts %}
- **{{ post.date | date: "%b %d, %Y" }}** - [{{ post.title }}]({{ post.url }})
{% endfor %}

</details>
---

<details markdown="1">
<summary><strong> Digital Forensics</strong></summary>

{% assign threat_posts = site.posts | where: "category", "digital-forensics" %}
{% for post in threat_posts %}
- **{{ post.date | date: "%b %d, %Y" }}** - [{{ post.title }}]({{ post.url }})
{% endfor %}

</details>
---

<details markdown="1">
<summary><strong> Network Traffic Inspection </strong></summary>

{% assign threat_posts = site.posts | where: "category", "network-traffic-inspection" %}
{% for post in threat_posts %}
- **{{ post.date | date: "%b %d, %Y" }}** - [{{ post.title }}]({{ post.url }})
{% endfor %}

</details>
---

<details markdown="1">
<summary><strong> Threat Intelligence </strong></summary>

{% assign threat_posts = site.posts | where: "category", "threat-intelligence" %}
{% for post in threat_posts %}
- **{{ post.date | date: "%b %d, %Y" }}** - [{{ post.title }}]({{ post.url }})
{% endfor %}

</details>
---

<details markdown="1">
<summary><strong> Malware Analysis </strong></summary>

{% assign threat_posts = site.posts | where: "category", "malware-analysis" %}
{% for post in threat_posts %}
- **{{ post.date | date: "%b %d, %Y" }}** - [{{ post.title }}]({{ post.url }})
{% endfor %}

</details>
---
