---
layout: page
title: Book Tracker
permalink: /book-tracker/
---
{% assign book_count = -1 %}
{% for book in site.books %}
{% assign book_count = book_count | plus: 1 %}
{% endfor %}

<table class='book-tracker'>
{% assign counter = 0 %}
{% assign sorted_books = site.books | sort: "last_read_date" | reverse %}
{% for book in sorted_books %}
{% assign mod_3 = counter | modulo: 3 %}
{% if mod_3 == 0 %}
<tr>
{% endif %} 
{% assign read_float = book.read_pages | times: 100.0  %}
{% assign read_proportion = read_float | divided_by: book.total_pages %}
<td>
<div class="container">
<div class="read" style="--proportion: {{ read_proportion }}%">
<a style="text-decoration:none;border-bottom:none;" href="{{site.url}}/books/{{ book.shorthand }}"><img border="0" style="border: 0 none;" src="/assets/images/{{ book.shorthand }}.jpg"  ></a>
</div>
<div class="unread">
<a style="text-decoration:none;border-bottom:none;" href="{{site.url}}/books/{{ book.shorthand }}"><img border="0" style="border: 0 none;" src="/assets/images/{{ book.shorthand }}.jpg"  ></a>
</div>
</div>
</td>
{% if mod_3 == 2 or counter == book_count %}
</tr>
{% endif %}
{% assign counter = counter | plus: 1 %} 
{% endfor %}
</table>
