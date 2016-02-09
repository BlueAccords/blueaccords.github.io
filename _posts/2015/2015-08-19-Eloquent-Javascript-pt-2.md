---
layout: post
title:  "Eloquent Javascript: part 2"
date:   2015-08-19
tags:
- javascript
---

note to self: look up rules for defining variables.

### Program Structure

* Assignment of values uses ```var age = 22``` syntax.

* The default value of an a ```var``` that has no value assigned to it is ```undefined```.

* Variable names are case-sensitive so ```TheDanger``` != ```thedanger```.

* The following is a list of reserved keywords that cannot be used as variables:
		break case catch class const continue debugger
		default delete do else enum export extends false
		finally for function if implements import in
		instanceof interface let new null package private
		protected public return static super switch this
		throw true try typeof var void while with yield

* ```continue``` : used to exit the current iteration of a loop but continue onto the next one.

* ```break``` : exits the current loop completely.

* ```switch``` statement is used for if else loops that have multiple outcomes.

		switch(age) {
			case(18):
				console.log("adult");
				break;
			case(64):
				console.log("senior");
				break;
			default:
				console.log("senior")
		}

Note: ```switch``` is used to compare the given value to cases of other values. Trying to use logical

operators will NOT work UNLESS you compare the value true to operators such as ```age < 64```

* ```camelCase``` is usually used to define variables. ex: ```theOneAndOnlyShazam```.

* ```comments``` use the following syntax:

		// for single lines and
		/* Is used for
		multiple lines of
		commenting
		*/
