---
layout: post
title: "Transact SQL shorthand JOIN operators"
description: ""
category: 
tags: []
---


If you've never seen the T-SQL operators, \*= and =\*, consider yourself lucky. These operators are shorthand, or non-ANSI, syntax
for LEFT OUTER JOIN and RIGHT OUTER JOIN, respectively. This syntax can cause ambiguity (sometimes interpreting as a cross join), and as a result [does not always interpret correctly](https://msdn.microsoft.com/en-us/library/aa213228%28v=sql.80%29.aspx).

Fortunately, these operators were [deprecated](https://msdn.microsoft.com/en-us/library/ms143729%28d=lightweight,v=sql.100%29.aspx) as of Sql Server 2008. 

Don't use these operators. Ever. Move your JOIN to the FROM clause where it belongs.

If you ever come across this in legacy code, replace this:

	SELECT *
	FROM Answers
	WHERE Answers.QuestionId *= Questions.Id and Answers.QuestionId is not null;

with this:

	SELECT * 
	FROM Answers a
	LEFT OUTER JOIN Questions q ON q.Id = a.QuestionId
	WHERE a.QuestionId is not null;