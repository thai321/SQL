# SQL
- **Select Statement**:
  - SELECT
  - DISTINCT
  - WHERE
  - COUNT
  - LIMIT
  - ORDER BY
  - BETWEEN
  - IN
  - LIKE
  - ILIKE

Group
## Select Statement

```sql
SELECT column1, column2, ...
FROM table_name;
```

```sql
SELECT *
From actor;

SELECT first_name, last_name
From actor;

SELECT first_name, last_name, email
FROM customer;
```


### **SELECT DISTINCT** statement
  - In a table, a column may contain many duplicate values; and sometimes you only want to list the different (distinct) values.
  - The **DISTINCT** keyword can be used to return only distinct (different) values.

```sql
SELECT DISTINCT column_1, column_2
FROM table_name;
```

```sql
SELECT DISTINCT release_year
FROM film;

SELECT DISTINCT rental_rate
FROM film;

SELECT DISTINCT rating
FROM film;
```

### Select with **WHERE**
  - What if you want to query just particular rows from a table?
  - In this case, you need to use the **WHERE** clause in the **SELECT** statment
  - The **WHERE** clause appears right after the **FROM** clause of the SELECT statement.
  - The conditions are used to filter the rows returned from the SELECT statement.
  - PostgreSQL provides you with various standard operators to construct the conditions.

```sql
SELECT column_1, column_2 ... column_n
FROM table_name
WHERE conditions;
```

-------

| OPERATOR | DESCRIPTION           |
|----------|-----------------------|
| =        | Equal                 |
| >        | greater than          |
| <        | Less than             |
| >=       | Greater than or equal |
| <=       | Less than or equal    |
| <> or != | Not equal             |
| AND      | Logical operator AND  |
| OR       | Logical operator OR   |


- If you want to get all customers whose first names are jamie, you can use the **WHERE** clause with the equal (=) operator as follows:

```sql
SELECT last_name, first_name
FROM customer
WHERE first_name='Jamie';
```

- If you want to get all customers whose first names are jamie and last names is Rice, you can use the **AND** logical operator that combines two conditions as the following query:

```sql
SELECT last_name, first_name
FROM customer
WHERE first_name='Jamie' AND last_name='Rice';
```

- If you want to know who paid the rental with amount is either less than $1 or greater than $8, you can use the following query with **OR** operator:

```sql
SELECT customer_id, amount, payment_date
FROM payment
WHERE amount <=1 OR amount >= 8;
```


### COUNT Statment

- The **COUNT** function returns the number of input rows that match a specific condition of a query
- The **COUNT(\*)** function erturns the number of rows returned by a select clause.
- When yo apply the **COUNT(\*)** to the entire table, PostgreSQL scans t able sequentially.

```sql
SELECT COUNT(*) FROM table;
```

- You can also sqecify a specific column count for readability

```sql
SELECT COUNT(column) FROM table;
```

--------

- Similar to the **COUNT(\*)** function, the **COUNT(column)** function returns the number of rows returned by a SELECT clause.
- **However, it does not consider NULL values in the column.**

- We can use **COUNT** with **DISTINCT**, for example:

```sql
SELECT COUNT(DISTINCT COLUMN)
FROM table;
```

```sql
SELECT COUNT(*)
FROM payment;
```

```sql
SELECT COUNT(DISTINCT (amount))
FROM payment;
```


### LIMIT

- **LIMIT** allows you to limit the number of rows you get back after a query
- Useful when wanting to get all columns but not all rows
- Goes at the end of a query

```sql
SELECT *
FROM customer
LIMIT 5;
```


### ORDER BY

- When you query data from a table, **PostgreSQL** returns the rows in the order that they were inserted into the table
- In order to sort the result  set, you use the **ORDER BY**  clause in the **SELECT** statement.
- The **ORDER BY** clause allows you to sort the rows returned from the **SELECT** statement in **ascending** or **descending** order based on criteria specified.
- The following illustrates the syntax of the **SELECT** statement:

```sql
SELECT column_1, column_2
FROM table_name
ORDER BY column_1 ASC/DESC;
```


- Specify the column that you want to sort in the **ORDER  BY** clause. If you sort the result set by multiple columns, use a comma to separate between two columns.
- Use **ASC** to sort the result set in ascending order and **DESC** to sort the result set in descending order.
- **If you leave it blank**, the **ORDER BY** clause will use **ASC** by default.

```sql
SELECT first_name, last_name
FROM customer
ORDER BY first_name ASC;
```

------


- If we have some first_name(s) are the same, then it will order by last_name in descending order

```sql
SELECT first_name, last_name
FROM customer
ORDER BY first_name ASC,
last_name DESC;
```

------

- **PostgreSQL** allow you to order a column without selecting that column. Other SQL such as: MySQL or Oracle SQL may not allow you to do this.

```sql
SELECT first_name
FROM customer
ORDER BY last_name;
```

- You should select a column if you want to order by it

```sql
SELECT first_name, last_name
FROM customer
ORDER BY last_name;
```

------

- Get the customer ID numbers for the top 10 highest payment amounts

```sql
SELECT customer_id, amount
FROM payment
ORDER BY amount DESC
LIMIT 10;
```


-------

