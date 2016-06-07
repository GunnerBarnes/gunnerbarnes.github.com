---
layout: post
title: "Django Distinct QuerySet"
description: ""
category: 
tags: [noshlocal, django, postgresql, python]
---

I've been working on a [little side project recently](http://www.noshlocal.com) that is essentially a restaurant review site that limits the length of reviews to 140 characters. Reviews can have one of three ratings (good, meh, and bad), and can be upvoted by other users. On the main restaurant listing page, restaurants are ordered by popularity and display the total counts of reviews for each rating category. To keep the ratings counts fair and reliable, I wanted to allow users to leave as many reviews as they would like for a specific restaurant, but have the ratings counts on the listing page only count one distinct review per ratings category. In the case that a user leaves 20 positive reviews for a restaurant, the listing page would only reflect one positive review for that user.

Now, this project has been an attempt to learn Python and Django, which provides a nice `QuerySet` method called [distinct().](https://docs.djangoproject.com/en/1.7/ref/models/querysets/#distinct)

> On PostgreSQL only, you can pass positional arguments (fields) in order to specify the names of fields to which the DISTINCT should apply. This translates to a SELECT DISTINCT ON SQL query. Here’s the difference. For a normal distinct() call, the database compares each field in each row when determining which rows are distinct. For a distinct() call with specified field names, the database will only compare the specified field names.

Did I mention that I also chose to learn PostgreSQL for this project? What a lucky move that turned out to be.

So my original ratings count variable:

`positive_review_count = Reviews.objects.filter(restaurant=restaurant, rating=Good).count()`

Becomes: 
	
`positive_review_count = Reviews.objects.filter(restaurant=restaurant, rating=Good).distinct('created_by').count()`

Just like that, the ratings counts become a little more honest.
