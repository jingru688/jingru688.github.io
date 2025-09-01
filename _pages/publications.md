---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---

{% include base_path %}

{%- assign manual_cat_order = site.publication_category_order -%}
{%- if manual_cat_order and manual_cat_order.size > 0 -%}
  {%- assign categories = manual_cat_order -%}
{%- else -%}
  {%- assign categories = site.publications | group_by: "category" | map: "name" | sort -%}
{%- endif -%}

{%- for key in categories -%}
  {%- assign label = site.publication_category[key].title | default: key -%}
  {%- assign items = site.publications | where: "category", key -%}
  {%- if items and items.size > 0 -%}
  <h2 id="{{ key | slugify }}">{{ label }}</h2>
  <ul class="pub-list" style="list-style:none;margin:0 0 1rem 0;padding:0;">
    {%- assign ordered = items | where_exp: "p", "p.order" | sort: "order" -%}
    {%- for post in ordered -%}
      <li style="margin:0 0 .75rem 0;">
        <a href="{{ post.paperurl | default: post.url | relative_url }}">{{ post.title }}</a>
        {%- assign cat = post.category | downcase -%}
        {%- unless cat == "jmp" or cat == "working" -%}
          {%- if post.venue -%}
            — <em>{{ post.venue }}</em>{% if post.date %}, {{ post.date | date: "%Y" }}{% endif %}
          {%- endif -%}
        {%- endunless -%}
      </li>
    {%- endfor -%}
    {%- assign unordered = items | where_exp: "p", "p.order == nil" | sort: "date" -%}
    {%- for post in unordered -%}
      <li style="margin:0 0 .75rem 0;">
        <a href="{{ post.paperurl | default: post.url | relative_url }}">{{ post.title }}</a>
        {%- assign cat = post.category | downcase -%}
        {%- unless cat == "jmp" or cat == "working" -%}
          {%- if post.venue -%}
            — <em>{{ post.venue }}</em>{% if post.date %}, {{ post.date | date: "%Y" }}{% endif %}
          {%- endif -%}
        {%- endunless -%}
      </li>
    {%- endfor -%}
  </ul>
  {%- endif -%}
{%- endfor -%}
