
Project directory structure
=============================

Example of how to structure here: https://blog.risingstack.com/node-hero-node-js-project-structure-tutorial/

## Rule 1 - Organize your Files Around Features, Not Roles

The style of keeping all css together, all controllers together etc in a css/controllers dir is old-school thinking. 

Principles - keep files together that work together in a dir named for the module/component. 

* Easier to find, don't have to open multiple dirs to work with the code. 
* Easier to maintain, grow, move/remove features this way. 
* Relative links to other files are generally shorter


## Rule 2 - Don't Put Logic in index.js Files. Use these files only to export functionality, like:

		// product/index.js
		var product = require('./product')

		module.exports = {  
			create: product.create
		}
		

## Rule 3 - Place Your Test Files Next to The Implementation. Put your additional test files to a separate test folder to avoid confusion.

		.
		├── test
		|   └── setup.spec.js
		├── product
		|   ├── index.js
		|   ├── product.js
		|   ├── product.spec.js
		|   └── product.hbs
		├── user
		|   ├── index.js
		|   ├── user.js
		|   ├── user.spec.js
		|   └── user.hbs


## Rule 4 - Use a config Directory

To place your configuration files, use a config directory.

		.
		├── config
		|   ├── index.js
		|   └── server.js
		├── product
		|   ├── index.js
		|   ├── product.js
		|   ├── product.spec.js
		|   └── product.hbs


## Rule 5 - Put Your Long npm Scripts in a scripts Directory

Create a separate directory for your additional long scripts in package.json

		.
		├── scripts
		|   ├── syncDb.sh
		|   └── provision.sh
		├── product
		|   ├── index.js
		|   ├── product.js
		|   ├── product.spec.js
		|   └── product.hbs



## Front-end files/dirs

	src/ 
is the source of your files. If you have a front-end and back-end it might be better to have two dirs, a 'client' and a 'server' dir.

	app/ 	
Not required if you use src, server, or client dirs. Another synonym for src.

	client/			
Frontend files. Webpack looks for all .html/.css files etc and bundles to a dist dir.
			
	config/		
Configuration files for the server and front end app, not for webpack.config.js, .gitignore, package.json etc.

	build/	dist/	
AKA 'dist' in some cases. Webpack bundled app files - is this just for the frontend files? Yes. 
		
	common/		
Common js files, stuff shared by two or more modules... could be called 'shared'
	
	utils/     
files containing utility methods, aka common/shared??
		
	recipe/ 	
A module dir. Module is synonym for component maybe? All files for the module go in here, including tests, html, js, controllers. This also keeps relative paths short

	recipe.controller.js	
Controller file for the recipe features

	recipe.model.js		
For mongoose model definition
				
	recipe.helper.js		
Helper methods. Maybe these are helper methods that aren't relevant for the 'common'/'shared' dir but only for this one module.

	recipe.test.js		
Test file for the recipe feature. Configuring the test runner (either Jest or Mocha) is easy: just make it run the tests in ./src/**/*Test.js.

	recipe.html			
View template for the recipes... so now this is front end... it goes in client dir.

	recipe.css			
css/sass whatever. 

	recipe.service.js    
Functions and logic linked to from the routes files. Is this what was tradionally the model file?
		
	shopping-cart/			
Just another module
			
	shoppingCart.controller.js		
Do you need the word controller or is this implicit? Also seen all controllers just called controller.js and you know which is which purely by the dir tree location.
		
	dashboards/				
If multiple 'screens' (i.e. what the end user sees on-screen. ) are required, each gets it's own sub-dir. The term 'screens' is used instead of view as it's clearer that the screen is what you're looking at on-screen. This term was recommended by an article I read.

	index.html		
Use webpack-dev-server to launch app for development, so this file can live here in src and still be launched. For deployment, webpack will move it to the correct root location in 'build' dir.

	sidebar.js		
Example of main app files... should this be in the app dir, or in a common/shared dir as it's used on all pages? 

	nav.js			
