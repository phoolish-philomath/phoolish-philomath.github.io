---
layout: page
title: Book Tracker
permalink: /book-tracker/
---
This is a webpage I created to track my reading progress as I try to make my way through the pile of books I am currently reading or intend to read. For each book cover on this page, the transparent area is proportional to how much of the book is still unread by me. Visualizing my books this way really helps me keep reading every day and (somewhat) encourages me to avoid acquiring yet more new books. 
<div class='book-tracker-grid'>
{% assign sorted_books = site.books | sort: "last_read_date" | reverse %}

<div class='read-grid'>
{% for book in sorted_books %}
{% assign read_float = book.read_pages | times: 100.0  %}
{% assign read_proportion = read_float | divided_by: book.total_pages %}
<div class="read" style="--proportion: {{ read_proportion }}%">
<a class="image-link" href="{{site.url}}/books/{{ book.shorthand }}">
<img src="/assets/images/{{ book.shorthand }}.jpg"  >
</a>
</div>
{% endfor %}
</div>

<div class='unread-grid'>
{% for book in sorted_books %}
<div class="unread">
<a class="image-link" href="{{site.url}}/books/{{ book.shorthand }}">
<img  src="/assets/images/{{ book.shorthand }}.jpg" alt="{{ book.title }}" >
</a>
</div>
{% endfor %}
</div>

</div>

### **Developer Notes**
The layout above has been created using CSS Grid.
