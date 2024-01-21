---
layout: post
title:  "Setting up Big Data Analysis Environment"
summary: "Learn how to set up big data analysis environment"
author: seunghwan
date: '2024-01-22 00:00:00 +0530'
category: ['big_data', 'hadoop', 'spark']
tags: big_data
#thumbnail: /assets/img/posts/code.jpg
usemathjax: false
permalink: /blog/adding-categories-tags-in-posts/
---

```yml
---
category: ['big_data', 'hadoop', 'spark']
---
```

```jsx
---
layout: page
title: Guides
permalink: /blog/categories/big_data/
---

<h5> Posts by Category : {{ page.title }} </h5>

<div class="card">
{% for post in site.categories.big_data %}
 <li class="category-posts"><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
</div>