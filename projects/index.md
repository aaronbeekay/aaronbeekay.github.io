---
layout: default
title: Projects
---

# Projects

<ul>
{% for project_hash in site.data.projects %}
{% assign p = project_hash[1]  %}
	<li><a href="/projects/{{p.project-slug}}">{{p.project-name}}</a></li>
{% endfor %}
</ul>
