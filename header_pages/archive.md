---
layout: page
title: "Archive"
permalink: /Archive/
main_nav: true
---

{% assign postsByYear = site.posts | group_by_exp:"post", "post.date | date: '%Y'" %}
{% for year in postsByYear %}
  <h4>{{ year.name }}</h4>
  <ul>
    {% for post in year.items %}
      <li>
        <div style="width:60px;float:left;">{{ post.date | date: "%b %-d"}}</div>
        <a href="{{ post.url }}">{{ post.title }}</a>
      </li>
    {% endfor %}
  </ul>
{% endfor %}