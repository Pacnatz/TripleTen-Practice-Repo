## Flexbox:
#### Flexbox:
Flexbox - Organize elements inside containers and alter their spacing and order
```css
.container{
	display: flex;
	flex-direction: column; /* row, column-reverse, row-reverse */
	justify-content: flex-start; /* flex-end space-around space-between */
	flex-wrap: wrap;
}
```
Child elements do not inherit flex containers

![[2. Flexbox Diagram]]

```css
display: inline-block;  /*Puts elements in a row with a small gap in between*/
display: flex;  /* No gap in between */
```
:nth of type() - pseudo class to differentiate odd / even

#### Justify Content:
* flex-start
* flex-end
* center
* space-around - equal spacing equal space on both sides space between item is 2x
* space-between - pushes elements as far away from each other as possible
* space evenly - equal spacing even on the gaps of flex-start / flex-end

#### Align Items:
* flex-start
* flex -end
* center
* baseline - draws a line and flex items sit on that line
* stretch (default)
align-self: works exactly like align-items but works for individual flex items

#### Order: 
* Works similar to z index but orders flex items from left to right & up to down
```css
order: 1;  /* will go last */
order: 0;  /* default, ordered top down in order of flex container */
order: -1;  /* will go first */
```

#### Flex-wrap:
* Allows flex items to go to a newline to make space
* nowrap - (default) all items will be on one line
* wrap - will wrap onto multiple lines (top to bottom)
* wrap-reverse - will wrap the newline on top

#### Flex-flow:
* Accepts both flex-direction and flex-wrap
```css
flex-flow: row wrap;
flex-flow: column-reverse wrap-reverse;
```

#### Align-items vs align-context:
* Align-items need flex: nowrap
* Affects each individual item inside a single flex line
* Align-content needs flex wrap enabled
* Affects the entire group of lines

#### Flex-grow:
* Allows flex items to grow and distribute along main axis of flex container
* 0 - (default) flex items keep their original size and do not grow
* \> 0 - flex items grow to fit container and are split into ratios (fr)

#### Flex-shrink:
* Opposite of flex-grow
* 1 - (default)
* \>1 - more shrinkage
* <1 - less shrinkage

#### Flex-basis:
```css
flex-basis: 300px;
```
Will keep the flex item at 300 pixels as long as there's space.
(Based on flex-direction) (Overrides width)
(Sets initial size before flex-grow / flex-shrink)

```css
row-gap: 20px;  /* Works like margins without the extra white-space */
column-gap: 20px;
```

![[3. Margin Collapsing | 200]]
Margin-bottom will collapse and element 2's margin-top: 40px; will take affect
<span class="blue-text-bold">Margin collapsing does not take place in a flex box. The margins will be added together</span>

## Meta Tags, Semantics, and Basic Interactivity
#### Semantic tags:
* header
* nav
* main
* section
* article
* footer
```html
<html lang="en"> Suggests translating a page if not in the user's native language.
```
Lang can be applied to any element

#### Meta tag structure:
* element name - (meta) tells the browser what the tag means
* name attribute - defines the meta tag type
* content attribute - contains an instruction, a value associated with the name attribute
```html
<meta name="author" content="Douglass Adams"/>
```


#### Viewport meta tag:
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
```
* device-width - sets the width of the page to match the screen width
* initial-scale=1.0 - sets the initial zoom level

#### Website description for search engines:
* description - Short paragraph that details the purpose of your webpage
* author - Only one author can be specified
* keywords - Makes it easier to find this webpage if they enter these keywords

#### Semantic Tags:
* Works just like \<div> but adds semantics
```html
<header>
<nav>
<main>
<section>
<article>
<footer>
```

#### Headings:
* h1 to h6
* h1 should be used for the most significant title on the page
* h2 for major sections h3 for subsections
* never skip heading levels
###### Extra info:
```css
list-style-type: disk;  /* Styling ol and ul's bullet points applied to parent */
```
```html
<ul role="list">  // For safari accessibility concerns
```

#### Navigation Example:
```html
<nav class="menu">
	<ul>
		<li><a href="#">Home</a></li>
	</ul>
</nav>
```
\<br> - Used for line breaks

#### Articles:
```html
<article>
	<h2>Article Title</h2>
	<p>The main body of the article</p>
</article>
```

#### Quotations:
```html
<blockquote cite="https://awesome-quotes.com">
	When the going gets tough, the tough get going
</blockquote>
```

#### Figures:
```html
<figure>
	<blockquote>Blah</blockquote>
	<figcaption>Nhat</figcaption>
