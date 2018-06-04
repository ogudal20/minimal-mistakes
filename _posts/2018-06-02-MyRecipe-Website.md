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

