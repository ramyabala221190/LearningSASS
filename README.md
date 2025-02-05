SASS-Syntactically Awesome Style Sheets

Why SASS over CSS ?

1. Reduced maintainance cost because of reusability
2. Lesser boilerplate
3. Better organisation
4. Improved readability
5. Reduced Duplication
6. Calculated values

A .scss file is nothing but CSS + SASS features.
The .scss file is run through  sass, which is a css preprocessor. 
This sass preprocessor will produce a regular .css file.
-----------------------------------------------------------------------------------------------------------

Features of SASS

1.Variables
Define value once in the variable and reuse the variable multiple times.
No need to hardcode values.
Reduced maintainance.

$variable_name:variable_value;

2. Style inheritance
3. Splitting our styles into multiple files
4. Nested Rules
5. Mixins
6. Functions

-----------------------------------------------------------------------------------------------------------

To enable VSC to process .scss files, we need to install the "Live Sass Compiler" extension.

As a convention, Sass files that are only meant to be imported, not compiled on their own, begin with _ (as in _code.scss). These are called partials, and they tell Sass tools not to try to compile those files on their own. You can leave off the _ when importing a partial.

We have defined mixins and variables in 2 seperate files 
_mixins.scss and _variables.scss

We have then imported these 2 files into styles.scss using @import.

@import '_variables.scss';
@import '_mixins.scss';


Multi line comments in .scss will appear in the .css
Single line comments in the .scss will not appear in the .css file.
-----------------------------------------------------------------------------------------------------------

Using Variables

A sass variable begins with $.
A variable declaration looks a lot like a property declaration: it’s written <variable>: <expression>.
It avoids repeating the same hardcoded text in multiple places.
Instead replace the text with a variable, where the variable is 

$base-color: #c6538c;
$border-dark: rgba($base-color, 0.88); // calculating based on $base-color

.alert {
  border: 1px solid $border-dark; //using the variable
}

-----------------------------------------------------------------------------------------------------------
Mixins 

Mixins allow you to define styles that can be re-used throughout your stylesheet.
Mixins can also take arguments, which allows their behavior to be customized each time they’re called. 

Mixins are very useful for dynamic styles. 

For example, in the Accept and Cancel buttons, we have customized the background color of the button
by passing the variable containing the color to be used for each button, as argument to the mixin.

This is how a mixin is defined.  @mixin some-mixin-name(...arguments){}

@mixin general-button($bg-color){
    padding:5px;
    border-radius: 5px;
    background-color: $bg-color;
}

We include the mixin using the @include. Observe how we are adding dynamic styles by passing arguments
to the mixin.

.accept-btn{
    @include general-button($success-color)
}

.cancel-btn{
    @include general-button($danger-color)
}
-------------------------------------------------------------------------------------------------------

Sass Inheritance using @extend

There are often cases when designing a page when one class should have all the styles of another class, as well as its own specific styles.

@extend .class-name

------------------------------------------------------------------------------------------------------
