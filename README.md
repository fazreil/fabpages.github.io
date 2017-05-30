{% include header.html %}

### This is Fazreil's website and it is under construction. 
this webpage will discuss mainly about stuff that I personally appreciate.

I mean it.

time now is: {{ site.time }}, I don't know what timezone that is to be honest.



<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>





