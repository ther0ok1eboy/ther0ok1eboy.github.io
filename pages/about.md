---
layout: page
title: About
description: None
keywords: None
comments: true
menu: About
permalink: /about/
---

TODO

## Contact

<ul>
{% for website in site.data.social %}
<li>{{website.sitename }}：<a href="{{ website.url }}" target="_blank">@{{ website.name }}</a></li>
{% endfor %}
<li>
WeChat:<br />
<img style="height:192px;width:192px;border:1px solid lightgrey;" src="/assets/images/qrcode.jpg" alt="闷骚的程序员" />
</li>
</ul>


## Skill Keywords

{% for skill in site.data.skills %}
### {{ skill.name }}
<div class="btn-inline">
{% for keyword in skill.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
