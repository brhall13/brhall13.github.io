{% for post in site.posts limit 6 %}
<a href="{{ post.url }}">{{ post.title }}</a>
{% endfor %}