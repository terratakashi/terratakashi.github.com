---
layout: post
title: "Ruby: Multiple if statements and do-while loop."
date: 2013-06-12 21:36
comments: true
categories: [Ruby, Programming Skill]
---

`if` is the most basic statement in programming.  However, sometimes we need to deal with more complex logical issue by mutilple `if` 
statements.  In C/C++ programs, we usually use continuous `if` statements, but in Ruby, you should use a filter list and method `include?` 
to implement it.

Here is a example in C/C++:

{% codeblock lang:c++ %}
	if( animal != tiger )
		if( animal != lion )
			if( animal != snake )
				cout << "It's a safe animal."
{% endcodeblock %}

We can implement the same logical issue in Ruby by creating a list.

{% codeblock lang:ruby %}
	dangerous_animal = [ tiger, lion, snake ]
	puts "It's a safe animal." if !dangerous_animal.include? (animal)
{% endcodeblock %}

Therefore, it's more redable for creating a filter list first, and also the list is easy to modify.



In Ruby language, there is no `do-while` like loop.  So the other way to reach the same effect is use `loop-do` and a terminated condition
 to  implement that.

Here is a example in C/C++:

{% codeblock lang:c++ %}
 	do{ 
		statement
 	}while( condition )
 {% endcodeblock %}

And in Ruby's way:

{% codeblock lang:ruby %}
 loop do
 	statement
 	break if statement
 end
{% endcodeblock %}


When I read some documents from Internet, they mentioned `do-while` loop is not necessary for Ruby.  Also, Matz doesn't want rubists to use this kind of logicals to solve problems.  In face, there are also potential problems for using `while` like loops.

