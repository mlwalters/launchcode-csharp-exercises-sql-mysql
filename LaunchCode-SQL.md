SQL Studio

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