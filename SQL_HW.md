## Homework Assignment

* 1a. Display the first and last names of all actors from the table `actor`.
                    select  first_name, last_name from actor;

* 1b. Display the first and last name of each actor in a single column in upper case letters. Name the column `Actor Name`. 
                    ALTER TABLE actor
                    ADD actor_name VARCHAR(1000);
                    SELECT CONCAT(first_name, last_name) AS ConcatenatedString;
                    
* 2a. You need to find the ID number, first name, and last name of an actor, of whom you know only the first name, "Joe." What is one query would you use to obtain this information?
                    select actor_id, first_name, last_name
                    from actor
                    where first_name = "joe" and last_name IS NULL;

* 2b. Find all actors whose last name contain the letters `GEN`:
                    select actor_id, first_name, last_name
                    from actor
                    where last_name  LIKE '%gen%';
* 2c. Find all actors whose last names contain the letters `LI`. This time, order the rows by last name and first name, in that order:
                    select actor_id, first_name, last_name
                    from actor
                    where last_name  LIKE '%lt%'
                    ORDER BY last_name DESC, first_name;
                    
* 2d. Using `IN`, display the `country_id` and `country` columns of the following countries: Afghanistan, Bangladesh, and China:
                    select country_id, country from country
                    WHERE country IN ('Afghanistan', 'Bangladesh', 'China');
* 3a. Add a `middle_name` column to the table `actor`. Position it between `first_name` and `last_name`. Hint: you will need to specify the data type.
                    ALTER TABLE actor
                    ADD middle_name VARCHAR(1000)
                    AFTER first_name;
* 3b. You realize that some of these actors have tremendously long last names. Change the data type of the `middle_name` column to `blobs`.
                    ALTER TABLE actor
                    ALTER COLUMN middle_name varbinary;
* 3c. Now delete the `middle_name` column.
                    ALTER TABLE actor DROP middle_name;
* 4a. List the last names of actors, as well as how many actors have that last name.
                    SELECT COUNT(last_name)
                    FROM actor
                    group by last_name;
* 4b. List last names of actors and the number of actors who have that last name, but only for names that are shared by at least two actors
                    SELECT COUNT(last_name) as p
                    FROM actor
                    group by last_name
                    where p > 1;
* 4c. Oh, no! The actor `HARPO WILLIAMS` was accidentally entered in the `actor` table as `GROUCHO WILLIAMS`, the name of Harpo's second cousin's husband's yoga teacher. Write a query to fix the record.
                    UPDATE actor
                    SET first_name = 'Harpo', actor_name = 'Harpo Williams'
                    WHERE actor_name = 'Groucho WIlliams';
* 4d. Perhaps we were too hasty in changing `GROUCHO` to `HARPO`. It turns out that `GROUCHO` was the correct name after all! In a single query, if the first name of the actor is currently `HARPO`, change it to `GROUCHO`. Otherwise, change the first name to `MUCHO GROUCHO`, as that is exactly what the actor will be with the grievous error. BE CAREFUL NOT TO CHANGE THE FIRST NAME OF EVERY ACTOR TO `MUCHO GROUCHO`, HOWEVER! (Hint: update the record using a unique identifier.)
                    UPDATE actor
                    SET first_name = 'Groucho', actor_name = 'Groucho Williams'
                    WHERE actor_name = 'Harpo WIlliams';
* 5a. You cannot locate the schema of the `address` table. Which query would you use to re-create it?
                    CREATE TABLE Address
                    INSERT INTO Address
                    SELECT address_id, address, address2, district, city_id, postal_code, phone, last_update
                    FROM store, customer, city, ;

* 6a. Use `JOIN` to display the first and last names, as well as the address, of each staff member. Use the tables `staff` and `address`:
                    select first_name, last_name, address
                    from staff
                    join address on staff.address_id = address.address_id;
* 6b. Use `JOIN` to display the total amount rung up by each staff member in August of 2005. Use tables `staff` and `payment`. 
                    select first_name, last_name, SUM(amount)
                    from staff
                    join payment on staff.staff_id = payment.staff_id
                    group by payment.staff_id;
* 6c. List each film and the number of actors who are listed for that film. Use tables `film_actor` and `film`. Use inner join.
                    select title, count(actor_id)
                    from film
                    INNER join film_actor on film.film_id = film_actor.film_id
                    group by title;
