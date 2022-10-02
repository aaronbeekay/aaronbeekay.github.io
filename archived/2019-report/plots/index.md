---
layout: project
project-slug: 2019-report
title: Analysis plots
---
All of these plots are interactive if you click the page for a given plot - you can zoom and scroll around.

*January 2019*: Round one of this report is the "raw data" plots that are up on this page. I haven't drawn any very interesting conclusions or mapped out correlations yet.

Next steps: clean up the data from the beginning of the year and put together some correlation plots.

If this is interesting, please [tell me](http://twitter.com/aaronbeekay) – it's a great motivator for me to keep hacking at this.


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