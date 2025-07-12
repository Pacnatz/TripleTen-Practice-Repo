## Grid Layout Part 1:
#### Introduction:
* Grids allow layouts in X and Y axis simultaneously
* display: grid;  <span class="green-text">grid containers and grid items</span>
* each added grid item creates one more row

#### CSS Grid Layout Terminology:
* display: inline-grid;  <span class="green-text">width equals total width of its child elements</span>
  <span class="green-text">only takes up the space needed for the content inside them</span>
* <span class="blue-text-bold">grid lines</span> - invisible horizontal and vertical lines that intersects to from columns and rows
* <span class="blue-text-bold">grid track</span> - space between any 2 lines on a grid
* <span class="blue-text-bold">grid cell</span> - space between intersecting rows and columns
![[5. Grid Diagram|300]]

#### Grid Container Rows and Columns:
```css
grid-template-columns: 25% 50% 25%;/* Seperates to 3 columns left right top down */
grid-template-rows: 150px 150px 100px; /*If 7 items the bottom right 2 are empty*/
/* setting these values to auto will make room for the widest item */
```

#### Grid gap property:
```css
gap: 10px;
column-gap: 20px;
row-gap: 10px;
grid-gap: 10px 20px; /* row column */
/* gap is not supported in older browsers */
```

#### Repeat Function:
```css
grid-template-columns: 20% 20% 20% 20% 20%;
grid-template-columns: repeat(5, 20%);
grid-template-columns: 100px repeat(3, auto) 200px; /* Creates 5 columns */
```

#### Fr Unit:
- Fractional unit
```css
.container{
	width: 600px;
	grid-template-columns: repeat(6, 1fr); 
	/* 6 even columns */
	grid-template-columns: 100px 2fr 3fr; 
	/* 2nd would be 200px 3rd would be 300 */
}
```

#### Positioning Items in a Grid Container:
```css
.block_size_big{
	grid-column-start: 1; /* Block starts at column1 ends at column3 */
	grid-column-end: 3;
	grid-row-start: 1; /* Makes a large square */
	grid-row-end: 6;
}
.block{
	grid-row: 1;  /* Starts at 1 ends at auto (takes up 1 cell) */
	grid-column: 2/4; /* Starts at 2 ends at 4 */
	grid-template-rows: [aside-start] 100px [aside-end];
	/* creates 2 variables aside-start starts at 1 aside-end is 2 */
	grid-column-end: span 2; /* Takes up 2 columns before it ends */ 
}
```
- Grids can have negative numbers
![[6. Grid Diagram Reverse|300]]

#### Grid Areas:
* <span class="blue-text-bold">Grid area</span> - a rectangular area consisting of one or more grid cells
```css
.container {
	grid-template-columns: repeat(3, 1fr);
	grid-template-rows: repeat(4, 1fr);
	grid-template-areas:
		"header header header"
		"news news aside"
		"promo promo aside"
		". footer footer"
}
.header {
	grid-area: header;  /* Will take up whole top row */
}
```
<span class="red-text-bold">Every grid cell must have a name (Use . for an empty cell)</span>
<span class="red-text-bold">Area names cannot create L Shapes only rectangular</span>
<span class="red-text-bold">Only one name per area</span>

## Grid Layout Part 2:
#### Size of Implicit Rows and Columns:
- <span class="blue-text-bold">Implicit tracks</span> - Rows and columns that are added automatically not from grid-templates
- Size for implicit tracks are auto by default
```css
grid-auto-rows: 100px 300px; 
/* Sets height of implicit cells to 100px and alternates with 300px */
```

#### Grid-auto-flow property:
```css
grid-auto-flow: column; /*Will create new cells in the next column instead of row*/
grid-auto-flow: dense; /* Will pack new cells efficiently into the grid */
```

#### Minmax() and fit-content() functions:
```css
grid-template-columns: 1fr 1fr minmax(300px, 1fr);
/* third column can't be smaller than 300px or larger than 1fr */
grid-template-columns: 1fr 1fr max-content; /* No text wrapping */
grid-template-columns: 1fr 1fr min-content; /* Text wrapping */
grid-template-columns: 1fr 1fr minmax(min-content, max-content); 
/* Won't be compressed to a size smaller than the content
   Won't take up too much space when there's a lot of content */
grid-template-columns: 1fr 1fr fit-content(200px);
/* If arg is <= max-content value is minmax(auto, arg) 
   If arg is >= max-content value is minmax(arg, auto)*/
```

