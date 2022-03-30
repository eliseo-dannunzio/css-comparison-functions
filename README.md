# css-comparison-functions

## So what's this all about? ##
Ever wonder if there was a way to implement value specific comparisons based on certain values of CSS variables?
The supplied code in this repo is a basic foundation for implementing such comparisons, all in pure CSS, no JS!

## How is this possible? ##
CSS already has two main functions, `min()` and `max()`, as well as `calc()` and `var()` functions. CSS variables are great, but I wanted to do some additional work to apply specific styles based on whether certain values in CSS variables are reached.

From these two main functions, you can create an `abs()` function, which in turn can be used to create a `GE()` (greater-than function) and an `IsZero()` function, which in turn can be used to create other functions down the line.

## What's this EPS value? ##
To create the `GE()` function, a certain amount of error precision is required to help generate the required output. The EPS (Error Precision Setting) value is a small amount, usually smaller than the finest level of precision of the numbers to be used. In this repo, as I've used integers in this demonstration, a value of 10% of the finest level of precision (i.e. of 1), and thus 0.1 is used. If you want to use numbers that are finer than this level of precision, use an EPS of 10% of that. For example, if you want to use numbers where the finest level of precision is 0.1, (such as 0.2, 5.7, 8.9, etc.), you can use an EPS of `10% * 0.1 = 0.01`. If you want to use numbers where the finest level of precision is 0.01, (such as 0.25, 5.73, 8.96, etc.), you can use an EPS of `10% * 0.01 = 0.001`, and so on.

## Are there any limits to this? ##
As CSS `calc()` functions are limited in browsers to a certain limit (roughly 100 nested `calc()` calls), and calculated CSS variables reference all previously derived calculations and thus fall under this nested `calc()` limit; there may be situations where a perfectly correct calculation may halt due to the limitations of the browser. The only thing you can do is find ways of optimizing your CSS code accordingly.

Another limitation is that these functions are not quite the "Define Once, Use multiple times" type like a standard JavaScript function. The examples in this repo are re-calculated once the radio buttons are updated and as a result the output is re-displayed with the updated values. If you're planning on implementing a similar function that performs the same comparison elsewhere, you can't reference the original function call. You have to define another instance of the comparison code and reference that new instance.
