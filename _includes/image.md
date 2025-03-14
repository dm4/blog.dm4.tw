{% assign url = page.url %}
{% assign suffix = page.url | slice: -4, 4 %}
{% if suffix == '-eng' %}
  {% assign length = page.url | size %}
  {% assign new_length = length | minus: 4 %}
  {% assign url = page.url | slice: 0, new_length %}
{% endif %}
<div align="center">
  <a target="_blank" href="{{ "/assets/images" | append: url | append: "/" | append: include.name }}">
    <img src="{{ "/assets/images" | append: url | append: "/" | append: include.name }}" alt="{{ include.alt }}" />
  </a>
</div>
