---
layout: page
title: Book Tracker
permalink: /book-tracker/
---
<style>
table.book-tracker {
border: 1px solid black;
	border-collapse: separate;
border-spacing: 30px;
}
table.book-tracker td {vertical-align:centre; horizontal-align:centre; border: 1px solid black;}


.read img {
    position:absolute;
    clip-path: polygon(0 calc(100% - var(--proportion)), 100% calc(100% - var(--proportion)), 100% 100%, 0 100%);
}

.unread img{
    filter: opacity(15%) saturate(5%);
}
</style>
<p>I made some changes by adding style to page.</p>
<table class='book-tracker'>
<tr>
<td>
<div class="read" style="--proportion: 60%">
<img src="/assets/images/left_hand.jpg" width="200px">
</div>
<div class="unread">
<img src="/assets/images/left_hand.jpg" width="200px">
</div>
</td>
<td>
<div class="read" style="--proportion: 10%">
<img src="/assets/images/enigma.jpg" width="200px">
</div>
<div class="unread">
<img src="/assets/images/enigma.jpg" width="200px">
</div>
</td>
<td>
<div class="read" style="--proportion: 35%">
<img src="/assets/images/dirty_wars.jpg" width="200px">
</div>
<div class="unread">
<img src="/assets/images/dirty_wars.jpg" width="200px">
</div>
</td>
</tr>
<tr>
<td>
<div class="read" style="--proportion: 10%">
<img src="/assets/images/raja_gidh.jpg" width="200px">
</div>
<div class="unread">
<img src="/assets/images/raja_gidh.jpg" width="200px">
</div>
</td>
<td>
<div class="read" style="--proportion: 10%">
<img src="/assets/images/angarey.jpg" width="200px">
</div>
<div class="unread">
<img src="/assets/images/angarey.jpg" width="200px">
</div>
</td>
<td>
<div class="read" style="--proportion: 10%">
<img src="/assets/images/dispossessed.jpeg" width="200px">
</div>
<div class="unread">
<img src="/assets/images/dispossessed.jpeg" width="200px">
</div>
</td>
</tr>
</table>
