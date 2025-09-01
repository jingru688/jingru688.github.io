---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---

{% include base_path %}

{%- assign manual_cat_order = site.publication_category_order -%}

{%- if manual_cat_order and manual_cat_order.size > 0 -%}
  {%- comment -%} Render categories in the order specified in _config.yml {%- endcomment -%}
  {%- for key in manual_cat_order -%}
    {%- assign label = site.publication_category[key].title | default: key -%}
    {%- assign items = site.publications | where: "category", key -%}
    {%- if items and items.size > 0 -%}
      <h2 id="{{ key | slugify }}">{{ label }}</h2>

      {%- comment -%} 1) Items with 'order' → ascending {%- endcomment -%}
      {%- assign ordered = items | where_exp: "p", "p.order" | sort: "order" -%}
      {%- for post in ordered -%}
        {% include archive-single.html %}
      {%- endfor -%}

      {%- comment -%} 2) Items without 'order' → by date (oldest → newest) {%- endcomment -%}
      {%- assign unordered = items | where_exp: "p", "p.order == nil" | sort: "date" -%}
      {%- for post in unordered -%}
        {% include archive-single.html %}
      {%- endfor -%}
    {%- endif -%}
  {%- endfor -%}

  {%- comment -%} Render any categories not listed in manual_cat_order (optional) {%- endcomment -%}
  {%- assign other_groups = site.publications | group_by: "category" -%}
  {%- for g in other_groups -%}
    {%- unless manual_cat_order contains g.name -%}
      {%- assign label = site.publication_category[g.name].title | default: g.name -%}
      <h2 id="{{ g.name | slugify }}">{{ label }}</h2>
      {%- assign ordered = g.items | where_exp: "p", "p.order" | sort: "order" -%}
      {%- for post in ordered -%}{% include archive-single.html %}{% endfor -%}
      {%- assign unordered = g.items | where_exp: "p", "p.order == nil" | sort: "date" -%}
      {%- for post in unordered -%}{% include archive-single.html %}{% endfor -%}
    {%- endunless -%}
  {%- endfor -%}

{%- else -%}
  {%- comment -%} No manual category order: group alphabetically by category {%- endcomment -%}
  {%- assign groups = site.publications | group_by: "category" -%}
  {%- for g in groups -%}
    <h2 id="{{ g.name | slugify }}">{{ site.publication_category[g.name].title | default: g.name }}</h2>
    {%- assign ordered = g.items | where_exp: "p", "p.order" | sort: "order" -%}
    {%- for post in ordered -%}{% include archive-single.html %}{% endfor -%}
    {%- assign unordered = g.items | where_exp: "p", "p.order == nil" | sort: "date" -%}
    {%- for post in unordered -%}{% include archive-single.html %}{% endfor -%}
  {%- endfor -%}
{%- endif -%}
