---
layout: page
title: About
permalink: /about/
weight: 3
---

# **关于我**

朋友你好，我是 **{{ site.author.name }}** :wave:<br>

<div class="row">
{% include about/skills.html title="编程技能" source=site.data.programming-skills %}
{% include about/skills.html title="其他技能" source=site.data.other-skills %}
</div>


<div class="row">
{% include about/timeline.html %}
</div>