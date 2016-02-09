---
layout: post
title:  "Beginning to learn javascript"
date:   2015-08-17
categories: [javascript, study]
tags:
- javascript
---

Currently going through [Eloquent Javascript: The Annotated Version](http://watchandcode.com/courses/eloquent-javascript-the-annotated-version/ "Eloquent Javascript: The Annotated Version")

These are my notes on chapter 1.

### Values, Types, and Operators
---

* Fractional calculations are not as precise as whole number calculations.
* Straightforward String escape characters like

	```
	"Hello,\n Testing \\, One\n\t, Two \"Three\""
	```

	Becomes

		Hello,
		Testing \ One  
			Two "Three"

* ```concatenation``` is done via



		"se" + "nd" + " " + "flow" + "ers"

	Becomes

		"send flowers"

* ```unary``` and ```binary``` operators.
	- ```unary``` operators only take one value.
	- ```binary``` operators take two values.
	- ```ternary``` operators take three values.

* ```typeof``` operator gives a string value defining the type of the given value

		console.log(typeof 4.5)
		// -> number

* ```boolean``` types are defined as standard true/false
* standard comparison operator

		console.log(3 == 5)
		// -> false
		console.log("family" = "family")
		// -> true
* Standard ```||, &&, and !``` logical operators that equal, and not respectively.
* ```tenary/conditional operator```: Takes three values. and outputs the left or right value
	depending on whether or not the value left of the question mark is true or false.

		console.log(true ? 1 : 2);
		// → 1
		// left value is outputted because the value is true
		console.log(false ? 1 : 2);
		// → 2
		// right value is outputted because the value is false.
		console.log((100 > 1) ? "true" : "false")
		// -> "true"
		// true is output because 100 > 1 statement is true

Basically its a one line statement for an if else statement. where the if is left of ```:``` and else is right of ```:```

* Undefined values are ```null``` and ```undefined```

### Oh shit Javascript, what are you doing?
---

Apparently there is a thing called ```type``` coercion that allows things like

	console.log(8 * null)
	// -> 0
	console.log("5" - 1)
	// -> 4
	console.log("5" + 1)
	// -> 51
	console.log("five" * 2)
	// -> NaN
	console.log(false == 0)
	// -> true


To happen. Thanks Javascript.

Basically Javascript converts values to types via its own rules unless it can't. In which case it converts the values to NaN.

Additionally, If you try to do arithmetic operations on NaN it will keep producing NaN so if NaN is an error that you get then checking for accidental type conversions might solve the problem.

* You can test if a value is a real value or not by comparing it to null.

		console.log(null == undefined);
		// → true
		console.log(null == 0);
		// → false

* ```0```, ```NaN``` and ```("")``` count as ```false``` according to Javascript while everything else is ```true```

### Important Note about type conversions
---


```===``` comparison checks whether or not two values are precisely equal to each other and should be used defensively to prevent accidental type conversions.

		console.log(false === 0)
		// -> false

As it should

* ```||``` and ```&&``` operators try to convert the left side of comparisons to a boolean value if possible
if both values are not boolean values themselves.

	* ```||``` will output the left value in it's original type in a comparison if it can be converted to true but otherwise outputs the right value.
			console.log(null || "user")
			// -> "user"
			console.log("salt" || "mines")
			// -> "salt"

	* ```&&``` will do the opposite and give the right value if convertable but will otherwise output the left if not.
			console.log("salty" && "bet")
			// -> "bet"
			console.log(null && "aptitude")
			// -> null


* ```Short-circuit evaluation```: Additional notes to ```||``` and ```&&``` are that they will not evaluate expressions if they can get their first choice value as something valid i.e. ```true || X``` will output true and ignore X while ```false && X``` will output false
