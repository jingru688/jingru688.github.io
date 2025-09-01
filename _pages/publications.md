---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---

{% include base_path %}

{%- comment -%}
This page:
• Supports manual category order via _config.yml: publication_category_order: [jmp, paper, working]
• Supports per-item manual ordering by adding `order: <number>` in each _publications/*.md (smaller = higher)
• Hides the “Published in …, YEAR” line for category jmp and working
{%- endcomment -%}

<style>
  /* light spacing; optional */
  ul.pub-list { list-style: none; margin: 0 0 1rem 0; padding: 0; }
  ul.pub-list li { margin: 0 0 .75rem 0; }
</style>

{%- assign manual_cat_order = site.publication_category_order -%}

{%- assign render_item = "yes" -%}{% comment %}dummy var to keep the snippet readable{% endcomment %}

{%- capture pub_item_template -%}
<li class="pub-item">
  {%- assign href = post.paperurl | default: post.url | relative_url -%}
  <a href="{{ href }}">{{ post.title }}</a>
  {%- assign cat = post.category | downcase -%}
  {%- unless cat == "jmp" or cat == "working" -%}
    {%- if post.venue -%}
      — <em>{{ post.venue }}</em>{% if post.date %}, {{ post.date | date: "%Y" }}{% endif %}
    {%- endif -%}
  {%- endunless -%}
</li>
{%- endcapture -%}

{%- if manual_cat_order and manual_cat_order.size > 0 -%}
  {%- comment -%} Render categories in the order from _config.yml {%- endcomment -%}
  {%- for key in manual_cat_order -%}
    {%- assign label = site.publication_category[key].title | default: key -%}
    {%- assign items = site.publications | where: "category", key -%}
    {%- if items and items.size > 0 -%}
      <h2 id="{{ key | slugify }}">{{ label }}</h2>

      <ul class="pub-list">
        {%- assign ordered = items | where_exp: "p", "p.order" | sort: "order" -%}
        {%- for post in ordered -%}
          {{ pub_item_template }}
        {%- endfor -%}

        {%- assign unordered = items | where_exp: "p", "p.order == nil" | sort: "date" -%}
        {%- for post in unordered -%}
          {{ pub_item_template }}
        {%- endfor -%}
      </ul>
    {%- endif -%}
  {%- endfor -%}

  {%- comment -%} Any remaining categories not listed {%- endcomment -%}
  {%- assign other = site.publications | group_by: "category" -%}
  {%- for g in other -%}
    {%- unless manual_cat_order contains g.name -%}
      {%- assign label = site.publication_category[g.name].title | default: g.name -%}
      <h2 id="{{ g.name | slugify }}">{{ label }}</h2>
      <ul class="pub-list">
        {%- assign ordered = g.items | where_exp: "p", "p.order" | sort: "order" -%}
        {%- for post in ordered -%}{{ pub_item_template }}{%- endfor -%}
        {%- assign unordered = g.items | where_exp: "p", "p.order == nil" | sort: "date" -%}
        {%- for post in unordered -%}{{ pub_item_template }}{%- endfor -%}
      </ul>
    {%- endunless -%}
  {%- endfor -%}

{%- else -%}
  {%- comment -%} No manual category order: group alphabetically {%- endcomment -%}
  {%- assign groups = site.publications | group_by: "category" -%}
  {%- for g in groups -%}
    <h2 id="{{ g.name | slugify }}">{{ site.publication_category[g.name].title | default: g.name }}</h2>
    <ul class="pub-list">
      {%- assign ordered = g.items | where_exp: "p", "p.order" | sort: "order" -%}
      {%- for post in ordered -%}{{ pub_item_template }}{%- endfor -%}
      {%- assign unordered = g.items | where_exp: "p", "p.order == nil" | sort: "date" -%}
      {%- for post in unordered -%}{{ pub_item_template }}{%- endfor -%}
    </ul>
  {%- endfor -%}
{%- endif -%}
