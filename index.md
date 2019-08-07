Welcome to my personal website. You can find some stuff that I [typed](/posts.html) or [read](/books.html) at some point in time.

## Recent Posts
<ul>
  {% for post in site.posts limit:5 %}
  <li>
    {{ post.excerpt }}
    <a href="{{ post.url }}">Read more</a>
  </li>
  {% endfor %}
</ul>
