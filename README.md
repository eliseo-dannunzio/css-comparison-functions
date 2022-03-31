# css-comparison-functions

## So what's this all about? ##
Ever wonder if there was a way to implement value specific comparisons based on certain values of CSS variables?
The supplied code in this repo is a basic foundation for implementing such comparisons, all in pure CSS, no JS!

## How is this possible? ##
CSS already has two main functions, `min()` and `max()`, as well as `calc()` and `var()` functions. CSS variables are great, but I wanted to do some additional work to apply specific styles based on whether certain values in CSS variables are reached.

From these two main functions, you can create an `abs()` function, which in turn can be used to create a `GE()` (greater-than function) and an `IsZero()` function, which in turn can be used to create other functions down the line.

## What's this EPS value? ##
To create the `GE()` function which eventually paves the way for other functions to be calculated, a certain amount of error precision is required to help generate the required output. The EPS (Error Precision Setting) value is a small amount, usually smaller than the finest level of precision of the numbers to be used. In this repo, as I've used integers in this demonstration, a value of 10% of the finest level of precision (i.e. of 1), and thus 0.1 is used. If you want to use numbers that are finer than this level of precision, use an EPS of 10% of that. For example, if you want to use numbers where the finest level of precision is 0.1, (such as 0.2, 5.7, 8.9, etc.), you can use an EPS of `10% * 0.1 = 0.01`. If you want to use numbers where the finest level of precision is 0.01, (such as 0.25, 5.73, 8.96, etc.), you can use an EPS of `10% * 0.01 = 0.001`, and so on.

## Are there any limits to this? ##
As CSS `var()` functions are limited in browsers to a certain limit (roughly 100 nested `var()` calls), and calculated CSS variables reference all previously derived calculations and thus fall under this nested `var()` limit; there may be situations where a perfectly correct calculation may halt due to the limitations of the browser. The only thing you can do is find ways of optimizing your CSS code accordingly.

Another limitation is that these functions are not quite the "Define Once, Use multiple times" type like a standard JavaScript function. The examples in this repo are re-calculated once the radio buttons are updated and as a result the output is re-displayed with the updated values. If you're planning on implementing a similar function that performs the same comparison elsewhere, you can't reference the original function call. You have to define another instance of the comparison code and reference that new instance. Again the previous premise in the paragraph above applies.

## How can I implement these functions? ##
As you're creating your code, you have to think ahead with what you're planning to do. Take for example the following code:
```
var x = 5;
var y = -3;
if (x + y > 0) {
  z = 10;
} else {
  z = 20;
}
```

Fairly easy in JavaScript. But in the case of CSS you would need to define all relevant values and the respective calculated functions. All comparison functions will return a 1 if it satisfies the condition, and 0 if it doesn't satisfy the condition, enabling crude binary assignment values like how branchless coding works. You also have to work out your result first and then work backwards to determine what you need to define in order to derive your result at the end...

Within the current demonstration, there already is a CSS variable for `x+y` called `--xpy` which could be used to calculate the value of `z` in the above example as `--z` as follows:

```
:root {
  --eps: 0.1;
}
/* However --x and --y are defined from various CSS events is entered here... */

--xpy: calc(var(--x) + var(--y));
--xpyp1: calc(var(--xpy) + 1);
--xpym1: calc(var(--xpy) - 1);
--abs_xpy: calc(var(--xpy) - 2 * min(var(--xpy), 0));
--abs_xpyp1: calc(var(--xpyp1) - 2 * min(var(--xpyp1), 0));
--abs_xpym1: calc(var(--xpym1) - 2 * min(var(--xpym1), 0));
--zero_xpy: calc(1 - (var(--xpy) * var(--xpy) - var(--abs_xpyp1) * var(--abs_xpym1) + 1) / 2);
--XpYgt0: calc((max(var(--xpy), 0 - var(--eps)) + var(--eps)) / (var(--abs_xpy) + var(--eps)) - var(--zero_xpy));
--z: calc(10 * var(--XpYgt0) + 20 * (1 - var(--XpYgt0)));
```

As you can see, it can get pretty complicated, very fast... The above is a very basic example, and can be optimized further by refactoring certain values into one another.

## How can this be optimized? ##
The simplest way is to reduce the number of `var()` references in your calculations. Treat this as a lesson in algebra and find ways of consolidating the `var()` references, substituting known (as opposed to dynamic) values for variables. For example substituting `--eps` for 0.1 and variables like `var(--xpyp1)` and `var(--xpym1)` with `var(--xpy) + 1` and `var(--xpy) - 1`. The more substitutions you make with known static values, the more likely you will find that certain variables can be merged, consolidated or eliminated altogether, thus reducing the `var()` nesting limit.

## So if it's that complicated, why bother? ##
There are projects out there that push the limits of the encompassing environment that the projects inhabit, like creating a fully functional Gameboy with Pokemon Red in Minecraft using a modified texture pack, or creating Snakes and Ladders in CSS and HTML or creating a Linux emulator in pure JavaScript. This is a similar experiment to see the limits of what can be done to push the boundaries of what certain technologies can do based on the curernt limitations. Get inspired and see for yourself!