#### Auto-fill and auto-fit properties:
* Used for when we want to show all items on a smaller screen
```css
grid-template-columns: repeat(auto-fill, 100px);
/* Creates as many columns possible keeps empty tracks 
   Keeps the size of the grid item */
grid-template-columns: repeat(auto-fit, 100px);
/* Creates only visible columns collapses empty tracks */
```

#### Aligning Grid Tracks and Grid Items:
- <span class="blue-text-bold">Block axis</span> - Corresponds to the columns <span class="green-text">Up and Down</span> 
- <span class="blue-text-bold">Inline axis</span> - Corresponds to the rows <span class="green-text">Left to Right</span>
<span class="red-text-bold">For grid items:</span>
- Align a grid item along block axis use align-self
- Justify a grid item along inline axis use justify-self
<span class="red-text-bold">For grid container:</span>
- Align a grid item along block axis use align-items
- Justify a grid item along inline axis use justify-items
- Align grid tracks along block axis use align-content
- Justify grid tracks along the inline axis use justify-content

## Developing an Interface for Different Devices
#### Relative Block Dimensions:
* min-width to set the smallest page width
* max-width & margin: auto stops the content area from stretching after a certain point
* You should give a minimum height to text elements. 
     <span class=red-text-bold>Ask yourself if you should use min-height instead of height</span>
* Max-height to keep it from being too big
###### Percentages:
* Best practice is to use percentages when working with flex boxes
* Or when the height of the parent element has been explicitly declared
###### Viewport Height and Width:
* vw and vh include the entire window. Including scroll bars and tab windows
* Better to go with percentages if you want something to take up the whole screen
###### Viewport Minimum and Maximum:
- <span class="blue-text-bold">vmin</span> - On desktop will refer to height on mobile refers to width
- <span class=blue-text-bold>vmax</span> - On mobile refers to height, on desktop refers to width

#### Calculating Values with the calc() Function:
* <span class="blue-text-bold">calc()</span> - Calculates dimensions like width, height, and margins
```css
selector{
	width: calc(100% / 3);
	height: calc((100vh + 300px)/2);
	/* + and - operators surrounded by white space but * and / do     not need it*/
}
```

#### Percentages for Margins and Padding:
```css
.parent{
	width: 30%;
}
.child{
	width: 100%;
	padding-bottom: 100%;
	/* Will take up a vertical space that will scale with the            WIDTH of its parent container */
}
```

#### Relative Dimensions for Text Elements
- <span class=blue-text-bold>em unit</span> - Defined relative to the size of the parent element's font
- <span class="blue-text-bold">rem unit</span> - Root em.  Equal to the font size of the document's root object
```css
html{
	font-size: 30px;
}
.parent {
	font-size: 40px;
}
.child {
	font-size: 2rem; /* Font size is 60px */
	line-height: 1; /* Most devs use 1.2 or 1.5 doesn't have to be pixels */
}
```

#### The Ins and Outs of Using Bitmap Images:
- <span class=blue-text-bold>Bitmap</span> - stored as pixels
- <span class="blue-text-bold">Vector</span> - Points, splines, bezier curves, circles, and polygons

#### The Ins and Outs of Using Vector Images:
###### Ways to place .svg files on your webpage:
1. Using the src attribute in the \<img> element
2. Using the background-image property in CSS
3. As an iframe
4. Using src attribute in the \<embed> element
5. using the data attribute in the \<object> element
6. By embedding the .svg file code in the HTML.

#### Optimizing Fonts for Devices with Different Resolutions: 
- .woff best font format for webpages
```css
-webkit-font-smoothing: antialiased; /* For chrome */
-moz-osx-font-smooothing: grayscale: /* For mozilla */
-ms-text-size-adjust: 100%; /* For internet explorer */
```
###### Rendering:
- Space between letters and characters (for fonts it overlaps the letters)
```css
text-rendering: optimizeLegibility;
/* Applies kerning and ligatures */
```