- Get the titles of the movies with film ids 1-5

```sql
SELECT film_id, title, release_year
FROM film
ORDER BY film_id
LIMIT 5;
```


### BETWEEN Statement

- We use the **BETWEEN** operator to match a value against a range of values. For example:
  - **value BETWEEN** low **AND** high;
- If the value is greater than or equal to the low value and elss than or equal to the high value, the expression returns true, or vice versa.
- We can rewrite the **BETWEEN** operator by using the greater than or equal (>=) or less than or euqal (<=) operators as the following statement:
  - **value** >= **and value** <= high;
- The following expression is equivalent to the expression that uses the **NOT BETWEEN** operator:
  - **value** < low **OR value** > high;

------

```sql
SELECT customer_id, amount
FROM payment
WHERE amount BETWEEN 8 AND 9;
```

```sql
SELECT customer_id, amount
FROM payment
WHERE amount NOT BETWEEN 8 AND 9;
```

```sql
SELECT amount, payment_date
FROM payment
WHERE payment_date BETWEEN '2007-02-07' AND '2007-02-15'
```


### IN Statement

- You use the **IN** operator with the **WHERE** clause to check if a value matches any value in a list of values.
- The syntax of the **IN** operator is as foolows:
  - **value IN** (value1, value2, ...)

- The expression returns true if the value matches any value in the list i.e., value1, value2, etc. The list of the values is not limited to a list of numbers or strings but also a result set of a **SELECT** statement as shown in the following query:
  - **value IN (SELECT value FROM table_name)**
- Just like with **BETWEEN**< you can use **NOT** to adjust an **IN** statement **(NOT IN)**

------

```sql
SELECT customer_id, rental_id, return_date
FROM rental
WHERE customer_id IN(1,2)
ORDER BY return_date DESC;
```


```sql
SELECT customer_id, rental_id, return_date
FROM rental
WHERE customer_id NOT IN(1,2)
ORDER BY return_date DESC;
```

----

```sql
SELECT customer_id, rental_id, return_date
FROM rental
WHERE customer_id IN(7, 13, 10)
ORDER BY return_date DESC;
```

- Same thing as

```sql
SELECT customer_id, rental_id, return_date
FROM rental
WHERE customer_id = 7
OR customer_id = 13
OR customer_id = 10
ORDER BY return_date DESC;
```

------

```sql
SELECT *
FROM payment
WHERE amount IN(7.99, 8.99);
```


### LIKE Statment

- Suppose the store manager asks you find a customer that he does not remember the name exactly.
- He just remembers that customer's first name begins with something like Jen
- How do you find the exact customer that the store manager is asking?

-----

- You may find the customer in the customer table by looking at the first name column to see if there is any value that begins with Jen.
- It is kind of tedious because there many rows in the customer table.

----

- Fortunately, you can use the PostgreSQL **LIKE** operator to as the following query:

```sql
SELECT first_name, last_name
FROM customer
WHERE first_name LIKE 'Jen%';
```

----

- Notice that the **WHERE** clause contains a special expression:
  - The first_name, the **LIKE** operator and a string that contains a percent (%) character, which is referred as a *pattern*.
- **The query returns rows whose values in the first name column begin with Jen and may be followed by any sequence of characters.**
- **This technique is called pattern matching.**


----

- You construct a pattern by combining a string with wildcard characters and use the **LIKE** or **NOT LIKE** operator to find the matches.
  - **Percent(%)** for matching any sequence of characters.
  - **Underscore(_)** for matching any single character.

-----

- Any name ending with the 'y'

```sql
SELECT first_name, last_name
FROM customer
WHERE first_name LIKE '%y';
```

----

- Any name contains 'er' (begin, middle, end)

```sql
SELECT first_name, last_name
FROM customer
WHERE first_name LIKE '%er%';
```

-----

- Any name with a single character and 'her' and follow by any character after

```sql
SELECT first_name, last_name
FROM customer
WHERE first_name LIKE '_her%';
```


----

```sql
SELECT first_name, last_name
FROM customer
WHERE first_name NOT LIKE 'Jen%';
```


-----

- Use **ILIKE** for case in-sensitive

```sql
SELECT first_name, last_name
FROM customer
WHERE first_name ILIKE 'BAR%';
```

---> Barbara, Barry


#### Some practices

- How many payment transactions were greater than $5.00?

```sql
SELECT COUNT(amount)
FROM payment
WHERE amount > 5;
```

-----

- How many actors have a first name that starts with the letter P?

```sql
SELECT COUNT(*)
FROM actor
WHERE first_name LIKE 'P%';
```


-----

- How many unique districts are our customers from?

```sql
SELECT COUNT(DISTINCT (district))
FROM address;
```

- Retrieve the lisit of names for those distinct districts from the previous question

```sql
SELECT DISTINCT(district)
FROM address;
```

---

- How many films have a rating of R and a replacement cost between $5 and $15?

```sql
SELECT COUNT(*)
FROM film
WHERE rating = 'R'
AND replacement_cost BETWEEN 5 AND 15;
```

---

- How many films have the word 'Truman' somewhere in the title?

```sql
SELECT COUNT(*)
FROM film
WHERE title LIKE '%Truman%';
```
