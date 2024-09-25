---
layout: pen-test
title: Wiki
description: 人越学越觉得自己无知
keywords: 维基, Wiki
comments: false
copyright: false
menu: 维基
permalink: /pen-test/
---

> 记多少命令和快捷键会让脑袋爆炸呢？

{% case site.components.pen-test.view %}

{% when 'list' %}

<ul class="listing">
{% for pen-test in site.pen-test %}
{% if pen-test.title != "pen-test Template" and pen-test.topmost == true %}
<li class="listing-item"><a href="{{ site.url }}{{ pen-test.url }}"><span class="top-most-flag">[置顶]</span>{{ pen-test.title }}</a></li>
{% endif %}
{% endfor %}
{% for pen-test in site.pen-test %}
{% if pen-test.title != "Pen-test Template" and pen-test.topmost != true %}
<li class="listing-item"><a href="{{ site.url }}{{ pen-test.url }}">{{ pen-test.title }}<span style="font-size:12px;color:red;font-style:italic;">{%if pen-test.layout == 'mindmap' %}  mindmap{% endif %}</span></a></li>
{% endif %}
{% endfor %}
</ul>

{% when 'cate' %}

{% assign item_grouped = site.pen-test | where_exp: 'item', 'item.title != "Pen-test Template"' | group_by: 'cate1' | sort: 'name' %}
{% for group in item_grouped %}
### {{ group.name }}
{% assign cate_items = group.items | sort: 'title' %}
{% assign item2_grouped = cate_items | group_by: 'cate2' | sort: 'name' %}
{% for sub_group in item2_grouped %}
{% assign name_len = sub_group.name | size %}
{% if name_len > 0 -%}
<i>{{ sub_group.name }}: <sup>{{ sub_group.items | size }}</sup></i>
{%- endif -%}
{%- assign item_count = sub_group.items | size -%}
{%- assign item_index = 0 -%}
{%- for item in sub_group.items -%}
{%- assign item_index = item_index | plus: 1 -%}
<a href="{%- if item.type == 'link' -%}{{ item.link }}{%- else -%}{{ site.url }}{{ item.url }}{%- endif -%}" style="display:inline-block;padding:0.5em" {% if item.type == 'link' %} target="_blank" {% endif %} >{{ item.title }}<span style="font-size:12px;color:red;font-style:italic;">{%if item.layout == 'mindmap' %}  mindmap{% endif %}</span></a>{%- if item_index < item_count -%}<span> <b>·</b></span>{%- endif -%}
{%- endfor -%}
{% endfor %}
{% endfor %}

{% endcase %}
