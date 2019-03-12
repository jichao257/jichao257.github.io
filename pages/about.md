---
layout: page
title: About
description: 菜鸟的 github 之家
keywords: 菜鸟集中营, Manoon
comments: true
menu: 关于
permalink: /about/
---

Hello 大家好，我是 Manoon。

这里是一个程序员的学习营地，是一个菜鸟的进击之路。

爱自己，爱生活，爱编程。

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
