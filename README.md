# SASS-Tutorial

Table of contents
=================

1. [Variable](#varibles)
2. [Importing Files and Partials](#imports-and-partials)
3. [Mixins](#mixins)
4. [@extend](#extend)

<a name="varibles"></a>
## 1. Variable

Variables can help keep things consistent throughout a website. Instead of repeating the same values across many different properties, you can simply set a variable, then refer to it whenever you need to. If you need to change the value, you only need to do it in one place.

If we look at the following Sass file, it contains two variables.

```scss
$primary-color: orange;
$secondary-color: gold;
​
body {
  color: $primary-color;
  background: $secondary-color;
}
```
One variable is called **$primary-color** and the other is called **$secondary-color**.

Variables are like containers that store a value. In our case, we store the value orange in one variable and **gold** in the other.

Instead of typing out orange everytime we need to use that color, we can use the variable name instead. If we need to update a color, we only need to update it in the place that we set the variable.

### Variable Syntax
Variables start with a dollar sign ($) followed by the variable name.

Variables are set like CSS properties — using a colon (:) between the variable name and the value being assigned to it.

The variable name is whatever you want to call it. You could call the variable $homer-simpson and it would still work.

Note that variables are written the same whether you use the indented syntax or the SCSS syntax.

### Hyphens & Underscores
Variable names can use hyphens and underscores interchangeably. So our variable called $primary-color could be accessed using $primary_color and vice versa.

So our file could look like this and it would still work:

```scss
$primary_color: orange;
$secondary_color: gold;
​
body {
    color: $primary-color;
    background: $secondary-color;
}
```

### Nesting
Variables can also be set within a selector. Like this:

```scss
header {
    $header-color: orange;
    color: $header-color;
}
```
However, such variables are only available within the level of nested selectors where theyâ€™re defined. But if theyâ€™re defined outside of any nested selectors, theyâ€™re available everywhere.

So we couldn't do this:

```scss
header {
    $header-color: orange;
    color: $header-color;
}
article {
    color: $header-color;
}
```
If you tried to compile that code, you'd receive an error that reads something like this: Error: Undefined variable: "$header-color"

### The !global Flag
However, the above example would work if you added the !global flag when setting the variable.

So you can do the following:

```scss
header {
    $header-color: orange !global;
    color: $header-color;
}
article {
    color: $header-color;
}
```
This code will be compiled to the following CSS:

```css
header {
    color: orange; }
​
article {
    color: orange; }
    
```
### Variable Values
The variable can hold any CSS value we want. We could create a variable to hold font families, font weight, border widths, background images, etc.

Here's an example of some more variables added to the Sass file.

```scss
$primary-color: orange;
$secondary-color: gold;
$font-stack: 'Open Sans', Helvetica, Sans-Serif;
$border-thick: 10px;
$border-bright: orange;
$border-style: solid;
$article-width: 60%;
$article-padding: 20px;
$article-margin: auto;
​
body {
    color: $primary-color;
    background: $secondary-color;
    font-family: $font-stack;
}
article {
    border: $border-thick $border-style $border-bright;
    width: $article-width;
    padding: $article-padding;
    margin: $article-margin;
}

```
This will result in the following CSS file:

```css
body {
  color: orange;
  background: gold;
  font-family: "Open Sans", Helvetica, Sans-Serif; }
​
article {
  border: 10px solid orange;
  width: 60%;
  padding: 20px;
  margin: auto; }

/*# sourceMappingURL=styles.css.map */
```
### Default Values
You can assign a default value to a variable, which will only be used if the variable doesn't have a value.

To do this, add the !default flag to the end of the assignment. Like this:

```scss
$primary-color: orange;
$primary-color: gold !default;
​
body {
  color: $primary-color;
}
```
In this particular case, the compiled CSS will be as follows:

```css
body {
  color: orange; }
  ```
The second (default) color wasn't used in this case, because a value had already been assigned to that variable.

But if we were to remove the first line, the second (default) value would be used.

So if our SCSS file looks like this:

```scss
$primary-color: gold !default;
​
body {
  color: $primary-color;
}
```
The compiled CSS will look like this:
```scss
body {
  color: gold; }
```
<a name="imports-and-partials"></a>
## 2. Importing Files and Partials

In this, we'll look at how SASS lets you keep your CSS code DRY (Don't Repeat Yourself).

### @import

Anything you can do in CSS you can do in Sass, and that includes using the @import directive. But as you've probably come to expect, Sass extends @import in some useful ways. To start with, it supports inclusion of .sass and .scss files. The basic syntax is the same:
```scss
Syntax:
@import <file name>
```
For example, **@import "colors"** will include colors.scss or colors.sass in the CSS ouput, if exists in the current directory. Notice that this is slightly different from the CSS @import directive: The file is included in the CSS; no extra http call will be required at runtime. This avoids one of the big performance problems with the standard CSS @import.

You can override the default behavior in several ways. Each of the following, for example, will result in a standard CSS @import directive being output in the CSS:

#### Examle: @import
```scss
@import "colors.css";                 //the .css extension is specified
@import http://test.com/colors.css;  //the http:// prefix is used
@import "colors" screen;             //the import statement includes a media query
@import url(colors);                 //the url() function is used
```

The resulting CSS would include the following statements:

#### CSS:
```css
@import "colors.css";               
@import http://test.com/colors.css;
@import "colors" screen:
@import url(colors); 
```

You'll normally use the @import directive at the top of a file where its contents will have global scope. But with a few exceptions, you can also nest @import statements. You can't use nested imports within mixins (discussed later in next chapter) or inside control statements, but otherwise you can use @import wherever you need it.

### Partials

One of the best uses of the Sass @import directive is to create separate files containing all the standard definitions for your site-colors, font sizes and families and so forth-and then include them where you need them. This is, of course, a classic example of DRY programming.

Since the sole purpose of these files is to be included in other Sass files, there's no point in transpiling them directly. In fact, many of them won't actually produce any CSS. An example of this is a file that includes only variable definitions: It would result in an empty CSS file.

SCSS:
```scss
$red: #ff0000;
$green: #00ff00;
$blue: #0000ff;
```
If the Sass transpiler is watching a directory (either through the command window or via an editor extension), you'll want to exclude changes to these files from transpilation, and Sass makes it easy to do so. Just prepend an underscore to their names. Instead of colors.scss, for example, name the file _colors.scss. A file that The transpiler will ignore it, but you can still include it in other Sass files.

Files like these are known as "partials", and you can include them in another .scss file in the normal way. By convention, the leading underscore is omitted, but you can include it if you wish. As we'll see:

Sass:
```scss
@import "colors";   //by convention, omit the underscore
@import "_colors";  //but this works, too
```
Structuring your CSS in this way doesn't have any performance penalties- you'll get clean, stand-alone CSS at the end of the transpilation process and it makes your code less repetitive and easier to maintain. In fact, if you examine CSS frameworks like Bootstrap 4 or MaterializeCSS you'll often find a single SCSS file that contains nothing but @import statements. All the other files are partials.

<a name="mixins"></a>
## 3. Mixins

In recent years HTML has made great strides towards become more semantic. Tags like <aside> and <article> enforce the meaning of the content rather than its layout. Unfortunately, the same isn't true of CSS. Defining classes like .float-left, .row and .col is better than re-defining float properties for every HTML tag, but they hardly adds to the meaning of the HTML.

There are CSS frameworks around they try to address the problem. (Semantic UI is a popular example; you'll find it at semantic-ui.com.) Sass provides another way, via the *@mixin* directive. Mixins are primarily used to provide non-semantic styling, but they can contain any valid CSS or Sass. The syntax is straight-forward:

Syntax:
```scss
@mixin <name>
{
    <contents>
}
```
Once you've created the mixin, simply use @include where you want the rules to be included in your file.

Syntax:
```scss
<selector>
{
    @include <mixin-name>

    [<other rules>]

}
```

Let's look at a simple example.

SCSS: Mixin
```scss
@mixin float-left {
   float: left;
}

.call-out {
  @include float-left;
  background-color: gray;
}
```
The resulting CSS will be:

CSS:
```css
.call-out {
   float: left;
   background-color: gray;
}
```
The mixin in the above example is defined in the same file where it's used, but you can (and usually will) define mixins in a partial. Just @import the file itself before you use the @include directive.

A Sass mixin isn't restricted to just defining property rules; it can also include selectors, including parent selectors. Using this technique, you could, for example, define all the rules for a button-like link in a single mixin:

SCSS:
```scss
@mixin a-button {
   a {
      background-color: blue;
      color: white;
      radius: 3px;
      margin: 2px;
      
      &:hover {
         color: red;
      }

      &:visited {
         color: green;
      }
   }
}
```

Use the @include directive to include this mixin in a style, as shown below:

SCSS: @include Mixin
```scss
@include a-button.scss //assuming a-button.scss is the name of above mixin file

.menu-button {
   @include a-button;
}
```
and the output would be:

CSS:
```css
.menu-button a {
   background-color: blue;
   color: white;
   radius: 3px;
   margin: 2px;
}
.menu-button a:hover {
   color: red;
}
.menu-button a:visited {
   color: green;
}
```
#### Mixin Variables
If you have more than one class that includes this functionality, it's clear that previous exmample will save a lot of typing and be more maintainable. But what if you have several classes that have the same basic functionality, but you need to pass different colors? Sass makes it easy: just pass the colors as variables, which are defined like function parameters:

SCSS: Mixin Variable
```scss
@mixin a-button($base, $hover, $link) {
   a {
      background-color: $base;
      color: white;
      radius: 3px;
      margin: 2px;
      
      &:hover {
         color: $hover;
      }

      &:visited {
         color: $link;
      }
   }
}
```
You pass the variable arguments to the mixin using the normal syntax:

SCSS:
```scss
.menu-button {
   @include a-button(blue, red, green);
}
.text-button {
   @include a-button(yellow, black, grey);
}
```
That example would result in the following CSS:

CSS:
```css
.menu-button a {
   background-color: blue;
   color: white;
   radius: 3px;
   margin: 2px;
}
.menu-button a:hover {
   color: red;
}
.menu-button a:visited {
   color: green;
}
.text-button a {
   background-color: yellow;
   color: white;
   radius: 3px;
   margin: 2px;
}
.text-button a:hover {
   color: black;
}
.text-button a:visited {
   color: grey;
}
```
Sass even lets you provide default values for mixin variables:

SCSS:
```scss
@mixin a-button($base: red, $hover: green, $link: blue) {
   a {
      background-color: $base;
      color: white;
      radius: 3px;
      margin: 2px;

      &:hover {
         color: $hover;
      }

      &:visited {
         color: $link;
      }
   }
}
```
When you define a default variable in this way, you only need to provide values that change when you include the mixin:

SCSS:
```scss
.menu-button {
   @include a-button($link: orange);
}
```
This would result in the following CSS:

CSS:
```css
.menu-button a {
   background-color: blue;
   color: white;
   radius: 3px;
   margin: 2px;
}
.menu-button a:hover {
   color: red;
}
.menu-button a:visited {
   color: orange;
}
```
As you can see, the a:visited rule is assigned color:orange, as provided, but the other two rules have the default values.

Just as when you call a Sass function, you only need to provide the parameter name if you're not including the arguments out of order or skipping some. This would work just fine (although the resulting button would be pretty ugly):

SCSS:
```scss
.menu-button {
    @include a-button(darkmagenta, darkolivegreen, skyblue);
}
```
#### Variable Variables
Some CSS properties can take different numbers of variables. An example is the margin property, which can take from 1 to 4 values. Sass supports this using variable arguments, which are declared with an ellipsis after their names:

SCSS: Mixin Variables
```scss
@mixin margin-mix($margin...) {
   margin: $margin;
}
```
Given this mixin definition, any of the following @include directives will transpile without error:

SCSS:
```scss
.narrow-border {
   @include margin-mix(1px);
}

.top-bottom-border {
  @include margin-mix(3px 2px);
}

.varied-border {
   @include margin-mix(1px 3px 6px 10px);
}
```
and with the expected results:

CSS:
```css
.narrow-border {
  margin: 1px; 
}

.top-bottom-border {
  margin: 3px 2px;
}

.varied-border {
  margin: 1px 3px 6px 10px; 
}
```
Passing Content to Mixins
Most often you'll use mixins for standard bits of styling that you'll use in multiple places and expand with additional rules when you include them. But you can build mixins that work the other way by passing a block of rules to the mixin. You don't need a variable to do this; the content you pass will be available to the mixin via the @content directive:

SCSS:
```scss
@mixin has-content {
   html {
      @content;
   }
}

@include has-content {
   #logo {
       background-image: url(logo.svg);
   }
}
```
This will result in the following CSS output:

CSS:
```css
html #logo {
   background-image: url(logo.svg);
}
```
Notice the syntax here uses braces, not parentheses, to distinguish the content from any variables the mixin might declare. You can use both;

SCSS:
```scss
@mixin test-content($color) {
   .test {
      color: $color;
      @content;
    }
}

@include test-content(blue) {
   background-color: red;
}
```
which will result in the following CSS:

CSS:
```css
.test {
   color: blue;
   background-color: red;
}
```
Passing content into a mixin isn't something you'll need to do very often, but it can be useful, particularly when you're dealing with media queries or browser-specific property names.


<a name="extend"></a>
## 4. @extend
