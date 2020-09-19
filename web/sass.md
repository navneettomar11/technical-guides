# Introduction
Sass can extend the CSS language because it is a preprocessor. It takes code written using Sass syntax, and convert it into basic CSS. This allows you to create variables, nest CSS rules into others, and import other Sass files other things. The result is more compact, easier to read code.

There are two syntaxes available for Sass. The first known as SCSS(Sassy CSS) and used throughtout these challenges, is a extension of the syntax of CSS. This means that every valid CSS stylesheet is a valid SCSS file with the same meaning. File using this syntax have the `.scss` extension.

The second and older syntax, known as the indented syntax(or sometimes just 'Sass'), used indentation rather than brakets to indicate nesting of selectors, and newlines rather than semicolons to separate properties. File using this syntax have the `.sass` extension.

## Store Data with Sass Variables
One feature of Sass that's different than CSS is it uses variables. They are declared and set to store data, similar to Javascript.

In Javascript, variables are defined using the `let` and `const` keywords. In Sass, variables start with a `$` followed by the variable name.

Here are couple examples:
```css
$main-fonts: Arial, san-serif;
$heading-color: green;
//To use variables
h1 {
    font-family: $main-fonts;
    color: $heading-color;
}
```

## Nest CSS with Sass
Sass allow nesting of CSS rules which is useful way of organizing a stylesheet.

Normally, each element is targeted on a different line to style id, like so:
```css
nav {
  background-color: red;
}

nav ul {
  list-style: none;
}

nav ul li {
  display: inline-block;
}
```
For a large project, the CSS file will have many lines and rules. This is where nesting can help organize your code by placing child styles rules within the respective parent elements.

```css
nav {
  background-color: red;

  ul {
    list-style: none;

    li {
      display: inline-block;
    }
  }
}
```

## Reusable CSS with Mixins
In Sass, a mixin is a group of CSS declarations that can be reused throughout the stylesheet.

Newer CSS features take time before they are fully adopted and ready to use in all browsers. As features are added to browsers, CSS rules using them may need vendor prefixes. Consider "box-shadow":
```css
div {
  -webkit-box-shadow: 0px 0px 4px #fff;
  -moz-box-shadow: 0px 0px 4px #fff;
  -ms-box-shadow: 0px 0px 4px #fff;
  box-shadow: 0px 0px 4px #fff;
}
```
It's lot of typing to re-write this rule all the elements that have a `box-shadow`, or to change each value to test different effects. Mixins are like function for CSS. Here is how to write one:
```css
@mixin box-shadow($x, $y, $blur, $c) {
    -webkit-box-shadow: $x $y $blur $c;
    -moz-box-shadow: $x $y $blur $c;
    -ms-box-shadow: $x $y $blur $c;
    box-shadow: $x $y $blur $c;
}
```

The definition starts with `@mixin` followed by a custom name. The parameters (th $x, $y, $blur, and $c) are optional. Now any times a `box-shadow` rule is needed, only a single line calling the mixin replaces having to type all the vendor prefixes. A mixin is called with the `@include` directive.
```css
div {
  @include box-shadow(0px, 0px, 4px, #fff);
}
```

## Use @if and @else to Add logic to your style
The `@if` directive in Sass is useful to test for a specific case - it works just like the `if` statement in Javascript.

```css
@mixin make-bold($bool) {
    @if $bool == true {
        font-weight: bold;
    }
}
```

And just like in Javascript, `@else if` and `@else` test for more conditions:

```css
@mixin text-effect($val) {
  @if $val == danger {
    color: red;
  }
  @else if $val == alert {
    color: yellow;
  }
  @else if $val == success {
    color: green;
  }
  @else {
    color: black;
  }
}
```

## Use @for to create a Sass Loop
The `@for` directive adds styles in a loop, very similar to a `for` loop in javascript.

`@for` is used in two ways "start through end" or "start to end". The main difference is that the "start to end" excludes the the end number as part of the count and "start through end" includes the end numbers as part of the count.

Here's start through end example:
```css
@for $i from 1 to 12 {
    .col-#{$i}  { width: 100% / 12 * i;}
}
```

The `#{$i} part is the syntax to combine a variable (i) with text to make a string. When the Sass file is converted to CSS, it look like this:

```css
.col-1 {
  width: 8.33333%;
}

.col-2 {
  width: 16.66667%;
}

...

.col-12 {
  width: 100%;
}
```

This is powerful way to create a grid layout. Now you have twelve options for coulmns widths avialable as CSS classes.

## Use @each to Map over items in a List
Sass also offer the `@each` directive which loops over each item in a list or map. On each iteration the variable get assigned to the current value from the list or map.

```css
@each $color in blue, red, green {
  .#{$color}-text {color: $color;}
}
```
A map has slightly different syntax. Here's an example:
```css
$colors: (color1: blue, color2: red, color3: green);

@each $key, $color in $colors {
  .#{$color}-text {color: $color;}
}
```

Note that the `$key` variable is needed to reference the keys in the map. Otherwise, the compiled CSS would have color1, color2... in it. Both of the above code examples are converted into the following CSS:

```css
.blue-text {
  color: blue;
}

.red-text {
  color: red;
}

.green-text {
  color: green;
}
```

## Apply a style Until a condition is Met with @while
The `@while` directive is an option with similar functionality to the Javascript `while` loop. It creates CSS rules until a condition is met.

The `@for` challenge gave an example to create a simple grid system. This can also work with `@while`.

```css
$x: 1;
@while $x < 13 {
  .col-#{$x} { width: 100%/12 * $x;}
  $x: $x + 1;
}
```

First, define a variable $x and set it to 1. Next, use the `@while` directive to create the grid system while `$x` is less than 13. After setting the CSS rule for `width`, `$x` is increment by 1 to avoid an infinite loop.

## Split your styles into smaller chunks with partials
Partials in Sass are seperates files that hold segmeent of CSS code. These are imported and used in other Sass files. This is great way to group similar code into a module to keep it organized.

Names for partials start with the underscore `(_)` character, which tells Sass it is small segmenet of CSS and not to convert into a CSS file.  Also, Sass files end with the `.scss` file extension. To bring the code in the partial into another Sass file, use the `@import` directive.

For example, if all your mixins are saved in a partial names "_mixins.scss" and they are needed in the "main.scss" file, this is how to use them in the main file:

```css
// In the main.scss file

@import 'mixins
```

Note that the underscore and file extension are not needed in the `import` statement - Sass understands it is partial.. Once partial is imported into a file, all varialbes, mixins and other code are available.


## Extend One Set of CSS styles to Another element.
Sass has a feature called `extend` that makes it easy to borrow the CSS rules from one element and build upon them in another.

For example, the below block of CSS rules style a `.panel` class. It has a `background-color`, `height` and `border`.

```css
.panel{
  background-color: red;
  height: 70px;
  border: 2px solid green;
}
```

Now you want another panel called `.big-panel`. Ut has the same base properties as `.panel`, but also needs a `width` and `font-size`. It's possile to copy and paste the initial CSS rules from `.panel`, but the code become repetitive as you add more types of panels. The `extend` directive is simple way to reuses the rules written for one element, then add more more for another.

```css
.big-panel{
  @extend .panel;
  width: 150px;
  font-size: 2em;
}
```

The `.big-panel` will have the same properties as `.panel` in addition to the new styles.
