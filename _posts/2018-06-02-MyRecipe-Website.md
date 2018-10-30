### Basic HTML Structure

* In the next set of tutorials, i'll be working on three web projects html/css.
* This section i will create the basic html structure.

```html
<!DOCTYPE html>

<html land="en">

	<head>
		<title>MyRecipe</title>
		<meta charset="utf-8">
		

		<link rel="stylesheet" type="text/css" href="style.css">
	</head>

	<body>


	</body>

</html>
```

* To create a comment in html you use the following syntax.

```
<!-- This is a comment -->
```

* Now the ```<!DOCTYPE html>``` declaration defines this document to be HTML5.
* The ```<html lang="en">``` defines the documents primary language as English.
* The ```<head>``` Head contains meta information about the page.
* The ```<title>``` element specifies a title for the document.
* The ```<meta>``` defines the metadata about an HTML document.
* The ```<link>``` tag defines the relationship between a document and a external resource. In this used to link a style sheet resource.
* The ```<body>``` tag defines the documents body.
* The ```<html>``` tag defines the root of an HTML document.

  
---

### HTML5 Header Section

* In this section i will create the content for the header section for the site.

* I made a few minor changes with the code.

```
<!DOCTYPE html>

<!-- Defines the documents primary language as English -->
<html lang="en">

	<!-- Head contains meta information about the page -->
	<head>
		<title>MyRecipe</title>
		<meta charset="utf-8">

		<link rel="stylesheet" type="text/css" href="style.css">

	</head>

	<!-- Body is used for the html content of the page --> 
	<body>
		<header>
	
			<div class="left">
				<ul>
					<li><a href="">Popular Recipes</a></li>
					<li><a href="">Whats New</a></li>
				</ul>

			</div>

			<div class="right">
				<ul>
					<li><a href="">Categories</a></li>
					<li><a href="">Meal Ideas</a></li>
				</ul>
			</div>


			<div id="logo">
				<img src="chefs-hat.png">
				<p>MyRecipes</p>
			</div>

		</header>
		
	
	</body>

</html>
```

* The <header> element is used for introductory content.
* The <div> tag is used to divde different sections within a HTML document.The <div> is oftemn used together with CSS, to layout a webpage.
* I gave each <div> tag a with class so i could identify it with css.
* I then used <ul> for an unordered list.
* The <a> defines a hyperlink where i then used href to specify where the url of the page will go. In this situation i have not yet link it to anywhere.
* I have added a image for the middle section. Which is a just a chef hat.

---

### Styling the Header

* This section i will start adding styling to the header section.
* So i will be working on the css file.

* First thing i'm going to do is change the background color ot white.
* To do this use the following code.

```
html{
	background-color: white;
}
```

* Now we are going to add the background color for the body.

```
body{
	background-color: #ede6e6;
}
```
* This will show a greyish color for the body.

* Now i'm going to have to centralize the body section.
* This can be done with a margin. The margin surrounds the div.

```
body{
	background-color: #ede6e6;
	margin: 0 auto;
}
```

* The margin is set to 0 so the top shows no whitespace and the same with the bottom.
* The auto for the margin is for the left and right and this will automatically set the position for both the right and left.

* Now for the width of the pixels.

```
body{
	background-color: #ede6e6;
	margin: 0 auto;
	width: 960px;
}
```

* Now i'm going to set the font family throughout the site.

```
body{
	background-color: #ede6e6;
	margin: 0 auto;
	width: 960px;
	font-family: arial, helvetica, sans-serif;
}
```

* Now im going to start styling some text. In this case they are hyperlinks surrounded by <a tags.

* First i'm going to remove the underlined hyperlinks.
* With text-decoration.
```
a {
	text-decoration: none;
}
```

* I'm going to also change the font color.
```
a {
	text-decoration: none;
	color: #373535;
}
```

* Now i'm going to style the header tags.
* First i'm going to set the height the of header container.

```
header{
	height: 150px;
}
```

* Now i'm going pad the text, as its bunch to the top of the screen.

```
header{
	height: 150px;
	padding-top: 20px;
}
```

* Now you will notice a gap from the text and the container at the top.

* Here i'm going to alter the unorderd list.
* By default the unordered list has some padding. Here i'm going to reset the padding.

```
ul{
	padding: 0;
}
```

* Here the indivdual lists, i'm going to change the display of them to inline instead of them being stacked on top of each other.

```
li{
	display: inline;
	
}
```

* Now you should the list items inline instead of them being stacked on top each other.

* Here i'm going to space the list items with padding.
* Also the font size of list items.

```
li{
	display: inline;
	padding: 10px;
	font-size: 16px;
}
```

* Here i'm going to create a border to separate the list items from each other.
* To do this, i'm going to have create psudo class to represent the first list item.

```
header li:first-child{
	border-right: 1px solid #373535;
}

```

* The border here is going to 1px wide and the line type will be solid, and the color set to 373535.

* Here i'm going to use float to separate the divs.
* To target a class you use the . to signify that a class will be used.

* First i will start with the left div.

```
.left{
	width: 320px;
	float: left;
}
```

* From the above i set the width to be 320px wide, and the float to the left of the page.

Now i'm going to do the same for the right div.

```
.right{
	width: 320px;
	float: right;
}
```

* Now i'm going to navigation div slightly to the bottom of the contaner.
* So i'm going to add some padding to the top, to push the div to the bottom of the container.
* And some padding the left to keep it away from the edges.

```
.left ul{
	padding-left: 30px;
	padding-top: 90px;
}
```

* This is for the left div.

* Now i'm going to do the same for the right div.




