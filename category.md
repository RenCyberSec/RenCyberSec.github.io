---
layout: page
subtitle: A collection of categorized posts
---

### Threat Hunting / Endpoint Investigation

{% assign threat_posts = site.posts | where: "category", "threat-hunting" %}
{% for post in threat_posts %}
- **{{ post.date | date: "%b %d, %Y" }}** - [{{ post.title }}]({{ post.url }})
{% endfor %}

---

### Malware Analysis

{% assign malware_posts = site.posts | where: "category", "malware-analysis" %}
{% for post in malware_posts %}
- **{{ post.date | date: "%b %d, %Y" }}** - [{{ post.title }}]({{ post.url }})
{% endfor %}
