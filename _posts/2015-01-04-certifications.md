---
title: "#CERTIFICATIONS"
bg: blackgray
color: white
fa-icon: trophy
---

{% assign certifications = site.data.certifications %}

{% for certification in certifications %}
 * **{{ certification.sponsor }}**: [{{ certification.title }}]({{ certification.uri }})
{% endfor %}