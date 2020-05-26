### Q1. Where are we instantiating instances of the `Property` class?

* In the `console.rb` file.

### Q2. Where are we defining the SQL that enables us to save the ruby `Property` object into the database?

* In the `property.rb` file under the `models` directory.

### Q3. In `console.rb`, which lines modify the database?

```sql
UPDATE properties SET (
      address,
      value,
      bedrooms,
      build
      ) =
      (
        $1, $2, $3, $4
      )
      WHERE id = $5;
```

```sql
DELETE FROM properties WHERE id = $1;
```

```sql
DELETE FROM properties;
```

### Q4. Why do we not define the id of a `Property` object at the point we instantiate it (‘new it up’)?

>Because we need to leave that up to the Postgres itself. We'd confuse things otherwise.

### Q5. Where and how do we assign the id (that is generated by the database) to the ruby `Property` object?

* First we add the `id` member in the `initialize()` block
* The assignment is done inside the `save()` method
* We use the following ```ruby @id = db.exec_prepared("save", values)[0]["id"].to_i ``` for the assignment

### Q6. Why do we put a guard (an `if` clause) on the `@id` attribute in the constructor?

* We can't add an `id` argument to the `Porperty` constructors in the `console.rb` file
* The guard is to allow us to define instances of the class without having to explicitly specify an `id`

### Q7. Why are some of the CRUD actions represented by instance methods, and others by class methods?

* If we want to be able to ```ruby save() ```, ```ruby update() ``` or ```ruby delete() ``` a specific record, we need to use it on the corresponding instance the record belongs to.
* If we want to be able to find a record among a range of records, we must use a class method that would search through the entore table, not within a specific instantiation.

### Q8. What type of data structure is returned by calls to `db.exec_prepared()`? In the `save` method, how do we access the id from the returned data structure?

* An array is always returned by `db.exec_prepared()`, to cover results for tables that are of varying size.
* The `[0]` in ```ruby @id = db.exec_prepared("save", values)[0]["id"].to_i ```, allows us to first access the element contained in the array returned by ```ruby db.exec_prepared() ```, and ```ruby [0]["id"] ``` allows us to access the value in the `id` key.

### Q9. Why do we use prepared statements when performing database operations?

* To guard against SQL injection attacks. If there's any attempy at malicious code being run, the attempy will fail with the code being automatically escaped.

# Extension Questions

Look at the `find_by_id` and `find_by_address` methods in the `Property` class.

### Q10. What do they take in as their arguments?

* ```ruby find_by_id ``` takes in an `INT` as the argument.
* ```ruby find_by_address ``` takes in a `string` as the argument.

### Q11. What are their return values?

* They both return objects of the ```ruby Property ``` class.