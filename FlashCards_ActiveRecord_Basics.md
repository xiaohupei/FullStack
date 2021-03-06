## ActiveRecord Basics

### What is ActiveRecord's naming convention?

Your model class name should be in `UpperCamelCase`, singular form (e.g. `SportsCar`).

Your table name should be in `lower_snake_case`, plural form (e.g. `sports_cars`).

ActiveRecord's magical 1:1 mapping between records of your DB and instances of your models relies entirely on this convention!

### What should prefix every migration filename?

A timestamp (format: YYYYMMDDHHMMSS)! For instance: `db/migrate/20140725164644_create_restaurants.rb`

This way, your files are properly ordered in your `migrate` folder.

### the following migration to create a `doctors` table with a `name` and a `specialty`

```ruby
class CreateDoctors < ActiveRecord::Migration[5.1]
  def change
    create_table :doctors do |t|
      t.string     :name
      t.string     :specialty
      t.timestamps # adds 2 columns, `created_at` and `updated_at`
    end
  end
end
```

### Fetch a doctor of a given specialty

```Ruby
surgeons = Doctor.where(specialty: "Surgeon")
# => returns a collection (~ array) of all surgeons.
You can also pass a string as an argument to the .where class method:
young_doctors = Doctor.where("age < 35")
# => returns a collection (~ array) of all doctors under 35 years old.
surgeons = Doctor.where("specialty LIKE %surgery%")
# => returns a collection (~ array) of all doctors with 'surgery' in their specialty.
dentists_and_surgeons = Doctor.where(specialty: ["Dentist", "Surgeon"])
```

### the following migration to add an `age` column to the `doctors` table

```Ruby
class AddAgeToDoctors < ActiveRecord::Migration[5.1]
  def change
    add_column :doctors, :age, :integer
  end
end
```

### the ActiveRecord query called based on the following SQL query read in your application logs:

```Ruby
SELECT * FROM restaurants WHERE rating >= 4 ORDER BY rating ASC LIMIT 5;
Restaurant.where("rating >= 4").order(rating: :asc).first(5)
```

### How do you retrieve all instances of a given ActiveRecord model?

```Ruby
By calling .all on your model!
doctors = Doctor.all
# => returns a collection (~ array) of all Doctor instances.
```

### How do you run pending migrations? By running the following in your terminal:

```Ruby
rake db:migrate
```

### what is a migration

You create a migration when you need to change your database's **schema** (add / remove table), (add / remove column), etc...

It's a change in the **structure** of the database, but it leaves the data unchanged.

### What is ActiveRecord

ActiveRecord is a design pattern that maps your objects to a relational database.

It abstracts SQL queries and comes with a set of simple methods and tools to manage your DB in ruby / CL.

### What rake task should you run in your terminal to create your DB?

```
rake db:create
```

Consider the following model:

```ruby
class Doctor < ActiveRecord::Base
end
```

### Assuming you already created a DB with a `doctors` table, how would you add a new doctor in your DB?

dr_house = Doctor.new(name: "Greg House")

dr_house.save

### How do you retrieve a specific record of a given ActiveRecord model?

By calling `.find(id)` on your model!

```Ruby
first_doctor = Doctor.find(1)
# => returns the instance of Doctor with the id 1
If you don't know the id of the record you want to retrieve, you can also call .find_by(attribute: value):
house = Doctor.find_by(name: "Greg House")
# => returns the first instance of Doctor with the name "Greg House"
```

### How do you retrieve the first instance of an ActiveRecord model? By calling `.first` on the model.

```ruby
Restaurant.first
# returns the first restaurant in the DB (lowest id).
```

### How do you retrieve the number of instances of a given ActiveRecord model? By calling `.count` on your model!

```Ruby
Doctor.count
# => returns the number of Doctor instances (Integer).
```

### What is the SQL query generated by `Doctor.all`?

```
SELECT * FROM doctors;
```

Note that every ActiveRecord query generates an SQL query that you can read in your application logs in your terminal!

### The generic syntax of a migration to add a column to a given table. table names are always plural!

```ruby
class AddColumnToTable < ActiveRecord::Migration[5.1]
  def change
    add_column :table, :column, :type
  end
end
```

`::find_by` returns the **first record** verifying the condition, whereas `::where` returns **every record** verifying the condition, in an array.

Note that `::where` always returns an array (even if there's zero or one result), and `::find_by` returns an instance or nil!