<span class="blue-text-bold">inline style</span> - A style that's placed inside a tag
<span class="blue-text-bold">selector</span> - An html tag you have selected to modify
```css
h2{
	font-size: 32px;
	color: forestgreen;
}
```

#### CSS Rules:
1. Style declarations are wrapped in curly braces and comes after selector
2. A set of curly braces can contain more than one property-value pairs
3. Each pair should be on a separate line and end with a semicolon
4. CSS rules are applied top to bottom. If applied more than once, the last one will be applied

\<link rel="stylesheet" href="style.css"/>            // Linking a stylesheet
\<div>\</div> helps divide parts of the content  // Spans width of webpage by default
\<span>\</span> Acts as a small container that helps you style specific text within a larger block

#### Margin vs Padding:
![[1. Margin vs Padding]]
\<header>\</header> like \<div> but for introductory section of your webpage
height: 100vh;  // vh stands for viewport height. 100vh is 100% of the visible screen height;
```css
body, h1{
	margin: 0;
}
```
background-color: rgb(255, 0, 0);  // There's also rgba for transparency (0-1) alpha value
background-image: url(link);  // Adds a background image with url() style with a container element
url() - method is required with CSS syntax it is not optional
background-repeat: no-repeat;
                repeat;
				repeat-x;
				repeat-y;
background-position: center;  // Sets margins to center background
background-size: contain  // Scales a background image to fit its container keeping aspect ratio
overlay - additional layer placed on top of something else

#### Inheritance in CSS:
* All styles will be inherited to the child element
font-weight: bold;  // Makes text bold
letter-spacing: -1.6px;  // Reduce spacing between letters
\<main>\</main> - used for the main content specific to a webpage
assistive technologies - Tools / software that help people with disabilities interact with web content
\<div class="overlay"> creates an overlay class and changes the selector to .overlay
class -> .
<!-- comments in html -->  /* comments in css*/

#### Top vs padding-top:
* padding-top adds pixels of padding to the top of the context
* Used with position property to move elements
```css
.example-top{
position: relative; /* or absoluted/fixed */
top: 20px; /* Moves element 20 px down from its normal position */
}
```

#### Single line padding:
* padding-top: 12px; 
* padding-right: 16px;
* padding-bottom: 0;
* padding-left: 16px;
* padding: 12px 16px 0 16px;
* padding: top right bottom left (clockwise)
* padding: 12px 16px 0; (if left & right are the same)

```css
border: dashed 5px rgba(250, 50, 50, .4);  /* Adds a border */
border-radius: 8px;  /* Rounded border (Can also use 50% for a circle) */
overflow: hidden;  /* Hides everything that overflows outside a container */
box-sizing: content-box; /* width and height only applies to the content */
box-sizing: border-box;  /* Size has width height + padding + border */
display: inline-block;  /* Can set width/height, all margins work */
display: block;  /* Takes full width of webpage and forces newline after */
display: inline;  /* Can't set width/height only horizontal margins work. */
```

\<div class="card no-right-margin">  // Element with 2 classes

#### Shadows:
```css
div{
	box-shadow: -2px 2px 5px rgba(0, 0, 0, 0.4);
	/* shifted 2 pixels left 2 pixels down 5 pixels blur radius grayish color */*
}
div{
	text-shadow: -2px 2px 5px rgba(0, 0, 0, 0.4); 
}
```
\* In almost all cases elements with children should not be given a fixed height

#### VS Code keyboard shortcuts:
* Ctrl + P - Finding files
* Ctrl + \` - Toggles integrated terminal
* Ctrl + \\ - Splits the editor window
* Alt + Click - Inserts an additional cursor
* Ctrl + Shift + ↑ - Inserts an additional cursor above the current cursor
* Ctrl + Shift + ↓ - Inserts an additional cursor below the current cursor
* Ctrl + Shift + L - Looks for all occurrences of the selection in a file
* Ctrl + F - Searches for text within a file
* Ctrl + Shift + F - Searches for text within the current project
* Ctrl + / - Toggles line comment
* Shift + Alt + A - Toggles a block comment*

#### Consistent Code Formatting:
* EditorConfig
* Prettier
* .prettierignore - In root directory contains normalize.css and reset.css
  vendor/normalize.css

