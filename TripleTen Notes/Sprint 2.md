## Intro to Git and the Command Line:
#### Intro:
<span class="blue-text-bold">shell</span> - A program that interprets command line inputs and translates them into instructions for the OS
<span class="blue-text-bold">pwd</span> - Print Working Directory  <span class="green-text">Displays current path</span>
<span class="blue-text-bold">ls</span> - List  <span class="green-text">Displays a list of the files and folders inside current directory</span>
<span class="blue-text-bold">cd</span> - Change Directory  <span class="green-text">cd Folder_Name</span>
<span class="blue-text-bold">#</span> - <span class="green-text">Bash comments</span>
<span class="red-text-bold">Avoid spaces in file names because it will be harder to work with git bash</span>
* cd "My Videos"
* cd My\ Videos*

#### Returning to the parent directory:
* cd ..  <span class="green-text">Immediate parent</span>
* cd ../Some_Folder
* cd ../../..  <span class="green-text">Go up 3 levels</span>
* cd ~  <span class="green-text">Goes to user directory</span>

#### Returning to previous directory:
* cd  <span class="green-text">Goes to home directory</span>
* cd -  <span class="green-text">Goes to last directory you visited</span>

#### Creating files and folders:
* mkdir projects  <span class="green-text">Makes a project directory</span>
* touch index.html <span class="green-text">Makes an html file</span>
* mkdir projects/sample-project  <span class="green-text">Can make files anywhere</span>
* mkdir -p projects/sample-project/new-folder  <span class="green-text">Creates files up to new-folder</span>

#### Deleting files and folders:
* rm about.html  <span class="green-text">Deletes about if it exists</span>
* rmdir images  <span class="green-text">Deletes images directory if it's empty</span>
* rm -r images  <span class="green-text">Permanently deletes all files and the containing directory</span>

#### Command Line Tips and Tricks:
* Up and down arrow keys - Navigates command line history
* Tab - For autocompletion of file paths, tab again to show a list.
* Ctrl + Shift + c - Copy
* Ctrl + Shift + v - Paste
* code File_Name - Creates a file and opens it in vs code

## Version Control with Git and Github:
#### Configuring Git:
* git config --global user.name "Your Name"
* git config --global user.email "your_email@domain.com"
* git config --global init.defaultBranch main <span class="green-text">Sets default branch</span>
* git config --list  <span class="green-text">Shows settings</span>

#### Initializing Git and Staging Changes:
* git init  <span class="green-text">Initializes a new repository</span>
* git status  <span class="green-text">Gets status of any stashed changes</span>
* git add index.html  <span class="green-text">Stages index.html to be ready to be committed</span>
* git add --all  <span class="green-text">Saves all files</span>
* git add -A   <span class="green-text">Saves all files</span>
* git add .   <span class="green-text">Does not stage deleted files only modified files</span>

#### Making Commits:
* git commit -m "Enter First Commit"
* git log  <span class="green-text">Logs all commits</span>

#### SSH:
<span class="blue-text-bold">SSH</span> - Secure Shell transport layer protocol
* A secure protocol like https
* Has a key pair (public & private)
* Used for authenticating actions specifically for remote desktops and via the command line

#### SSH Commands:
* ls -al ~/.ssh  <span class="green-text">Checks and lists all ssh keys</span>
* ssh -keygen -t ed25519 -C "your_email@domain.com"  <span class="green-text">Generates ssh keys</span>
* ps -a | grep ssh-agent | grep -v grep  <span class="green-text">Checks for running ssh agent</span>
* eval $(ssh-agent -s)  <span class="green-text">Starts an ssh agent (need to run again after rebooting)</span>
* ssh-add ~/.ssh/id_ed25519  <span class="green-text">Binds private key to agent (need to run when reopening terminal)</span>
* clip < ~/.ssh/id_ed25519.pub  <span class="green-text">Copies public key to clip board</span>
* cat ~/.ssh/id_ed25519.pub  <span class="green-text">Prints public key to terminal</span>
* ssh -T git@github.com  <span class="green-text">Tests the connection</span>

#### Linking Local and Remote Repositories:
* https://github.com/your-username/your-repo.git (https) REMOTE_URL
* git@github.com:your-username/your-repo.git (ssh) REMOTE_URL
* git remote add origin REMOTE_URL
* git push -u origin main  <span class="green-text">Pushing to remote repository (-u is used if no set remote yet)</span>
* git remote set-url origin REMOTE_URL  <span class="green-text">Changing remote repo</span>
* git remote remove origin  <span class="green-text">Removes remote repo</span>
* git push
* git pull
* git clone REMOTE_URL

