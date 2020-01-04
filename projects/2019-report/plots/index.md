---
layout: project
project-slug: 2019-report
title: Analysis plots
---
All of these plots are interactive if you click the page for a given plot - you can zoom and scroll around.

Later on, I hope to add some more context and analysis (correlations, process explanation, major events that influenced my behaviors).

If that's interesting to you, [tell me](http://twitter.com/aaronbeekay) and I'll be a lot more motivated to get it done.


<div class="card-columns">
{% assign all_plots = site.pages | where: 'layout', 'single-plot' %}
{% for plot in all_plots %}
	<div class="card mb-3">
	<a href="{{plot.url}}"><img class="card-img-top" src="/assets/images/{{plot.plot-slug}}-sm.png"></a>
<div class="card-body">
<h5 class="card-title"><a href="{{plot.url}}">{{plot.title}}</a>
</h5></div></div>
{% endfor %}
</div>