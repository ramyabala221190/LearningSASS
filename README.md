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

We have then imported these 2 files into styles.scss using @use.

@use '_variables.scss' as *;
@use '_mixins.scss' as *;

The @use rule is intended to replace the old @import rule, but it’s intentionally designed to work differently. Here are some major differences between the two:

@use only makes variables, functions, and mixins available within the scope of the current file. It never adds them to the global scope. This makes it easy to figure out where each name your Sass file references comes from, and means you can use shorter names without any risk of collision.

@use only ever loads each file once. This ensures you don’t end up accidentally duplicating your dependencies’ CSS many times over.

@use must appear at the beginning your file, and cannot be nested in style rules.

Each @use rule can only have one URL.

@use requires quotes around its URL,


Multi line comments in .scss will appear in the .css
Single line comments in the .scss will not appear in the .css file.

All Sass identifiers, treat hyphens and underscores as identical. This means that reset-list and reset_list both refer to the same identifier. 
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

Instead of SASS variables, we have used CSS variables in the _variables.scss
Because it is defined inside :root, it is globally available everywhere.
It can be accessed using var().

:root{
    --danger-color:red;
    --success-color:green;
    --bg-color:lightblue;
}
In angular you can define something like this in the styles.scss file to be used
anywhere in the application

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


Passing variable no of arguments to a mixin

You can also pass multiple arguments to a mixin.
f the last argument in a @mixin declaration ends in ..., then all extra arguments to that mixin are passed to that argument as a list. This argument is known as an argument list. For example the last argument here is the border. Although the values in the border are space seperated,
you can also have comma seperated arguments. You can access the comma seperated arguments as list and iterate over them.

@mixin general-button($bg-color,$text-color,$border...){
    padding:5px;
    border-radius: 5px;
    background-color: $bg-color;
    color:$text-color;
    border:$border;
}

-------------------------------------------------------------------------------------------------------

Sass Inheritance using @extend

There are often cases when designing a page when one class should have all the styles of another class, as well as its own specific styles.

@extend .class-name

Ensure the base class is prepended by % so that it doesnt come in the .css file and also because the class is not used anywhere
in the html as well. Such classes are only meant to be extended.

%general-box{
    width:100%;
    height:auto;
    border:1px solid black;
    padding:5px;
    margin:5px;
}

%general-text{
    font-size:12px;
    text-decoration: underline;
}

.learning-mixin,.if-condition,.learning-inheritance,.for-condition{
    @extend %general-box;
 }
 
 .normal-text{
    @extend %general-text;
}

------------------------------------------------------------------------------------------------------
& in Nested rules

The & is an extremely useful feature in Sass (and Less). It’s used when nesting. It can be a nice time-saver when you know how to use it, 
or a bit of a time-waster when you’re struggling and could have written the same code in regular CSS.

The & is always the fully compiled parent selector.

--------------------------------------------------------------------------------------------------------

@extend and @content

Passing style content to a mixin using @content

@mixin general-button($bg-color,$text-color,$border...){
    border-radius: 5px;
    background-color: $bg-color;
    color:$text-color;
    border:$border;
    @content;
}

We are passing some styles to the mixin within the {}, which will replace @content;
So in the below example @content will be replaced with padding:10px;

.accept-btn{
    @include general-button($success-color,darkgreen,1px solid yellow){
        padding:10px
    }
}

The {} after the mixin name in the @include is only required if you want to pass some style content to the mixin.

-------------------------------------------------------------------------------------------------------
@if and @else condition

Using the IF condition inside the mixin to set the default font size for font size less than 5px

@mixin set-font-size($size){
   @if $size < 5px{
    font-size: 10px;
   }
   @else{
    font-size: $size;
   }
}

@if @else if and @else can be used to set styles based on some condition.

-------------------------------------------------------------------------------------------------------
FOR LOOP EACH Loop

.for-condition{
    @for $i from 1 through 7{
        div.color-box-#{$i}{
            width:100px;
            height:100px;
            @include set-rainbow-colors(#{$i},darkviolet,indigo,blue,green,yellow,orange,red)
      }  
    }
   
}

@mixin set-rainbow-colors($colorIndex,$colors...){
    //$colors... means that the remaining arguments are a list under the same variable $colors
   @each $color in $colors{
      $i: index($colors,$color); //get the index of the color in the list
      @if $colorIndex == #{$i} { //compare
        background-color: #{$color}; //set the background color
      }
   }
}


-------------------------------------------------------------------------------------------------------
Accessibility

https://dev.to/polgarj/a-short-guide-to-help-you-pick-the-correct-html-tag-56l9

When designing your web page, the first step is to define its regions. These are helpful for assistive technology users. They enable people to understand the overall structure of the content and move from section to section quickly.
Using the right HTML5 tags to structure your page makes it easier to understand and read using assistive technologies, creating a better user experience.

