**Q1. Where are we instantiating instances of the `Property` class?**

* In the `console.rb` file.

**Q2. Where are we defining the SQL that enables us to save the ruby `Property` object into the database?**

* In the `property.rb` file under the `models` directory.

**Q3. In `console.rb`, which lines modify the database?**

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

**Q4. Why do we not define the id of a `Property` object at the point we instantiate it (‘new it up’)?**

* Because we need to leave that up to Postgres itself. We'd confuse things in the table otherwise.

**Q5. Where and how do we assign the id (that is generated by the database) to the ruby `Property` object?**

* First we add in the `id` member to the `initialize()` block
* The assignment is done inside the `save()` method
* We use the following for the assignment
```ruby 
@id = db.exec_prepared("save", values)[0]["id"].to_i 
``` 

**Q6. Why do we put a guard (an `if` clause) on the `@id` attribute in the constructor?**

* We can't add an `id` argument to the `Porperty` constructors in the `console.rb` file
* The guard is to allow us to define instances of the class without having to explicitly specify an `id`

**Q7. Why are some of the CRUD actions represented by instance methods, and others by class methods?**

* If we want to be able to `save()`, `update()` or `delete()` a specific record, we need to use it on the corresponding instance the record belongs to.
* If we want to be able to find a record among a range of records, we must use a class method that would search through the entire table, not within a specific instantiation.

**Q8. What type of data structure is returned by calls to `db.exec_prepared()`? In the `save` method, how do we access the id from the returned data structure?**

* An array is always returned by `db.exec_prepared()`, to cover entries for tables that are of varying size.
* The `[0]` in `@id = db.exec_prepared("save", values)[0]["id"].to_i`, allows us to first access the element contained in the array returned by `db.exec_prepared()`, and `[0]["id"]` allows us to access the value in the `id` key of that element.

**Q9. Why do we use prepared statements when performing database operations?**

* To guard against SQL injection attacks. Malicious or interfering code will be escaped as a precaution.

# Extension Questions

Look at the `find_by_id` and `find_by_address` methods in the `Property` class.

**Q10. What do they take in as their arguments?**

* `find_by_id` takes an `INT` as the argument.
* `find_by_address` takes a `string` as the argument.

**Q11. What are their return values?**

* They both return objects of the `Property` class.