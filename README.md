# This is Fazreil's website and it is under construction. 
### Here's the deal: _this webpage will discuss mainly about stuff that I personally appreciate._
<!--
time now is: {{ site.time }}, I don't know what timezone that is to be honest.
-->
<!-- 
<ul>
{% for post in site.posts %}
	<li>{{ post.category }}</li>
	{% endfor %}	
</ul>
s-->

## Posts:

{% for cat in site.posts %}
{% if cat.category != "classified" %}
### {{ cat.category }}
{% for post in site.posts %}
{% if cat.category == post.category %}
<ul>
	<li>
	<a href="{{ post.url }}">{{ post.title }}</a>
	</li>
</ul>
{% endif %}
{% endfor %}
{% endif %}
{% endfor %}	
