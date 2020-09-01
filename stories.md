---
layout: page
title:  Stories
permalink: /stories/
---

<div class="home">
  {%- if site.posts.size > 0 -%}
    <ul class="post-list">
      {%- for post in site.posts -%}
      <li>
        <h3>
          <a class="post-link" href="{{ post.url | relative_url }}">
            {{ post.title | escape }}
          </a>
        </h3>
        <p class="post-meta">
          {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
          <time class="dt-published" datetime="{{ post.date | date_to_xmlschema }}" itemprop="datePublished">
            {{ post.date | date: date_format }}
          </time>
          {%- if post.author -%}
            •{% for author in post.author %}
              <span itemprop="author" itemscope itemtype="http://schema.org/Person">
                <span class="p-author h-card" itemprop="name">{{ author }}</span></span>
                {%- if forloop.last == false %}, {% endif -%}
            {% endfor %}
          {%- endif -%}
          {%- for category in post.categories -%}
            &nbsp;• <span>{{ category | capitalize }}</span>
          {%- endfor -%}
        </p>
        {%- if site.show_excerpts -%}
          {{ post.excerpt }}
        {%- endif -%}
      </li>
      {%- endfor -%}
    </ul>
  {%- endif -%}

</div>