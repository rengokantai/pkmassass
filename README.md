## pkmassass

###Chapter 4. CSS and HTML – SMACSS
Block Element Modifier (BEM)  
####The good and the bad of CSS grids
####Semantics
Writing semantics HTML simply means our elements should be used for the purposes they were intended. 
(tbc)

###Chapter 5. Advanced Sass
####Variables
#####!default
Ex
```
$primary-color: red !default; 
```
Saying if this $primary-color hasn't already been defined, then set it to red, otherwise use the previous value.  
Note: this would then be imported before any other partial files in your project:  
```
@import "config";  //your configfile
@import "otherfile"; 
```
#####Variable scope
######Local scope
the !global declaration only affect selectors declared after this declaration
```
$bg-color: red; 
%square { 
    width: 100px; 
    height: 100px;
} 
.square1 { 
    @extend %square; 
    background-color: $bg-color 
} 
.square2 { 
    $bg-color: blue !global; 
    @extend %square; 
    background-color: $bg-color 
} 
.square3 { 
    @extend %square; 
    background-color: $bg-color 
} 
```
will render
```
.square1, .square2, .square3 {
  width: 100px;
  height: 100px;
}

.square1 {
  background-color: red;
}

.square2 {
  background-color: blue;
}

.square3 {
  background-color: blue;
}
```
######Variable scope in functions
!global rule is same for functions  
Ex:
```
$bg-color: red; 
@function bg-color() { 
    @return $bg-color; 
} 
%square { 
    width: 100px; 
    height: 100px; 
} 
.square1 { 
    @extend %square; 
    background-color: $bg-color; 
}
.square2 { 
    @extend %square; 
    background-color: bg-color(); 
} 
.square3 { 
    $bg-color: orange; 
    @extend %square; 
    background-color: $bg-color; 
} 
.square4 { 
  $bg-color: blue !global; 
    @extend %square; 
    background-color: bg-color(); 
} 
.square5 { 
    @extend %square; 
    background-color: bg-color(); 
} 
```
is transplied to
```
.square1, .square2, .square3, .square4, .square5 {
  width: 100px;
  height: 100px;
}

.square1 {
  background-color: red;
}

.square2 {
  background-color: red;
}

.square3 {
  background-color: orange;
}

.square4 {
  background-color: blue;
}

.square5 {
  background-color: blue;
}
```
######Deeply nested variable scope
function change global scope
```
$bg-color: hsl(0, 100%, 25%); 
%square { 
    width: 100px; 
    height: 100px; 
} 
@function set-color() { 
    @if lightness($bg-color) > 20% { 
        $bg-color: red !global;     //this changed global $bg-color from hsl(0, 100%, 25%) to red
        @return black; 
    } @else { 
 
        @return white; 
    } 
} 
@mixin colors() { 
    background-color: $bg-color; 
    color: set-color(); 
} 
.square1 { 
    @extend %square; 
    @include colors; 
} 
 .square2 { 
    @extend %square; 
    @include colors; 
} .square3 { 
    @extend %square; 
    @include colors; 
} 
```
will be compiled to
```
.square1, .square2, .square3 {
  width: 100px;
  height: 100px;
}

.square1 {
  background-color: maroon;//first time, not changed
  color: black;
}

.square2 {
  background-color: red; //changed.
  color: black;
}

.square3 {
  background-color: red;
  color: black;
}
```
#####Making arrows in Sass
(tbc)
#####@content directive
######Using @content for media queries
[Need to memorize sass functions](http://sass-lang.com/documentation/Sass/Script/Functions.html#map_get-instance_method)  
memorize
```
map_remove
map_has_key
map_keys
map_values
map_merge
map_get
```
###Chapter 6. Gulp
####Setting up
```
compass create pkmassasspj6 --sass-dir=assets/scss --css-dir=assets/css
mv screen.scss style.scss //delete all other 5 css/scss files 
```
add npm scripts
```
"sass:watch": "sass --watch --compass --style=expanded --line-comments --line-numbers --sourcemap=auto assets/scss/style.scss:assets/css/style.css", 
"sass:compile": "sass --update --compass --sourcemaps=none --style=compressed --force assets/scss:assets/css" 
```
####Node.js, node-sass, and fileWatch
3 callbacks: before reading scss, before transpile to css, after write css
####Setting up Gulp
(tbc)