#### .gitignore:
* .DS_Store  <span class="green-text">Ignores all files named .DS_Store</span>
* git rm FILE_NAME  <span class="green-text">Removes from git tracking and deletes from disk</span>
* git rm --cached FILE_NAME <span class="green-text">Removes from git tracking but doesn't delete from disk</span>
* find . --name .DS_Store -print0 | xargs -0 git rm --ignore-unmatch
*<span class="green-text">Finds and deletes all .DS_Store files</span>

## Embedding Content
#### iframe:
```html
<iframe>
	class="shreksophone"
	width="560"  <!-- Better to use percentages -->
	height="315"
	src="https://www.youtube.com/embed/pxw-5qfJ1dk"
	title="Youtube video player"
	frameborder="0"  <!-- Deprecated use CSS border property instead -->
	allow="accelerometer; autoplay; copy-write; encrypted-media; gyroscope; picture-in-picture"
	allowfullscreen
></iframe>
```

#### Responsive iframes:
<span class="blue-text-bold">responsive</span> - displays well on screens of all sizes
```css
.container{
	position: relative;
	overflow: hidden;
	width: 100%;
	padding-top: 56.25%; /* 9:16 aspect ratio */
}
.container__iframe{
	position: absolute;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
}
```

## Animation and Special Effects
#### Transform property:
```css
transform: translate(0px, 0px);
transform: scale(1, 1);
transform: rotate(0 deg);
transform: skew(0 deg, 0 deg);
transform: translate(10px, 20px) rotate(30 deg) /* Multiple transforms */
```

#### Transform Matrix:
* Combines scale, skew, translate
* transform: matrix(1, 2, 3, 4, 5, 6);
* uses radians instead of degrees

#### Smooth Transitions:
```css
div {
	background-color: black;
	transition-property: background-color;
	transition-duration: 1s;
	transition-timing-function: linear;
	transition-delay: 2s;
	transition: background-color 1s linear 2s;
}
```

###### Transition-timing-functions:
* ease
* ease-in
* ease-out
* ease-in-out
* linear

<span class="red-text-bold">Applying transitions to the hover state will only affect when you hover over, not when you take off. Applying transitions to the base class will affect it both ways</span>

#### Shadows:
* Horizontal shadow offset
* Vertical shadow offset
* Blur radius
* Shadow color
```css
box-shadow: 4px 0 5px rgba(0, 0, 0, 0.5);
text-shadow: 5px 6px 7px red;
```

#### Linear Gradients:
* Set with background-image property
* to bottom, to top, to left, to right, to bottom left, to top right
```css
background-image: linear-gradient(to bottom, black, white);
background-image: linear-gradient(#6be454, #56d9ba); /* To bottom is default */
background-image: linear-gradient(90 deg, #6be454, #56d9ba); /* Using degrees */
background-image: linear-gradient(#0078FF 0px 100px, #FF5A0A 100px 200px);
/* Hard stops */
background-image: linear-gradient(#0B2337, #126DDC, #76C2E0, #D1DC9D, #F09174); /* Multiple colors */
```

#### Radial gradients:
```css
background-image: radial-gradient(at 40px 50px, #0078FF, #C2E3E3);
/* Can create hard stops */
```

#### Hiding Overflow:
* When a nested element has a bigger width/height than the parent
* visible  *<span class="green-text">default</span>
* auto  *<span class="green-text">hides overflow and adds a scroll bar</span>
* scroll  *<span class="green-text">always shows a scroll bar</span>
* hidden  *<span class="green-text">hides overflow</span>
* overflow-x & overflow-y  *<span class="green-text">If one has a value than the other will switch to auto</span>

#### Keyframes:
```css
.block_size_med:hover {
	animation-name: arrogant-animation;
	animation-duration 2s;
	animation: arrogant-animation 2s;  /* Shorthand */
}
@keyframes arrogant-animation {
	/* Can also use from{} and to{} for beginning and end */
	0% {
		margin-left: 0;
	}
	50% {
		margin-left: 200px;
	}
	100% {
		margin-left: 0;
	}
	
}
```

#### Animations:
* animation-delay
* animation-iteration-count  *<span class="green-text">infinite for a loop</span>
* animation-direction  *<span class="green-text">normal, reverse, alternate, alternate-reverse</span>
* animation-timing-function  *<span class="green-text">linear, ease-in, ease-out, ease, ease-in-out</span>
* animation-play-state  *<span class="green-text">running paused controlled via javascript</span>
* animation-fill-mode  *<span class="green-text">forward, backward both how the last frame is preserved</span>
```css
animation: move 2s ease-in-out 1s 3 reverse forward running;
```
<span class="red-text-bold">animation-duration placed before animation-delay</span>

## Forms
#### What is a form:
```html
<form>
	<fieldset class="form__fieldset">
		<!-- form elements -->
	</fieldset>
</form>
```
<span class="blue-text-bold">fieldsets</span> - group all text inputs or all button input together. Helps with semantics.

