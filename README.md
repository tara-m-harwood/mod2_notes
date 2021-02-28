# Tara's codeless notes for the Mod2 coding challenge

## Create a new Rails API project
* In terminal: `rails new <project_name> --api`
* `cd <project_name>`
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
  * all of the routes, which can be verified by running `rails routes` (scroll up to top)

## Migrate database
* check each of the migration files in `db\migrate`; edit if desired
  * take the timestamps out if you won't be using them to reduce visual clutter
* run `rails db:migrate NAME=create_class_table` in terminal
  * this will add `schema.rb` and `development.sqlite3` to the db folder

## Complete setup of relationships between models 
* if the tables have relationships, they need to be configured in the models in `app\models`
* For a 1:M relationship, the 'M' table should already have a line of: `belongs_to :1_table`
* But for the '1' table we need to manually add `belongs_to :venue`

## Seed the database
* Open `db\seeds.rb`
* Call the `.destroy_all` method for each class
* Create seeds with the `Model.create(k:v,k:v)` method
* For relationships, assign seeds in the '1' table to variables.  These variables can be then be used as parameters for creating the 'M' table seeds
* Run `rails db: seed` in terminal
* To check that seeds have been saved to db, run `rails c` then `Model.all`

## Create the controllers
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
  * set an @ variable to the method 'Class.create(field1: params[:field1], field2: params[:field2])
  * `render json:` @ variable
* **update**
  * set an @ variable to the method `Class.find_by(id: params[:id])`
  * directly on that variable, call `.update(field1: params[:field1], field2: params[:field2])`
  * `render json:` @ variable
* **destroy**
  * set an @ variable to the method `Class.find_by(id: params[:id])`
  * directly on that variable, call `.destroy`
  * `render json:` @ variable
* test all of these out with Postman

## Uncomment CORS code
* I have no idea what this does, but it makes my rails app work!
* In `config/initializers.rb` go to `cors.rb` and uncomment the method there
* In `Gemfile` uncomment the line: `gem 'rack-cors'`




       

