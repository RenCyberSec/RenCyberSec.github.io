---
layout: page
---

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


### Malware Analysis

{% assign malware_posts = site.posts | where: "category", "malware-analysis" %}
{% for post in malware_posts %}
- **{{ post.date | date: "%b %d, %Y" }}** - [{{ post.title }}]({{ post.url }})
{% endfor %}