#### Input Fields:
* Does not require a closing tag
* \<input type="text" />
* number
* email
* password
* tel
* url
* range  *<span class="green-text">Creates a slider</span>
* file
* date  *<span class="green-text">Date without time</span>
* datetime-local  *<span class="green-text">Date with time</span>
* month
* week
* time
* \<input type="submit" disabled />  *<span class="green-text">input submit for older browsers</span>

#### Maximum and Minimum Values:
```html
<input type="range" min="-20" max="500"/>  <!-- Defaults from 0 to 100 -->
<input type="number" min="20" max="50" step="10" /> <!-- Steps by 10 -->
```

#### Submit and Reset Buttons:
```html
<button type="submit">Submit</button>       <!-- Submits form -->
<button type="reset">Reset</button>         <!-- Resets form -->
<button type="button">Do Something</button> <!-- No default behavior -->
```

#### Input Fields with Different Syntax:
```html
<textarea></textarea> <!-- For larger texts like a paragraph -->
<select>
	<option>Item 1</option>
	<option>Item 2</option>
</select>
```

#### Labels:
```html
<label for="first-name">First Name</label> <!-- Links label and input -->
<input type="text" id="first-name"/><!-- Clicking on label selects input -->
<label>Name <!-- Works but prevents linking this label to another field -->
	<input type="text"/>
</label>
```

#### Submitted Values:
* server_address/form.html?firstname=Mike&authorized=yes&search=google
* ? - Separates server address with string query
* & - Separates name/value pairs
```html
<input type="text" name="firstname" value="No name entered">
<select name="search">
	<option value="google">Google</option>
</select>
```

#### Checkboxes and Radio Buttons:
* Checkboxes - allow users to check one or more options
* Radiobuttons - allow users to select only one option
```html
<input type="checkbox" />
<input type="radio" />
```
* checkbox needs to all have the same name attributes and labels need to match the id
* radio also needs to also all have the same name attribute
```html
<input type="radio" name="choice" checked /> <!-- Initialize with checked -->
```

#### Placeholders:
* doesn't take place of the value and does not need to be manually deleted
```html
<input type="text" name="firstname" placeholder="John Doe" />
```

#### Required Fields:
```html
<input type="text" required />
```

#### Styling Text Input Fields:
* Input fields and labels are inline elements. Need to change display to control positioning
* Input fields don't inherit fonts. Need to manually add font-family: inherit;
```css
input:focus {  /* When input is selected */
	outline-color: yellow;
	outline-style: dashed;
	outline-width: 3px;
}
input[type="submit"] {  /* You can also use BEM methodology */
	/* Add styles here */
}
```

#### Styling Non-Standard Input Fields:
* <span class="blue-text-bold">Non-standard input field</span> - Any field that doesn't take text. (Checkbox, Radio, Range)
* Hide the input field
```html
<label> <!-- Still need this to click on it -->
	<input type="checkbox" class="invisible-checkbox" />
	<span class="visible-checkbox"></span>
</label>
```
```css
input[type="checkbox"] {  /* Hiding checkbox */
	appearance: none;
}
input[type="checkbox"]+span {  /* Gets the span element right after checkbox */
	/* Add styles here */
}
```
###### CheckBox Pseudo Classes: 
- disabled
- checked
- focus

#### Styling Placeholder:
```css
input::-webkit-input-placeholder { }  /* Safari and Chrome */
input::-moz-placeholder { }  /* Firefox */
input::-ms-input-placeholder { } /* Internet Explorer */
input::placeholder{ }
```
<span class="green-text">There are a lot of places where you need vendor prefixes. Placeholders is just one example </span>

## File Organization with BEM:
#### CSS At-Rules @import:
- Change the behavior or appearance of elements on a webpage
- @identifier_name rule;
```css
@import url("main.css"); /* includes external files like fonts or extra css code */
/* only works at top of file */
```

#### File Structure:
![[4. BEM File structure|400]]

#### Global Styles:
* Recommended to declare font family and font sizes for each BEM block
* You could also create a shared style but not recommended with BEM
* Place into vendors directory then connect with @import


## README.md: 
* \# Heading 1  <span class="green-text">First level heading</span>
* \## Heading 2  <span class="green-text">Second level heading</span>
* \*Welcome*  <span class="green-text">Italics</span>
* \_\_Hello__  <span class="green-text">Bold</span>
* \~\~Nevermind~~  <span class="green-text">Crossed out</span>
* 1. One  <span class="green-text">Ordered list</span>
* 2. Two
* 3. Three
* - Milk  <span class="green-text">Unordered list</span>
* - Eggs
* - Toast
* \`\`\`html/css/javascript  <span class="green-text">Code snippet</span>
  alert('Hey!')
  \`\`\`
* \[Practicum] (https://www.practicum.com) <span class="green-text">Link</span>
* \!\[alt text] (relative_path)  <span class="green-text">Picture</span>

