# Tara's codeless notes for the Mod2 coding challenge

## Before starting the timer
* Make sure you are well-rested, well-nourished, and comfortable
* Read the README all the way through
* Plan out your app on paper or whiteboard
  * What models/tables and attributes/fields do we need?
  * What relationships will we need to create?
  * What methods will we need?  Which Classes will those methods be called on? 

## Create a new Rails API project
* In terminal: `rails new <project_name> **--api**`
* `cd <project_name>`

## Uncomment CORS code
* In `config/initializers.rb` go to `cors.rb` and uncomment the method there
* In `Gemfile` uncomment the line: `gem 'rack-cors'`
* `bundle install`

## Generate resources for each Class
* Think through the ERD / plan fields and relationships
* For each class: `rails g resource Class field1 field2 field3`
  * default field type is 'string' -- any other type needs to be specified
  * example: `some_number:integer` or `some_table:references`
* The generator will create these resources:
  * complete migration file for each table in `db\migrate`
  * blank controller for each class in `app\controllers`
  * model for each class in `app\models` -- may need editing, based on relationships
  * all of the routes, which can be verified by running `rails routes` and scrolling to the top

## Migrate database
* check each of the migration files in `db\migrate`; edit if desired
  * take the timestamps out if you won't be using them to reduce visual clutter
* run `rails db:migrate NAME=create_class_table` in terminal
  * this will add `schema.rb` and `development.sqlite3` to the db folder

## Complete setup of relationships between models 
* if the tables have relationships, they need to be configured in the models in `app\models`
* For a 1:M relationship, the 'M' table should already have a line of: `belongs_to :1_table`
* But for the '1' table we need to manually add `belongs_to :M_table`

## Seed the database
* Open `db\seeds.rb`
* Call the `.destroy_all` method for each class
* Create seeds with the `Model.create(k:v,k:v)` method
* For relationships, assign seeds in the '1' table to variables.  These variables can be then be used as parameters for creating the 'M' table seeds
* Run `rails db: seed` in terminal
* To check that seeds have been saved to db, run `rails c` then `Model.all`

## Write the controller methods
* Edit the controller files for each model in `app\controllers`
* Define 5 methods: index, show, create, update, and destroy
* **index** 
  * set a pluralized @ variable to the `Class.all` method
  * `render json:` pluralized @ variable
  * for included relationship, add `, include: [:models]`
  * note: all the rest of the @ variables are singular, only **index** uses plural
* **show**
  * set an @ variable to the method `Class.find_by(id: params[:id])`
  * `render json:` @ variable
  *  for included relationship, add `, include: [:models]`
* **create**
  * set an @ variable to the method `Class.create(field1: params[:field1], field2: params[:field2])`
  * `render json:` @ variable
* **update**
  * set an @ variable to the method `Class.find_by(id: params[:id])`
  * directly on that variable, call `.update(field1: params[:field1], field2: params[:field2])`
  * `render json:` @ variable
* **destroy**
  * set an @ variable to the method `Class.find_by(id: params[:id])`
  * directly on that variable, call `.destroy`
  * `render json:` @ variable
* test all of the routes out with Postman

## Create the frontend file structure
* From the terminal, `take frontend`
* `touch index.html index.js styles.css`
* note: remember you will need to be in this folder when you run lite-server

## Write the starter HTML
* Open index.html
* Type an `!`, then trigger the Emmet abbreviation
* In the head, change the title
* Just above the title, add a `<script></script>` tag with the `src` set to index.js.  Add the word `defer` to the opening tag.
* In the body, create some kind of container element that we can append to later

## Write the starter JS
* Open index.js
* **select the enclosing container**
  * set a constant $ variable to the value of `document.querySelector('add-selector-here')` to select the container we built in the HTML
* **fetch the data**
* take a look at: https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch
* start with the first two lines of the top example - the `fetch` and the first `.then`
* but within our second `.then`, we will create the code to iterate over the collection and manipulate the DOM
* `.then(models => { models.forEach(model => {`
  * **create a new HTML container element
  * for each item in our collection, we are going to make an HTML container with some HTML elements
  * set a $ constant to `document.createElement('element')` -- element will likely be a div
  * directly on that constant, set a className property to = `"my-class-name"` -- note syntax here uses **=** sign, not parens
  * **create new HTML elements that we want to display for each item
  * set $ constants to `document.createElement('element')` -- elements can be headers, images, anything
  * ^^ repeat as needed
  * **manipulate the elements**
  * on each $ item constant created above, call something like `.innerText = model.attribute` (as one example)
  * other manipulations are possible here, like [string interpolation](https://dmitripavlutin.com/string-interpolation-in-javascript/)
  * ^^ note that string interpolations must be enclosed in backticks
  * for an image use `.src`, and assign to the image path
  * **attach to the dom**
  * on the *new item container*, call `.append($var1, $var2, $var3)
  * on the *enclosing container* call `.append($newContainer)`
  * be sure to add all the closing puncuations, likely nested 4 deep
 
## Run the app!
* In terminal, run `rails s` to start the rails server
* In a different terminal window, run `lite-server` to run the webserver
* Note: Be sure you are in the correct folder when launching the servers
  * if lite-server gives us problems, see https://www.npmjs.com/package/lite-server
* Open in a browser to watch the magic happen!
