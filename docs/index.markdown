---
layout: home
---

<ul style="list-style:none;">
  {% for post in site.posts %}
    <li>
        <a href="{{ post.url }}">
            <h2>
            {{ post.title }}
                <span style="float:right;">
                    {{ post.date | date_to_string }}
                </span>
            </h2>
        </a>
    </li>
  {% endfor %}
</ul>