#### Meta Tags for Smooth Scaling:
###### Content Attribute:
- width
- height
- initial-scale <span class="green-text">the scale at launch</span>
- minimum-scale <span class="green-text">the lower boundary for scaling</span>
- maximum-scale <span class="green-text">the upper boundary for scaling</span>
- user-scalable <span class="green-text">the ability for users to scale on their own</span>

###### Width and Height:
```html
<meta name="viewport" content="width=device-width, height=device-height, initial-scale=1">
```

###### Scale Settings:
```html
<meta name="viewport" content="initial-scale=1, maximum-scale=1 minimum-scale=.5 user-scalable=yes">
```

#### Media Queries:
On mobile:
- Since there's no mouse, there's no :hover effect
- Instead of clicking, users tap the screen
- It can be hard to read fine print
- Tapping on small elements may be difficult
- It's fine for the design to look completely different, because a great desktop interface might be terrible for phones

<span class="blue-text-bold">all</span> - is for all device types
<span class="blue-text-bold">screen</span> -is for all screen devices
```css
/* Generally placed at the end of the CSS Code */
@media screen { 
	body { 
	font-family: "Arial"; 
	} 
}
/* min-width for mobile first approach
   max-width for desktop first approach */
@media screen and (min-width: 720px) {
	body { 
		background-color: green; 
	}
}
```

#### Approaches to Building Media Queries
###### Categorizing devices by screen width:
- Small smartphones (max-width: 320px)
- Medium smartphones (max-width: 375px)
- Large smartphones (max-width: 425px)
- Tablets (max-width: 768px)
- Small desktop computers and laptops
- Mid-sized desktop computers and laptops
- Large Monitors (min-width: 1440px)
- Huge Monitors (min-width: 2560px)
###### Styling via orientation:
```css
@media screen and (max-width: 568px) and (max-height: 320px) { /* styles for iPhone 5 */ }
```
## Debugging Responsive Webpages
#### How to avoid horizontal scroll:
<span class="red-text-bold">If you see a horizontal scrollbar somethings wrong</span>
###### How to detect horizontal scrolling:
1. Open DevTools and the Device Toolbar
2. Select "Add device type"
3. Choose "Desktop"
###### How to determine what's causing horizontal scrolling:
1. Go to the "Elements" panel in DevTools
2. Delete an element from the page
3. Check for horizontal scroll. If it is still present delete another element and repeat
4. Eventually the horizontal scroll should disappear. When that happens the most recently deleted element should be the cause of the scrolling
###### How to fix horizontal scroll:
- Explicitly set widths (in pixels) are a frequent cause of horizontal scroll. Check your element's width and min-width properties carefully
- Also, pay close attention to horizontal padding, increasing the amount of horizontal content.
- Using width: 100vw is a common source of horizontal scroll because it doesn't account for the width of the vertical scrollbar. Try using 100% instead.
- Other styles that may affect the amount of horizontal content on a page include margin, grid-template-columns, and gap

## Intermediate Bash:
#### Working Quickly on the Command Line:
###### Using shortcuts to correct mistakes:
1. Go to the beginning of the row with <span class="blue-text-bold">Ctrl+A</span>
2. Correct the mistake
3. Return to the end of the row using <span class="blue-text-bold">Ctrl + E</span>
4. Finish entering the command
###### Accessing the command history with reverse-i-search:
- <span class="blue-text-bold">Ctrl + R</span> - starts reverse-i-search. You can start searching for the command you're looking for

#### Copying and Moving Files:
###### Running multiple commands with the && operator:
```bash
mkdir new-project && cd new-project && touch index.html
# runs left to right, does not execute if there's a typo
cp WHAT_TO_COPY_1 WHAT_TO_COPY_2 WHERE_TO_COPY_TO
# You don't need to copy into the directory
cp index.html about.html
# This will copy index into the same directory as about
mv WHAT_TO_MOVE_1 WHAT_TO_MOVE_2 WHERE_TO_MOVE_TO
mv indx.html index.html
# mv can be used to rename files. This will rename indx to index
```

