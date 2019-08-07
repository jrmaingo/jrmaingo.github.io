# Posts
Things that I felt the need to write down at some point in time.

<ul>
  {% for post in site.posts %}
  <li>
    {{ post.excerpt }}
    <a href="{{ post.url }}">Read more</a>
  </li>
  {% endfor %}
</ul>
