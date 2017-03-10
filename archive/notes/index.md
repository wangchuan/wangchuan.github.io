---
layout: page
title: "Notes"
categories: hidden
tags: index
use_math: true
---
{% include JB/setup %}

<link rel="stylesheet" href="/glyphicons/css/glyphicons.css" />

## Table of Contents

<p style="padding-top: 10px"/>
<div id="recent-posts" class="recent-posts">
  {% for this_post in site.html_pages %}
  {% if this_post.categories == "hidden" and this_post.tags == "special" %}
  <a href="{{ this_post.url }}">
        <p class="post-title" style="font-size:18px; padding-top: 3px;font-family:Helvetica Neue; font-weight: 400;">{{ this_post.title }}
        </p>
  </a>
  {% endif %}
  {% endfor %}
</div>
<script type="text/javascript">
  var el = document.getElementById("recent-posts");
  fix_cjk_linebreak(el);
  fix_table_style(el);
</script>

