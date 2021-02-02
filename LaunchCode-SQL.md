

LaunchCode CoderGirl C# Web Development

## SQL / MySQL 

Written in markdown format to easily copy-paste for reviewing purposes:

#### 17.5. Studio: Movie SQLs

1-7:

```mysql
SELECT title FROM movies;

SELECT title, year_released
FROM movies
ORDER BY year_released Desc; 

SELECT * FROM directors
ORDER BY country ASC;

SELECT * FROM directors
ORDER BY country, last_name ASC;

INSERT INTO directors (first_name, last_name, country)
VALUES ("Rob", "Reiner", "USA");

SELECT last_name, director_id FROM directors
WHERE first_name="Rob" AND last_name="Reiner";

INSERT INTO movies (title, year_released, director_id)
VALUES ("The Princess Bride", 1987, 11);
```



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
   

### 18.5 Studio: A Library

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
   
   Another possible answer:

```mysql
SELECT 
    book.title, author.first_name, author.last_name
FROM
    book
INNER JOIN author
ON book.author_id = author.author_id
WHERE
    author.deathday IS NULL;
```



**Loan Out a Book**

1. Change `available` to `FALSE` for the appropriate book
   
   ```mysql
UPDATE book SET available = FALSE WHERE book_id = 6;
   ```
   
2. Update the appropriate `patron` with the `loan_id` for the new row created in the `loan` table.
   
   ```mysql
   INSERT INTO loan (patron_id, date_out, book_id)
VALUE (27, now(), 15); 
   
   -- updated date_out entry because now() inserts time, only date needed
   UPDATE loan
   SET date_out = current_date()
   WHERE loan_id = 1;
   ```
   
   3. Update the appropriate `patron` with the `loan_id` for the new row created in the `loan` table

```mysql
UPDATE loan
SET patron_id = 3
WHERE loan_id = 1;

-- above was wrong, dhoud have updated the patron table, not loan table
UPDATE patron
SET loan_id = 1
WHERE patron_id = 3;
```

**Check a Book Back In**

LOAN TABLE:

```mysql
CREATE TABLE loan (
   loan_id INT AUTO_INCREMENT PRIMARY KEY,
   patron_id INT,
   date_out DATE,
   date_in DATE,
   book_id INT,
   FOREIGN KEY (book_id)
      REFERENCES book(book_id)
      ON UPDATE SET NULL
      ON DELETE SET NULL
);
```

REFERENCE_BOOK TABLE:

```mysql
CREATE TABLE reference_books (
   reference_id INT AUTO_INCREMENT PRIMARY KEY,
   edition INT,
   book_id INT,
   FOREIGN KEY (book_id)
      REFERENCES book(book_id)
      ON UPDATE SET NULL
      ON DELETE SET NULL
);
```



###### A foreign key with "set null on delete" means that if a record in the parent table is deleted, then the corresponding records in the child table will have the foreign key fields set to NULL. The records in the child table will ***not*** be deleted in SQL Server.

1. Change `available` to `TRUE` for the appropriate book.

```mysql
UPDATE book 
SET available = TRUE 
WHERE book_id = 15;

UPDATE loan
SET date_in = current_date()
WHERE loan_id = 1;

-- this didn't work, safe update mode
UPDATE loan
SET loan_id = NULL
WHERE patron_id = 3;
-- didn't work either, cannot set loan_id to NULL
UPDATE loan
SET loan_id = NULL
WHERE patron_id = 3 and loan_id > 0;
-- updated the book title but did not set the loan_id to NULL
UPDATE book
SET title = "PERSISTENCE"
WHERE book_id = 15;

-- tried deleting a book to check if loan_id in loan table will change to NULL. It did not but patron table loan_id is set to NULL
DELETE FROM book
WHERE book_id = 15;


-- Tried again, realizing my mistake from Loan Out a Book #3
-- added a new loan again, set the loan_id in the patron table
INSERT INTO loan (patron_id, date_out, book_id)
VALUE (46, current_date(), 35); 

UPDATE patron
SET loan_id = 2
WHERE patron_id = 46;

-- added this because I though updating the book details (parent table) will affect child table's records to be NULL
UPDATE book
SET title = "Set Fire to the Rain"
WHERE book_id = 35;
-- I was wrong
-- instructions just simply say to update patron table' loan_id, book table's availability, and loan table's date_in/date_out. DO NOT OVERTHINK! :D
UPDATE book 
SET available = FALSE 
WHERE book_id = 35;
```



**Wrap-up Query**

Write a query to wrap up the studio. This query should return the names of the patrons with the genre of every book they currently have checked out

```mysql
# Return names of the patrons with the genre of every book they currently have checked out.

-- Retrieve first name, last name, and genre of checked out books.
SELECT patron_loan.first_name, patron_loan.last_name, genres
FROM (
	-- Return first name, last name, and book_id from the patron_loan table.
	SELECT first_name, last_name, book_id
    FROM patron
    -- Merge entries from the loan and patron tables that have the same loan_id.
    INNER JOIN loan ON loan.loan_id = patron.loan_id
) AS patron_loan
-- Merge entries from the patron_loan and genre_book tables that have the same book_id.
INNER JOIN (
	SELECT genres, book_id
    FROM genre
    -- Merge the genre and book table entries.
    INNER JOIN book ON genre.genre_id = book.genre_id
) AS genre_book
ON genre_book.book_id = patron_loan.book_id;

# Alternative approach:

SELECT first_name, last_name, genres
FROM (
	SELECT first_name, last_name, book_id
	FROM patron
	INNER JOIN loan ON loan.loan_id = patron.loan_id
) AS patron_loan
-- Merge entries from patron_loan and checked_out_genres that have mathcing book_id values.
INNER JOIN (
	-- Return entries for books that are NOT available.
	SELECT book_id, genre_id
    FROM book
    WHERE available = FALSE
) AS checked_out_genres ON patron_loan.book_id = checked_out_genres.book_id
-- Merge entries from genre and checked_out_genres that have mathcing genre_id values.
INNER JOIN genre ON checked_out_genres.genre_id = genre.genre_id;

-- Wrap-up query part not my solution, copied from TA
```



**Bonus Mission**

1. Return the counts of the books of each genre. Check out the [documentation](https://dev.mysql.com/doc/refman/8.0/en/counting-rows.html) to see how this could be done!

```mysql
SELECT genre_id, COUNT(*) FROM book GROUP BY genre_id;
```