#### Viewing and Editing Files with the Command Line:
###### Viewing file contents with the cat command:
- <span class="blue-text-bold">cat</span> - allows us to view file contents in the terminal
```bash
cat index.html
cat ~/Documents/file.txt
# cat won't open image files
cat -n index.html
# -n flag adds line numbers to the display
```
###### Nano: a command line text editor:
```bash
nano hello.txt
# Save changes by pressing Ctrl + O then enter to exit
# Ctrl + X if there are unsaved changes
```
There's also another editor called Vim (Learn enough to be productive)

#### Getting help without leaving terminal:
```bash
git --help
# Pull sup the help menu
```
###### What exactly is a pipe: 
<span class="blue-text-bold">pipe</span> - a connection between 2 processes or programs, where the output of one program is provided to the second program. " | "

## Intermediate Git
#### The commit cycle revisited:
- <span class="blue-text-bold">Modified files</span> -  Added using git add but were changed after
- <span class="blue-text-bold">Untracked Files</span> - New files that have not yet been added with git add
- Git will not see empty folders. Make an empty file in it named .gitkeep
![[7. Git Commit Cycle|450]]
#### The commit log and hashes:
```bash
git log
# You can exit with the Q key
```
###### Commit Hashes:
- <span class="blue-text-bold">commit hash</span> - a string that stores all the information about your commit
- Process of converting any data into a unique value of a certain length

#### Inspecting Changes: git diff:
- running git diff shows you the difference in the files in the staging area
- + indicates something added - indicates something taken away
- git diff compares the files now and between the last recent commit
- git diff --staged shows changes only on staged files
```bash
git log --oneline
# output
# first 7 hash characters and their commends
git diff d9aab83 06c23bf
#compares changes between 2 commit
git --no-pager diff 2a83df9 4ebb9f2
# Disable pager temporarily
git diff -U1000 2a83df9 4ebb9f2
# -U shows ALL lines around changes
git diff --name-status 2a83df9 4ebb9f2
# Shows just what files changed
```
###### How to navigate the : Pager Prompt:
| Key   | Action                                |
| ----- | ------------------------------------- |
| Space | Go to the next page                   |
| Enter | Scroll down one line                  |
| b     | Go back one page                      |
| q     | Quit and exit the viewer              |
| /text | Search fo r"text"                     |
| n     | Go to the next match from your search |

#### If the Commit Goes Wrong:
```bash
git commit --amend -m "add index.html with basic structure"
#Adding changes to your last commit after you've already pushed it
#Only works if your commit hasn't been pushed to a remote repo
git push -f
# Careful of this, you should avoid changing commit history
```
###### Everything's broken:
```bash
git reset HEAD
# unstages files that are currently staged without changing the files' contents. 
git reset --hard HEAD
# Will replace any changed files with those from the most recent commit
git reset commit-hash
# Resets your repo to a previous commit
```

#### Where does git keep its changes:
<span class="blue-text-bold">HEAD</span> - Generally points to the current commit and branch you're working on
```bash
cd .git/ && cat HEAD
# outputs refs/heads/main
cat refs/heads/main
# Outputs our current commits hash
```
## Adding Fonts via @font-face
```css
@font-face {
	src:
	url(https://pictures.s3.yander.net/fonts/Ibmplexserif.woff2)
	format("woff2"),
	url(https://pictures.s3.yander.net/fonts/Ibmplexserif.woff)
	format("woff");
	font-family: "IBM Plex Serif";
	/* naming the custom font */
	font-weight: normal;
}
body {
	font-family: "IBM Plex Serif";
}
```
 <span class="red-text-bold">font-face declarations are placed before CSS styles but after normalize.css</span>
- Place font files in your project's vendor directory
vendor/
	fonts/
		--> contains all necessary font files
	fonts.css
	normalize.css
```css
@font-face { 
	src: 
		url(path/to/font.woff2) format('woff2'),
		url(path/to/font.woff) format('woff'),
		url(path/to/font.ttf) format('truetype'),
		url(path/to/font.eot) format('eot'); 
		/* Multiple formats the browser can choose from
}
```
<span class="red-text-bold">You can see what formats browsers are compatible with on caniuse.com</span>

