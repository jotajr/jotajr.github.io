---
title: "#GRADUATION"
bg: green
color: white
fa-icon: graduation-cap
---

{% assign graduations = site.data.graduations %}

{% for graduation in graduations %}
<div class="row">
  <div class="half column title">

    <h2>
      {{ graduation.title }}
    </h2>
      {{ graduation.date }}
       {% if graduation.concluded %}
        -  {{ graduation.concluded }}
      {% endif %}    

      {% if graduation.lock %}
      <span class="lock" title="locked">-  {{ graduation.lock }}</span>
        
      {% endif %}    

  </div>

  <div class="half column desc">
    <h4>{{ graduation.institution }}</h4>

    {% if graduation.description %}
      {{ graduation.description | markdownify }}
    {% endif %}    
  </div>

</div>
{% endfor %}