</figure>
```

#### Not recommended unless use case is clear:
* \<em>
* \<i>
* \<strong>
* \<b>

#### Identifiers:
* Unique attribute to represent elements in CSS
```html
<h1 id="lesson-13">Lesson 13</h1>
<a href="#lesson-13">Link to somewhere on the same site</a>
<a href="https://my-site.com/article#lesson-13">Link going to a different site</a>
```
```css
#lesson-13{
	color: red;
}
text-decoration: none; /* Removes underline */
```
* Don't start with numbers / special characters
* Separate words with hyphens or underscores
* Use simple English words
<span class="blue-text-bold">You can have an element with classes and ids</span>

#### Link states:
```css
a:link {
	color: #00f;
}
a:visited {
	color: #808080;
}
a:hover {
	color: #0f0;
}
a:active {
	color: #f00;
}
a:not(:visited) { }
```

#### Pseudo Class:
```css
.card:nth-of-type(3n) {
	margin: 0;
}
.card:nth-of-type(even) { }
.card:nth-of-type(odd) { }
.card:first-of-type { }
.card:last-of-type { }
```

#### Pseudo-Elements:
* Written with an additional colon (specifies parts of an element)
```css
p::first-line { }
p::first-letter { }
.selector::before { } /* Modifies stuff immediately before the element */
.selector::after { content: "After"; }  /* content property required */
```

#### Attribute selectors:
```css
img[src="logo.png"] {  /* Selects only images with that address */
	border: 1px solid #0000ff;
}
```
User-agent stylesheet - Fixed styles that browsers use. (Heading elements)
* Rewrite all the main styles
* reset.css (deletes all user-agent styles) (link first)
* normalize.css (makes all user-agent styles the same) (link first)*

## The DevTools:
* Right click → Inspect
* Top half is html & bottom half is CSS
Selecting an element does:
* The element itself is outlined on the webpage
* Corresponding tag is highlighted in the elements tab
* The CSS styles associated with the elements are shown in the styles tab
<span class="blue-text-bold">Devtools can be useful for testing styles without reopening code editor</span>

## Setting Up a Local Development Environment:
Development env - combination of tools and settings that allow us to test and debug code
File System - Hierarchy of nested folders or directories
* Root directory - C drive
* User directory - User folder in C drive
VS Code is primarily a plain text editor
! + tab - Generates basic HTML boilerplate
##### Opening HTML pages in a browser:
* Open HTML file manually and reloading
* Use Live server VSCode extension (https://localhost:port_number)
#### Validating HTML and CSS:
* Html - W3C Markup Validation Service
* CSS - W3C CSS Validation Service

## BEM Methodology:
#### BEM (Block Element Modifier):
* Naming conventions
* Structuring files
<span class="red-text-bold">Block</span> - Logically and functionally independent page component. Headers, Logos, Search Bars.
<span class="red-text-bold">Element</span> - Essential part of a block that has no application outside the block. (Search button)
<span class="red-text-bold">Modifier</span> - A property applied to a block or element

#### BEM Blocks:
* Blocks have the same name as their classes
* Blocks can be nested inside one another
* BEM styles are applied to class names only. No tag names, ID, or nesting
###### Naming BEM Blocks:
* Block names are set using the class attribute. \<header class="header">\</header>
* A block's name should describe the purpose of the element not their appearance
* Block names can be multiple words separated by a hyphen
* Block names should be unique (only assign buttons to buttons)
* Block elements should not affect external geometry (use padding rather than margin)
```css
.nav {
	width: 100%;
	height: 90px;
	padding: 20px;
}
```

#### BEM Elements:
* Blocks can contain 0 or more elements.
* block-name__element-name
* Should not be used outside its own block
* Should describe the purpose of the element rather than its appearance
* Only use class names to apply styles.
* Can be nested but still has the same naming convention

#### BEM Modifiers:
* Used to denote the state, behavior or appearance of a block or element
* block__element_modifier
###### Boolean Modifiers:
* menu__link (false)
* menu__link_active (true)
###### Key-Value modifier:
* solution-status_type_success
* block__element_key_value
<span class="blue-text-bold">Never have modifiers alone in a class list</span>

###### Modifier keys:
* <span class="red-text">type</span> - used to describe purpose or functionality (button_type_submit)
* <span class="red-text">theme</span> - describes appearance (card_theme_dark)
* <span class="red-text">size</span> - describes different size (icon_size_s)
* <span class="red-text">content</span> - describes the type of content in container (footer__column_content_hours)

#### Using Modifiers and Mixes with Blocks
* <span class="red-text">modifier</span> - classes that are applied to specific blocks or elements to change their state, behavior, appearance
* <span class="red-text">mix</span> - class to apply styles to multiple blocks or elements

###### Creating mixes:
* Combining, blocks, main-text mixer for section and footer (combining general rules across different blocks & elements)
* Mixing elements with blocks (for positioning)
```html
<div class="footer">
	<div class="social-icons footer__social-icons"></div>
