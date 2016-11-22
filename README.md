
Project directory structure
=============================


Principles - keep files together that work together in a dir named for the module/component. Easier to find, don't have to open multiple dirs to work with the code. Easier to maintain, grow, move/remove features this way. The style of keeping all css together, all controllers together etc is old-school thinking. Example of how to structure here: https://blog.risingstack.com/node-hero-node-js-project-structure-tutorial/

Rule 1 - Organize your Files Around Features, Not Roles

Rule 2 - Don't Put Logic in index.js Files
		Use these files only to export functionality, like:

		// product/index.js
		var product = require('./product')

		module.exports = {  
		  create: product.create
		}
		
Rule 3 - Place Your Test Files Next to The Implementation
		Put your additional test files to a separate test folder to avoid confusion.
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

Rule 4 - Use a config Directory

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
		
Rule 5 - Put Your Long npm Scripts in a scripts Directory

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


src/		// src is what webpack uses to bundle all front-end code right? Maybe the src file is just front end stuff, and the other backend Express stuff should be in a dir named 'server' as it doesn't get bundled? or does it? Maybe don't need a 'src' dir and instead just use 'app' and point webpack at app as the source dir?

If app is just a front end, or just backend, like if its j
	
	app/ 	// backend logic? or fronted stuff also? should app go in the src file? it's going to be private anyway, so maybe should be in the root? though does webpack pick and choose the required files and bundle them anyway?
		
		common/		// common js files, stuff shared by two or more modules... could be called 'shared'
		utils/      // files containing utility methods, aka common/shared??
		
		recipe/ 	// module dir. Module is synonym for component maybe? All files for the module go in here, including tests, html, js, controllers. This also keeps relative paths short
				recipe.controller.js	// files for a module don't have subdirs
				recipe.model.js		// for mongoose model definition
				recipe.helper.js		// helper methods. Maybe these are helper methods that aren't relevant for the 'common'/'shared' dir but only for this one module.
				recipe.test.js		// Configuring the test runner (either Jest or Mocha) is easy: just make it run the tests in ./src/**/*Test.js.
				recipe.html			// view template for the recipes... so now this is front end... so does it go in client dir?
				recipe.css			// css/sass whatever. 
				recipe.service.js    // functions and logic linked to from the routes files. Is this what was tradionally the model file?
		
		shopping-cart/			// another module
			shoppingCartController.js		// do you need the word controller or is this implicit? Also seen all controllers just called controller.js and you know which is which purely by the dir tree location.
		
		dashboards/				// if multiple 'screens' are required, each gets it's own sub-dir. The term 'screens' is used instead of view as it's clearer that the screen is what you're looking at on-screen.
			adminDash/
				adminDashController.js
				adminDashTest.js
				adminDash.html
			supplierDashboard/
				supplierDashController.js
				supplierDashTest.js
				supplierDash.html
				
		server.js 		// or app.js or split out to both; the server.js has the basics, and app.js has other stuff? app.js is synonym for a controller file? server.js file should not have logic, the logic goes in the controllers
		routes.js 		// just the routes, linking to the relevant controllers in the module dirs, like we saw in the url shortener examples. This is backend file right?
		index.html		// use webpack-dev-server to launch app for development, so it can live here in src
		sidebar.js		// example of main app files... should this be in the app dir, or in a common/shared dir? This a front-end file.
		nav.js			// another example of main app files. This a front-end file.
		reducers.js		// some react thing. There appears to also be reducers in the component dirs
		content/       	// aka Public?? Static pages like About Us, Privacy Policy etc. Maybe also these files could just go in root of app? untidy? what about images? they stay with the module dir surely? Does webpack scoop them up? 
		client/			// frontend files - this may not be needed? is this relevant? wait, webpack looks for all .html/.css files etc and bundles to a dist dir, so maybe don't need this any more? moduleName might become the top level under app/ ? Or maybe call it 'modules'. Maybe client and src folders are redundant with the use of webpack to pull out the required files from the dir tree.
			
config/		// configuration files for the server/framework - how does this work if some of these config files live in the root, e.g. webpack.config.js, .gitignore, package.json
build/		// aka 'dist' in some cases. Webpack bundled app files - is this just for the frontend files? How do you deploy the backend if all the files are mixed up? Known as 'public' once deployed?
server/ 	// all private backend files. Maybe this is where all Express files should go? i.e. those that don't require webpack to bundle for the browser? Don't know about this at all?
scripts/	// long npm Scripts, ie. ones not run in a single one-liner in package.json?
tools/                     # Build automation scripts and utilities
	build.js               # Builds the project from source to output (build) folder
	bundle.js              # Bundles the web resources into package(s) through Webpack
	clean.js               # Cleans up the output (build) folder...  eh? what does this mean? deletes the files, prior to running webpack? or tidies up somehow?
	copy.js                # Copies static files to output (build) folder
	deploy.js              # Deploys your web application
	run.js                 # Helper function for running build automation tasks
	runServer.js           # Launches (or restarts) Node.js server
	start.js               # Launches the development web server with "live reload"
	webpack.config.js      # Configurations for client-side and server-side bundles... thought this had to go in the root?	
node_modules/
.gitignore
.babelrc		// babel transpiling file
package.json
Procfile		// think it's a backup for if a server can't read a package.json file? Uppercase P
webpack.config.js
README.md
.editorconfig		// http://editorconfig.org/  Default formatting for editors





UNANSWERED QUESTIONS

How to separate the server from the frontend?

What about static files?
Public/       // Static Files Directory - do we no-longer need this with webpack, or is this aka 
---- css
---- js
---- svg





When my team migrated to node.js over a year ago, we settled on the following directory structure for its simplicity:
client/ -- all public-facing code, e.g. clientside JavaScript, CSS, images.
server/ -- all serverside code like routes, models, middleware
app.js -- loads routes, middleware, and exposes an express app
web.js -- main entry point, starts web server located in app.js





My controllers doesn't talk directly to the models (Mongoose) and models doesn't contain any static functions/methods. The business logic is in the services and the instrumentation of those happens in the controllers. The idea is to be able to easily switch the DB layer, e.g throwing mongoose for native mongo client.
Any code that will talk to an outside service/application is going into the services layer.
And a helpers folder to keep wide useful functions that doesn't belong to any specific domain model.


    Oh darn, seems like my interpretation of a data model is completely different. My data models aren't directly derived models from some base class of an orm/odm/etc. I usually write some es6 classes which use some kind of database object (like a knex.js instance) to run queries. These models export some interface, which more or less sounds like your service layers.
    permalink
    embed
    save
    parent
    give gold
    [–]kostarelo 1 point 8 months ago 
    
    
        Exactly, so your Models are my Services. In the case of Mongoose, you have to have Models files and I don't like my app to be dependent on those directly. This is why I have the services folder.



