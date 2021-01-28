

LaunchCode CoderGirl C# Web Development

## SQL / MySQL 

Written in markdown format to easily copy-paste for reviewing purposes:

#### 17.5. Studio: Movie SQLs

8. If you list all of the data from the `movies` table (`SELECT * FROM movies;`), you will see a column of director ID numbers. This data is not particularly helpful to a user, since they probably want to see the director names instead. Use an `INNER JOIN` in your SQL command to display a list of movie titles, years released, and director last names.

   ```mysql
   SELECT 
       title, year_released, last_name
   FROM
      movies AS m
   INNER JOIN
   	directors AS d
   ON m.director_id = d.director_id
   ```

   

9. List all the movies in the database along with the first and last name of the director. Order the list alphabetically by the director’s last name.

   ```mysql
   SELECT 
       title, year_released, first_name, last_name
   FROM
      movies AS m
   INNER JOIN
   	directors AS d
   ON m.director_id = d.director_id
   ORDER BY last_name ASC;
   ```

   

10. List the first and last name for the director of *The Incredibles*. You can do this with either a join or a `WHERE` command, but for this step please use `WHERE`.

```mysql
SELECT 
    first_name, last_name
FROM
   movies AS m
INNER JOIN
	directors AS d
ON m.director_id = d.director_id
WHERE title = "The Incredibles";
```

11.  List the last name and country of origin for the director of *Roma*. You can do this with either a join or a `WHERE` command, but for this step please use a join.

    ```mysql
    SELECT 
        last_name, country
    FROM
       movies AS m
    INNER JOIN
    	directors AS d
    ON m.director_id = d.director_id
    WHERE title = "Roma";
    ```

12. Delete a row from the `movies` table. What consequence does this have on `directors`? List the contents of both tables to find out.

    --- Deleting a row didn't change the data in the directors table

    ```mysql
    DELETE FROM movies WHERE movie_id = 15;
    ```

    13. Try to delete one person from the `directors` table. What error results from trying to remove a director?

    --- Error Code: 1451. Cannot delete or update a parent row: a foreign key constraint fails (`movie-buff`.`movies`, CONSTRAINT `movies_ibfk_1` FOREIGN KEY (`director_id`) REFERENCES `directors` (`director_id`))

### 18.2 Database Management

1. #### Setup:

   ```mysql
   CREATE TABLE writing_supply (
   	supply_id INT PRIMARY KEY AUTO_INCREMENT,
       utensil_type ENUM("Pencil", "Pen"),
       num_drawers INT
   );
   
   CREATE TABLE pencil_drawer (
   	drawer_id INT PRIMARY KEY AUTO_INCREMENT,
       pencil_type ENUM("Wood", "Mechanical"),
       quantity INT,
       refill BOOLEAN,
       supply_id INT,
       FOREIGN KEY (supply_id) REFERENCES writing_supply(supply_id)
   );
   
   CREATE TABLE pen_drawer (
   	drawer_id INT PRIMARY KEY AUTO_INCREMENT,
       color ENUM("Black", "Blue", "Red", "Green", "Purple"),
       quantity INT,
       refill BOOLEAN,
       supply_id INT,
       FOREIGN KEY (supply_id) REFERENCES writing_supply(supply_id)
   );
   ```

   **Using subqueries**
   
   1. Retrieve the `supply_id` values for any `writing_supply` containers that hold pens.
   2. Using the `supply_id` values, retrieve the ID and `color` values for any drawers in the last container that hold 60 or more pens.
   
   ```mysql
   SELECT 
       drawer_id, color
   FROM
       pen_drawer
   WHERE
       supply_id IN (SELECT 
               supply_id
           FROM
               writing_supply
           WHERE
               utensil_type = 'Pen')
           AND quantity >= 60;
   ```
   
   
   
   #### 18.4 Exercises:
   
   1. Create the tables
   
      plant table:
   
      ```mysql
      CREATE TABLE plant (
      	plant_id INT PRIMARY KEY AUTO_INCREMENT,
          plant_name VARCHAR(150),
          zone INT,
          season VARCHAR(10)
      );
      ```
   
      seeds table:
   
      ```mysql
      CREATE TABLE seeds (
      	seed_id INT PRIMARY KEY AUTO_INCREMENT,
          expiration_date DATE,
          quantity INT,
          reorder BOOL,
          plant_id INT,
          FOREIGN KEY (plant_id) REFERENCES plant(plant_id)
      );
      ```
   
      garden_bed table:
   
      ```mysql
      CREATE TABLE garden_bed (
      	space_number INT PRIMARY KEY AUTO_INCREMENT,
          date_planted DATE,
          doing_well BOOL,
          plant_id INT,
          FOREIGN KEY (plant_id) REFERENCES plant(plant_id)
      );
      ```
   

### 18.5 Studio

**Warm-up queries**

1. Return the mystery book titles and their ISBNs.

```mysql
SELECT title, isbn FROM book WHERE genre_id = 6;
```

2.  ```mysql
   SELECT 
       title, first_name, last_name
   FROM
       author
   LEFT OUTER JOIN book
   ON author.author_id = book.author_id
   WHERE
       deathday IS NULL;
    ```

**Loan Out a Book**

1. ```mysql
   UPDATE book SET available = FALSE WHERE book_id = 6;
   ```

2. ```mysql
   INSERT INTO loan (patron_id, date_in, book_id)
   VALUE ();
   ```

3. 