</div>
```
social-icons is now both an independent block and an element of the footer block

#### CSS Ordering Guidelines: 
1. Write the styles for the block
2. Next, write any pseudo-classes, pseudo-elements, and modifiers of this block
3. Then, write the styles for the first element of that block
4. 4. Follow this with any pseudo-classes, pseudo-elements, and modifiers of that element
5. Repeat (3) and (4) for each element of the block
6. Then, repeat (1) through (5) for each block on the webpage

#### BEM and other types of CSS selectors:
* Attribute selectors
```css
.form__input[type="email"] { }
.form__input_type_email { }  /* BEM equivalent */
```
* Grouping selectors (selector list)
```css
h1, h2, h3 { }  /* Not BEM approved */
```
* Descendant combinator
```css
.profile p { } /* Not BEM approved */
```
* Adjacent sibling combinator
```css
img + p { } /* Not BEM approved */
```

#### CSS Specificity and BEM:
<span class="red-text-bold">specificity</span> - refers to the rules that dictate which style will be applied to an element when multiple conflicting styles are present.
###### The specificity score:
1. Inline Styles \<style>\</style>                                                    1-0-0-0
2. ID Selectors  #                                                                              1-0-0
3. Class selectors, Attribute Selectors, and Pseudo-Classes  .  []  ::  0-1-0
4. Element selectors, Pseudo Elements  div  h1  :                            0-0-1
Specificity is read left to right if the same, it's applied top down

## Positioning Elements
#### Normal Flow: 
* If an element is part of the flow, then all other elements in the flow knows where it is and reacts to it
* Elements in the flow can't really be repositioned. But they can appear as if they have been with margin & padding
* Increasing margin and padding only grows the elements appearing like it shifted
* Applying negative margin can potentially cause the elements to overlap.

#### Static Position:
* Default position value
* Elements are part of normal flow

#### Relative Positioning:
* position: relative;
* offsets position with respect to their place in the document flow.
* offset using top, left, bottom, right
* if you set top and bottom at the same time, bottom will be ignored
* same with left and right

#### Fixed Position:
* position: fixed;
* completely pulls elements out of the flow
* sits on top of normal flow
* top: 10px; right: 40px;  // Elements sits at top right of browser window
* top-left corner priority

#### Absolute Positioning:
* position: absolute
* does not stay on the screen as you scroll
* placed relative to their parent elements, as opposed to the viewport
* if there's no immediate parent with relative positioning, it will act like fixed
* if parent element is position: static, then it will go up until it find a parent with position: relative
  else it will act as fix (Positions relative to body)

#### Z index:
* Determines the order in which elements will be layered
* Negative to positive whole numbers
* The greater z-index will be shown on top
* Default is auto. Similar but not exactly like z-index: 0
* Can't be applied to elements with position: static or position: none

#### Stacking Context:
* A box in an HTML document in which stacking occurs
* All stackable elements within a stacking context will be stacked according to their z-index but they won't be stacked with elements that are outside of their stacking context
* Largest stacking context is the root element (\<html>)
* Elements with position that isn't static, and z-index that isn't auto
* Flex items with z-index that isn't auto
* z-index: auto does not create stacking context (z-index: 0 does)

## File Structure and File Paths
<span class="red-text-bold">extension</span> - file extension such as .html .css
<span class="red-text-bold">stem</span> - file name
* Data files: primarily store information .txt .jpg .html .css
* Executable files: These are files that can run directly as software applications or task
###### Directory:
* Nested structure
* Hierarchy
* Project organization

<span class="red-text-bold">root directory</span> - Top most directory
<span class="red-text-bold">subdirectory</span> - Nested directories

###### Common subdirectories:
* Images
* Vendor ( third party files such as normalize.css and fonts )
* Styles
* Scripts

If your webpage contains images from another domain, it may not load efficiently
On windows and mac files and directories and case insensitive but URLs are case sensitive.
(Make sure all files are case-sensitive)
./ - Refers to the current directory
../ - Refers to a directory one level higher
<span class="blue-text-bold">Only use relative paths with live server</span>
<span class="red-text-bold">Content Delivery Network (CDN)</span> - A link to an external website that can contain CSS or JavaScript that you can link to your html