Another example of main app files.
	
	reducers.js		
Some react thing. There appears to also be reducers in the component dirs



## Back-end server files/dirs

	server/ 	
all private backend files. This is where all Express files should go. i.e. those that don't require webpack to bundle for the browser. How do you deploy the backend? David at hack-night suggests just using git to deploy to Heroku etc. 

	server.js / app.js		
Or app.js or split out to both; the server.js has the basics, and app.js has other stuff? app.js is synonym for a controller file? server.js file should not have logic, the logic goes in the controllers

	config/		
Configuration files for the server and front end app, not for webpack.config.js, .gitignore, package.json etc.

	routes.js 		
Just the routes, linking to the relevant controllers in the module dirs, like we saw in the url shortener examples. David at hack night said this would be a good way to do it. The routes file is then kept short and you can see at a glance all the routes.

	content/ public/ assets/      	
AKA 'public' or 'assets' Static pages like About Us, Privacy Policy etc. Maybe also these files could just go in root of app? untidy? what about images? they stay with the module dir surely? Does webpack scoop them up? This dir would go in the server dir. This would go in the server dir and be a back-end dir.


## General Development files/dir

	scripts/
Long npm Scripts, ie. ones not run in a single one-liner in package.json?

	tools/                     
Build automation scripts and utilities

	build.js               
Builds the project from source to output (build) folder

	bundle.js              
Bundles the web resources into package(s) through Webpack

	clean.js               
Cleans up the output (build) folder...  eh? what does this mean? deletes the files, prior to running webpack? or tidies up somehow?

	copy.js                
Copies static files to output (build) folder

	deploy.js              
Deploys your web application

	run.js 
Helper function for running build automation tasks

	runServer.js          
Launches (or restarts) Node.js server

	start.js              
Launches the development web server with "live reload"

	webpack.config.js      
Configurations for client-side and server-side bundles... thought this had to go in the root?	

	node_modules/
Created by npm for dependencies.

	.gitignore
Files not to be put under git control and uploaded to git.

	.babelrc		
babel transpiling file

	package.json
Lists dependencies and script files

	Procfile		
Think it's a backup for if a server can't read a package.json file? Uppercase P

	webpack.config.js
Webpack configuration
		
	README.md
Info about the app, how to run it, etc.
	
	.editorconfig		
http://editorconfig.org/  Default formatting for editors

	.eslintignore
Locations not to lint

	.eslintrc.json
Confirguratin for eslint linter

	.env
Environment variable package config for dotenv npm module. Stores passwords. Should be added to .gitignore

	.env-sample
This contains a copy of .env but with the passwords removed. It is included in git so that other devs can see the format required if they want to set up a copy of your app.





## Notes

How to separate the server from the frontend? Use 'server' and 'client' dirs


When my team migrated to node.js over a year ago, we settled on the following directory structure for its simplicity:

* client/ -- all public-facing code, e.g. clientside JavaScript, CSS, images.
* server/ -- all serverside code like routes, models, middleware
* app.js -- loads routes, middleware, and exposes an express app
* web.js -- main entry point, starts web server located in app.js


My controllers doesn't talk directly to the models (Mongoose) and models doesn't contain any static functions/methods. The business logic is in the services and the instrumentation of those happens in the controllers. The idea is to be able to easily switch the DB layer, e.g throwing mongoose for native mongo client.

Any code that will talk to an outside service/application is going into the services layer.
And a helpers folder to keep wide useful functions that doesn't belong to any specific domain model.

	Oh darn, seems like my interpretation of a data model is completely different. My data models aren't directly derived models from some base class of an orm/odm/etc. I usually write some es6 classes which use some kind of database object (like a knex.js instance) to run queries. These models export some interface, which more or less sounds like your service layers.  
    
Exactly, so your Models are my Services. In the case of Mongoose, you have to have Models files and I don't like my app to be dependent on those directly. This is why I have the services folder.