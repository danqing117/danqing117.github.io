---
layout: default
---

<body>
  <div class="index-wrapper">
    <div class="aside">
      <div class="info-card">
        <p><font size="5">Danqing Yin</font></p>
        <a href="http://linkedin.com/in/danqing-yin-412a76b7" target="_blank"><img src="http://www.freeiconspng.com/uploads/linkedin-icon-31.png" alt="" width="25"/></a>
      </div>
      <div id="particles-js"></div>
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
