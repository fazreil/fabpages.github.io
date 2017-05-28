{% include header.html %}

### This is Fazreil's website and it is under construction. 
So f_ck off.

I mean it.

time now is: {{ site.time }}



<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>

