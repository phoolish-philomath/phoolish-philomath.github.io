---
layout: page
title: Book Tracker
permalink: /book-tracker/
---
This is a simple webpage I created to start tracking my reading progress as I try to make my way through the pile of books I am currently reading or intend to read. It also serves the purpose of helping me get a bit better at front-end development. 

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
<img src="/assets/images/{{ book.shorthand }}.jpg"  >
</div>
<div class="unread">
<a class="imagelink" href="{{site.url}}/books/{{ book.shorthand }}">
<img  src="/assets/images/{{ book.shorthand }}.jpg" alt="{{ book.title }}: {{ read_proportion }}" >
</a>
</div>
</div>
</td>
{% if mod_3 == 2 or counter == book_count %}
</tr>
{% endif %}
{% assign counter = counter | plus: 1 %} 
{% endfor %}
</table>

### **Notes**
For each book cover on this page, the transparent area is proportional to how much of the book is still unread.
 
You can click on a book cover to be taken to a profile page dedicated to that book with more details and notes.