* 6d. How many copies of the film `Hunchback Impossible` exist in the inventory system?
                    select title
                    from film
                    join inventory on film.film_id = inventory.film_id
                    where title = 'Hunchback Impossible';
* 6e. Using the tables `payment` and `customer` and the `JOIN` command, list the total paid by each customer. List the customers alphabetically by last name:
                    select last_name, sum(amount)
                    from payment
                    join customer on payment.customer_id = customer.customer_id
                    group by last_name DESC;
  ```
  	![Total amount paid](Images/total_payment.png)
  ```

* 7a. The music of Queen and Kris Kristofferson have seen an unlikely resurgence. As an unintended consequence, films starting with the letters `K` and `Q` have also soared in popularity. Use subqueries to display the titles of movies starting with the letters `K` and `Q` whose language is English. 
                    SELECT title
                    FROM (
                    SELECT name
                    FROM film.language
                    WHERE title LIKE 'k%' or 'q%'  and language = 'english')
* 7b. Use subqueries to display all actors who appear in the film `Alone Trip`.
                    SELECT actor_name
                    FROM (
                    SELECT *
                    FROM actor.film
                    WHERE title  = 'Alone Trip')
* 7c. You want to run an email marketing campaign in Canada, for which you will need the names and email addresses of all Canadian customers. Use joins to retrieve this information.
                    select first_name, last_name, email
                    from customer
                    join address on customer.address_id = address.address_id
                    join city on address.city_id = city.city_id
                    join country on city.country_id = country.country_id
                    where country = "Canada";
* 7d. Sales have been lagging among young families, and you wish to target all family movies for a promotion. Identify all movies categorized as famiy films.
                    select title, category.name
                    from film
                    join film_category on film.film_id = film_category.film_id
                    join category on film_category.category_id = category.category_id
                    where category.name = 'family';

* 7e. Display the most frequently rented movies in descending order.
                    select title, count(title) as frequency
                    from film
                    join inventory on film.film_id = inventory.film_id
                    join rental on inventory.inventory_id = rental.inventory_id
                    group by title
                    Order by title DESC;
* 7f. Write a query to display how much business, in dollars, each store brought in.
                    select store.store_id as id, sum(amount)
                    from store
                    join customer on store.store_id = customer.store_id
                    join payment on customer.customer_id = payment.customer_id
                    group by id;
* 7g. Write a query to display for each store its store ID, city, and country.
                    select store.store_id as id, city, country
                    from store
                    join customer on store.store_id = customer.store_id
                    join address on customer.address_id = address.address_id
                    join city on address.city_id = city.city_id
                    join country on city.country_id = country.country_id;

* 7h. List the top five genres in gross revenue in descending order. (**Hint**: you may need to use the following tables: category, film_category, inventory, payment, and rental.)
                    select category.name as genre, sum(amount) as revenue
                    from payment
                    join rental on payment.rental_id = rental.rental_id
                    join inventory on rental.inventory_id = inventory.inventory_id
                    join film_category on inventory.film_id = film_category.film_id
                    join category on film_category.category_id = category.category_id
                    group by genre
                    order by revenue DESC
                    LIMIT 5;
* 8a. In your new role as an executive, you would like to have an easy way of viewing the Top five genres by gross revenue. Use the solution from the problem above to create a view. If you haven't solved 7h, you can substitute another query to create a view.
                    CREATE VIEW `top5` AS
                    select category.name as genre, sum(amount) as revenue
                    from payment
                    join rental on payment.rental_id = rental.rental_id
                    join inventory on rental.inventory_id = inventory.inventory_id
                    join film_category on inventory.film_id = film_category.film_id
                    join category on film_category.category_id = category.category_id
                    group by genre
                    order by revenue DESC
                    LIMIT 5;
* 8b. How would you display the view that you created in 8a?
                    I would export the data into a csv and create a bar graph using matplotlib
* 8c. You find that you no longer need the view `top_five_genres`. Write a query to delete it.
                    DROP VIEW top5;
                    
                    
### Appendix: List of Tables in the Sakila DB

* A schema is also available as `sakila_schema.svg`. Open it with a browser to view.

```sql
	'actor'
	'actor_info'
	'address'
	'category'
	'city'
	'country'
	'customer'
	'customer_list'
	'film'
	'film_actor'
	'film_category'
	'film_list'
	'film_text'
	'inventory'
	'language'
	'nicer_but_slower_film_list'
	'payment'
	'rental'
	'sales_by_film_category'
	'sales_by_store'
	'staff'
	'staff_list'
	'store'
```
