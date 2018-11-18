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


<a name="mixins"></a>
## 3. Mixins

sometext

<a name="extend"></a>
## 4. @extend
