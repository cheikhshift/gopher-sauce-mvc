# Tutorial on Go MVC
Build website pages with Go lang code.

![enter image description here](http://www.unixstickers.com/image/data/stickers/golang/Golang%20mashup.sh.png)


### Requirements

- Go lang
- [Gopher Sauce](https://github.com/cheikhshift/Gopher-Sauce/blob/master/readme.md)

## Getting started
(unix commands)
					
	mkdir new-project
	cd new-project		
	gos --make

Please make sure your current working directory is `new-project` (or any other name).

## Create templates
This tutorial will use 4 templates :
 - Page
 - Login block
 - Demo panel one
 - Demo panel two

### Page
This is the wrapper which will be used with each page. `Page` MVC will have one property `Body` which is a string.

#### Add model to project
Add the following tag to your `<header>` tag within your `gos.gxml` file.

	<struct name="PanelPage">
		Body string
	</struct>

#### Create template
Create a new file named `page.tmpl` within your `tmpl` directory.

Contents of file : 
(Lookout for {{ .Body }} this is how model properties are passed.For example : {{ .< Property > }})

	 <!DOCTYPE html>
			<html lang="en">
			  <head>
			    <!-- Required meta tags -->
			    <meta charset="utf-8">
			    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
			
			    <!-- Bootstrap CSS -->
			    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-alpha.6/css/bootstrap.min.css" integrity="sha384-rwoIResjU2yc3z8GV/NPeZWAv56rSmLldC3R/AZzGRnGxQQKnKkoFVhFQhNUwEyJ" crossorigin="anonymous">
			  </head>
			  <body>
			   
			{{ .Body }}
			    <!-- jQuery first, then Tether, then Bootstrap JS. -->
			    <script src="https://code.jquery.com/jquery-3.1.1.slim.min.js" integrity="sha384-A7FZj7v+d/sdmMqp/nOQwliLvUsJfDHW+k9Omg/a/EheAdgtzNs3hpfag6Ed950n" crossorigin="anonymous"></script>
			    <script src="https://cdnjs.cloudflare.com/ajax/libs/tether/1.4.0/js/tether.min.js" integrity="sha384-DztdAPBWPRXSA/3eYEEUWrWCy7G5KFbe8fFjk5JAIxUYHKkDx6Qin1DkWx51bBrb" crossorigin="anonymous"></script>
			    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-alpha.6/js/bootstrap.min.js" integrity="sha384-vBWWzlZJ8ea9aCX4pEW3rVHjgjt7zpkNpZk+02D9phzyeVkE+jo0ieGizqPLForn" crossorigin="anonymous"></script>
			  </body>
			</html>

#### Link template
 Add the following tag to your `<templates>` tag within your `gos.gxml` file.

	<template name="Page" tmpl="page" struct="PanelPage"/>
 
### Login block
This is a code snippet of a login form. At this point the endpoint the form is sending data to does not exist yet. 

#### Create template
Create a new file named `login.tmpl` within your `tmpl` directory.

Contents of file : 
	
	<div class="list-group list-group-item" style="margin:0 auto;margin-top:3.2em; max-width:400px">
	
		<form action="/login" method="post">
			<p class="text-muted">Server side page assembly</p>
			<p><input type="text" name="name" class="form-control" placeholder="Enter your first name" /></p>
		<p><input type="submit" class="btn btn-lg btn-block btn-success"  value="login"/></p>
	
		</form>
	</div>

#### Link template
 Add the following tag to your `<templates>` tag within your `gos.gxml` file. Since the struct is empty `gos` will revert to struct : `NoStruct`.

	<template name="Login" tmpl="login" struct=""/>

### Demo panel one
This is a code snippet of a simple navigation panel. The panel will display the name entered on authentication.

#### Add model to project
Add the following tag to your `<header>` tag within your `gos.gxml` file.

	<struct name="UserPanel">
			Name string
	</struct>

#### Create template 
Create a new file named `panel.tmpl` within your `tmpl` directory.

Contents of file : 

	<div class="list-group list-group-item" style="margin:0 auto;margin-top:3.2em; max-width:400px">
		<p><a class="btn btn-danger" href="/logout">Logout</a></p>
		<h3>Welcome {{ .Name }} </h3>
		<p><a href="two">Visit Page Two</a></p>
	</div>

#### Link template
 Add the following tag to your `<templates>` tag within your `gos.gxml` file.

	<template name="Panel" tmpl="panel" struct="UserPanel"/>

### Demo panel two
This is a code snippet of a simple navigation panel. The panel will display the name entered on authentication.

#### Create template 
Create a new file named `panel_two.tmpl` within your `tmpl` directory.

Contents of file : 

	<div class="list-group list-group-item" style="margin:0 auto;margin-top:3.2em; max-width:400px">
		<p><a class="btn btn-danger" href="/logout">Logout</a></p>
		<h3>Welcome {{ .Name }} </h3>
		<p><a href="one">Visit Page One</a></p>
	</div>

#### Link template
 Add the following tag to your `<templates>` tag within your `gos.gxml` file.

	<template name="PanelTwo" tmpl="panel_two" struct="UserPanel"/>

## Authentication

#### Add /login endpoint
 Add the following tag to your `<endpoints>` tag within your `gos.gxml` file.

	<end path="/login" type="POST">
	</end>

##### Add Go code
Within the same `<end>` tag add the following snippets (in order):

Save name as session variable :

	session.Values["name"] = r.FormValue("name")

Save session before redirection (crucial) :

	session.Save(r,w)

Redirect user to panel :

	http.Redirect(w,r,"/panel/one",307)

`<end>` tag should look like this : 

	 <end path="/login" type="POST">
			session.Values["name"] = r.FormValue("name")
			session.Save(r,w)
			http.Redirect(w,r,"/panel/one",307)
	  </end>

#### Add /logout endpoint
 Add the following tag to your `<endpoints>` tag within your `gos.gxml` file.

	<end path="/logout" type="GET">
	</end>

##### Add Go code
Within the same `<end>` tag add the following snippets (in order):
Delete `name` session variable :

	delete(session.Values, "name")

Save session before redirection (crucial) :

	session.Save(r,w)

Redirect user to panel. :

	http.Redirect(w,r,"/panel/one",307)

`<end>` tag should look like this : 

	 <end path="/logout" type="GET">
			delete(session.Values, "name")
			session.Save(r,w)
			http.Redirect(w,r,"/panel/login",307)
	  </end>

## Assemble web page

#### Add /panel endpoint
 Add the following tag to your `<endpoints>` tag within your `gos.gxml` file. The endpoint will be set to type `star`, this will make any sub set paths use this endpoint code. For example `/panel/path/test` and `/panel/virtual/page` will invoke the same code functionality. 

	<end path="/panel" type="star">
	</end>

##### Add Go code
Within the same `<end>` tag add the following snippets  (in order):

Initialize page wrapper model :

	page := PanelPage{}

Add some security. The following statement will evaluate if the session variable `name` exists, if not the login form will be set as the `Page.Body`. The  function `bLogin` is used to generate HTML of your template. Since no `struct` was specified Gos will revert to struct `NoStruct` as the template's model. To build any template the syntax is `b<template name>`. If the user is logged in the Body of the `Page` template is changed as needed. :


	if _, ok := session.Values["name"]; ok  {
		/* User is logged in */
			route := strings.Split(r.URL.Path, "/")
			if len(route) > 2 {
				name := session.Values["name"].(string)
				panel := UserPanel{Name: name }
				if route[2] == "one" {
				 	page.Body = bPanel(panel)
				} else {
					page.Body = bPanelTwo(panel)
				}
			} else {
				 http.Redirect(w,r,"/panel/one",307)
			}
	} else {
		page.Body = bLogin(NoStruct{})
	}

Set content type :

	w.Header().Set("Content-Type",  "text/html")

Compile template :

	html := bPage(page)
	w.Write([]byte(html)) 	

`<end>` tag should look like this :

       <end path="/panel"  type="star" >
      
      	// writing directly to control headers

		page := PanelPage{}
		
		if _, ok := session.Values["name"]; ok  {
			route := strings.Split(r.URL.Path, "/")
			if len(route) > 2 {
				name := session.Values["name"].(string)
				panel := UserPanel{Name: name }
				if route[2] == "one" {
				 	page.Body = bPanel(panel)
				} else {
					page.Body = bPanelTwo(panel)
				}
			} else {
				 http.Redirect(w,r,"/panel/one",307)
			}

		} else {

			page.Body = bLogin(NoStruct{})

		}

		
		w.Header().Set("Access-Control-Allow-Origin", "*")
		w.Header().Set("Content-Type",  "text/html")
		w.Write([]byte(bPage(page)))
		  
      </end> 

### Testing
Run project :

	gos --run

Visit page : [localhost:8080/panel](http://localhost:8080/panel)

Download this Repository and run `gos --run` once in directory.
