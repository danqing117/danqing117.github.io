---
layout: default
---

<body>
  <div class="index-wrapper">
    <div class="aside">
      <div class="info-card">
        <h1><font size="6" face = "Agency FB">Danqing Yin</font></h1>
        <a href="http://linkedin.com/in/danqing-yin-412a76b7" target="_blank"><img src="images/linkedin-icon-31.png" alt="" width="25"/></a>
        <a href="mailto:danqing.yin@yahoo.com" target="_blank"><img src="images/mail2.png" alt="" width="24"/></a>
      </div>
      <!--<div id="particles-js"></div>-->
    </div>

    <div class="index-content">
      <ul class="artical-list">
        {% for post in site.categories.blog %}
        <li>
          <a href="{{ post.url }}" class="title">{{ post.title }}</a>
          <div class="title-desc">{{ post.description }}</div>
        </li>
        {% endfor %}
      </ul>
    </div>
  </div>
</body>
