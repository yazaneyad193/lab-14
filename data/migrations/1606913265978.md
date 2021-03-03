# Migrations of the database:

<!-- create a copy of the lab14 database:  lab14_normal-->
`CREATE DATABASE lab14_normal WITH TEMPLATE lab14;`

<!-- create a second table in the lab14_normal database named authors. -->
`CREATE TABLE AUTHORS (id SERIAL PRIMARY KEY, name VARCHAR(255));`

<!--  retrieve unique author values from the books table and insert each one into the authors table in the name column. -->
`INSERT INTO authors(name) SELECT DISTINCT author FROM books;`

<!-- add a column to the books table named author_id. -->
`ALTER TABLE books ADD COLUMN author_id INT;`

<!-- running a subquery for every row in the books table. The subquery finds the author row that has a name matching the current book’s author value. The id of that author row is then set as the value of the author_id property in the current book row. -->
`UPDATE books SET author_id=author.id FROM (SELECT * FROM authors) AS author WHERE books.author = author.name;`

<!-- modify the books table by removing the column named author. -->
`ALTER TABLE books DROP COLUMN author;`

<!--  modify the data type of the author_id in the books table, setting it as a foreign key which references the primary key in the authors table. Now PostgreSQL knows HOW these 2 tables are connected. -->
`ALTER TABLE books ADD CONSTRAINT fk_authors FOREIGN KEY (author_id) REFERENCES authors(id);`
