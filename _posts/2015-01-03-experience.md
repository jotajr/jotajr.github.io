---
title: "#EXPERIENCE"
bg: turquoise
color: white
fa-icon: briefcase
---

{% assign experiences = site.data.experiences %}

{% for experience in experiences %}
<div class="row">
  <div class="small column">
    <h3>
      {{ experience.company }}
      <span class="heading-font-size">{{ experience.date }}</span>
    </h3>
  </div>
  <div class="big column">
    {{ experience.description | markdownify }}

  </div>
</div>
{% endfor %}

<p class="center">
  <a href='http://vizualize.me/ridermansb?r=ridermansb' title='Riderman Sousa infographic CV on vizualize.me' target='_blank'><img src='http://vizualize.me/media/img/socialicons/viewmyviz.png'/></a>
</p>