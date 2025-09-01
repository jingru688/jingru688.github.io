---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---

{% include base_path %}

{%- comment -%}
CONFIG:
• Optional category order in _config.yml:
    publication_category_order:
      - jmp
      - paper
      - working
• Manual per-item order in each _publications/*.md:
    order: 1   # smaller number = higher in list
BEHAVIOR:
• For categories 'jmp' and 'working', the “Published in …, YEAR” line is hidden.
• For all others (e.g., 'paper'), it shows if `venue:` exists.
{%- endcomment -%}

<style>
  ul.pub-list { list-style: none; margin: 0 0 1rem 0; padding: 0; }
  ul.pub-list li { margin: 0 0 .75rem 0; }
</style>

{%- assign manual_cat_order = site.publication_category_order -%}

{%- if manual_cat_order and manual_cat_order.size > 0 -%}
  {%- comment -%} Render categories in the manual order {%- endcomment -%}
  {%- for key in manual_cat_order -%}
    {%- assign label = site.publication_category[key].title | default: key -%}
    {%- assign items = site.publications | where: "category", key -%}
    {%- if items and items.size > 0 -%}
      <h2 id="{{ key | slugify }}">{{ label }}</h2>
      <ul class="pub-list">

        {%- assign ordered = items | where_exp: "p", "p.order" | sort: "order" -%}
        {%- for post in ordered -%}
          <li>
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
          <li>
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

  {%- comment -%} Any remaining categories not listed (optional) {%- endcomment -%}
  {%- assign groups = site.publications | group_by: "category" -%}
  {%- for g in groups -%}
    {%- unless manual_cat_order contains g.name -%}
      <h2 id="{{ g.name | slugify }}">{{ site.publication_category[g.name].title | default: g.name }}</h2>
      <ul class="pub-list">
        {%- assign ordered = g.items | where_exp: "p", "p.order" | sort: "order" -%}
        {%- for post in ordered -%}
          <li>
            <a href="{{ post.paperurl | default: post.url | relative_url }}">{{ post.title }}</a>
            {%- assign cat = post.category | downcase -%}
            {%- unless cat == "jmp" or cat == "working" -%}
              {%- if post.venue -%}
                — <em>{{ post.venue }}</em>{% if post.date %}, {{ post.date | date: "%Y" }}{% endif %}
              {%- endif -%}
            {%- endunless -%}
          </li>
        {%- endfor -%}
        {%- assign unordered = g.items | where_exp: "p", "p.order == nil" | sort: "date" -%}
        {%- for post in unordered -%}
          <li>
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
    {%- endunless -%}
  {%- endfor -%}

{%- else -%}
  {%- comment -%} No manual category order: group alphabetically {%- endcomment -%}
  {%- assign groups = site.publications | group_by: "category" -%}
  {%- for g in groups -%}
    <h2 id="{{ g.name | slugify }}">{{ site.publication_category[g.name].title | default: g.name }}</h2>
    <ul class="pub-list">
      {%- assign ordered = g.items | where_exp: "p", "p.order" | sort: "order" -%}
      {%- for post in ordered -%}
        <li>
          <a href="{{ post.paperurl | default: post.url | relative_url }}">{{ post.title }}</a>
          {%- assign cat = post.category | downcase -%}
          {%- unless cat == "jmp" or cat == "working" -%}
            {%- if post.venue -%}
              — <em>{{ post.venue }}</em>{% if post.date %}, {{ post.date | date: "%Y" }}{% endif %}
            {%- endif -%}
          {%- endunless -%}
        </li>
      {%- endfor -%}
      {%- assign unordered = g.items | where_exp: "p", "p.order == nil" | sort: "date" -%}
      {%- for post in unordered -%}
        <li>
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
  {%- endfor -%}
{%- endif -%}