Using semantic HTML, in addition to ARIA attributes, you will indicate the regions that are dedicated to navigation, main content, and elements like the search bar, header, and footer.

Semantics in HTML is the use of meaningful tags. When you use an  article  tag for a blog article or a  nav  tag for your navigation links, you explicitly assign a role to this element.
By contrast, neutral tags like  div  or  span don't convey information about the content's role.

1. The  header  tag may contain the logo, a search bar, and a navigation menu.

It can also be used inside an  article  or  section  tag. In this case, the header is only associated with the section and not with the entire page.

2. Define a Navigation Region Using the Nav Tag. The  nav  tag, contains the links that navigate to other pages.

3. Define the Main Content Region Using the Main Tag
The content of the  main  region changes from page to page (unlike your navigation menu). The main region usually starts with an h1 level heading that describes the overall topic of the page.

4. Define a Section Using the Section Tag. A  section  tag contains a single subject or functionality. It usually starts with a heading.
While the  section  element technically has semantic meaning, this element's role isn’t communicated to assistive technologies unless it has an accessible name, such as an aria-label. 
This tag is used to split a page into sections like Introduction, Contact Information, Details, etc and each of these sections can be in a different <section> tag. The <section> tag is introduced to wrap-up the things in a particular section. The <section> tag divides the content into sections and subsections. The section tag is used when requirements of two headers or footers or any other section of documents needed. Section tag grouped the generic block of related content. 

Note: Placing <article> tag inside of <section> tag is good practice, like section basically defines the types and the articles will contain the specific contents in that type of section. 

5. Define Standalone Content Using the Article Tag
The  article  tag is not limited to a newspaper, magazine, or blog article. It can contain any standalone content, such as a cooking recipe.
<article> tag contains independent content that doesn’t require any other context. So the <article> tag can be placed inside the main content. But each of the articles will contain independent content within it. Like YouTube use to contain different kinds of videos on a single page, each video is independent or you can see the course page of GeeksforGeeks each course is independent, each course can have its own page. 

6. Define Complementary Content Using the Aside Tag
The  aside  tag contains content that is complementary to the main content.

7. Define a Footer Using the Footer Tag
The  footer  tag creates a concluding region for the entire page, or for a section or article.

As with navigation, if you have multiple footers on a page, you will have to add an aria-label to define how this region is different from the other footer tags on your page.

8. ARIA stands for Accessible Rich Internet Applications.

ARIA is a kind of markup that can be used with HTML to add accessibility information into your code. Assistive technologies can gather important information from ARIA attributes, and then relay this information to users.

According to the W3C (World Wide Web Consortium), ARIA attributes can define:

=>The role of the element (i.e., a navigation menu).

For example, the HTML  header  element can also be expressed with ARIA by using  role=”banner”  . In other cases, they can provide information beyond what is currently possible with HTML and define the utility or function of an element or group of elements more precisely. For example,  role=”search” , which enables the identification of a page section dedicated to search functionality, does not currently have an HTML equivalent. 

Possible value of roles:https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles

=>The state of the element (i.e., whether a dropdown menu is open or closed). The state usually indicates a characteristic of the element that can change.

=>The property of an element (i.e., a label that gives the element an accessible name). 

A web page may include several regions of the same type, for example, several  nav  or  aside  regions for complimentary content. In these cases, you need to provide more specific information by giving the region an accessible name. 

An  aria-label  allows you to give a region a name that isn’t visible on the page but can be read by assistive technologies and communicated to non-visual users who rely on screen readers and braille displays. 

aria-labelledby can be used to reference another element to define its accessible name, when an element's accessible name needs to use content from elsewhere in the DOM.

If there is no content that can be referenced to create an accessible name, the aria-label attribute should be used instead.

The following example uses aria-labelledby to provide an accessible name for a checkbox input by using the text content of a sibling element:

html
Copy to Clipboard
<span
  role="checkbox"
  aria-checked="false"
  tabindex="0"
  aria-labelledby="tac"></span>
<span id="tac">I agree to the Terms and Conditions.</span>

You can use the aria-label to:

Identify multiple navigation menus:  aria-label="Primary Navigation"  and  aria-label="Secondary Navigation" .

Distinguish several zones of complementary content:aside  or  ARIA role=”region” . For example, on a recipe website you could dedicate one aside to utensils and another to advertising content:aria-label=”equipment you’ll need” and aria-label=”advertisement”.

9. Why ARIA ?

Modern versions of popular browsers (Firefox, Chrome, Safari) support HTML5 very well. They do not require ARIA attributes to understand the meaning and role of a tag when HTML and ARIA give equivalent information. However, in some cases having both HTML and ARIA provides broader support.

For accessibility, it is always important to think about the largest number of users, the browsers, and the tools they use. For example, it may be that:

Your users are not using modern browsers because they are not authorized to do so. In large companies, the IT department configures the machines, leaving no choice. 

Their hardware is old and does not allow recent versions of major browsers.

