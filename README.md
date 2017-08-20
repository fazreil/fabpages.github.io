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
<ul>
	{% for cat in site.posts %}
	<li>{{ cat.category }}</li>
	{% for post in site.posts %}
		<ol>
			{% if cat.category == post.category %}
			<li>
				<a href="{{ post.url }}">{{ post.title }}</a>
			</li>
			{% endif %}
		<ol>
	{% endfor %}
	{% endfor %}	
</ul>




