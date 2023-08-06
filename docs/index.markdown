---
layout: home
---

<ul style="list-style:none;">
    {% for post in site.posts %}
    <div class="blog-link">
        <li>
            <a href="{{ post.url }}">
                <h2 style="border-bottom: solid 2px rgba(0, 0, 0, 0.5);">
                <div class="blog-title">
                {{ post.title }}
                    <span style="float:right;">
                        {{ post.date | date_to_string }}
                    </span>
                </div>
                </h2>
            </a>
        </li>
    </div>
    {% endfor %}
</ul>
