# ShrikantBalakrishnanTas4-PostgreSQL-
Task 4 Project: Movie Rental Analysis System (using Redshift or PostgreSQL) Objective: Perform advanced analysis on movie rental data using OLAP operations.

The project will include the following tasks:
Database Creation:
Create a database named MovieRental.
Create table rental_data with columns:
MOVIE_ID (integer),
CUSTOMER_ID (integer),
GENRE (varchar),
RENTAL_DATE (date),
RETURN_DATE (date),
RENTAL_FEE (numeric).
Data Creation:
Insert 10–15 sample rental records.
OLAP Operations:
a) Drill Down: Analyze rentals from genre to individual movie level.
b) Rollup: Summarize total rental fees by genre and then overall.
c) Cube: Analyze total rental fees across combinations of genre, rental date, and
customer.
d) Slice: Extract rentals only from the ‘Action’ genre.
e) Dice: Extract rentals where GENRE = 'Action' or 'Drama' and RENTAL_DATE is in
the last 3 months.




-- Table: public.rental_data

-- DROP TABLE IF EXISTS public.rental_data;

CREATE TABLE IF NOT EXISTS public.rental_data			-- create table rental_data
(
)

TABLESPACE pg_default;

ALTER TABLE IF EXISTS public.rental_data
    OWNER to postgres;

-- SCHEMA: public

-- DROP SCHEMA IF EXISTS public ;

CREATE SCHEMA IF NOT EXISTS public
    AUTHORIZATION pg_database_owner;

COMMENT ON SCHEMA public
    IS 'standard public schema';

GRANT USAGE ON SCHEMA public TO PUBLIC;

GRANT ALL ON SCHEMA public TO pg_database_owner;

-- Table: public.rental_data

DROP TABLE IF EXISTS public.rental_data;

CREATE TABLE IF NOT EXISTS public.rental_data 					-- create table rental_data
(
    "MOVIE_ID" integer,
    "CUSTOMER_ID" integer,
    "GENRE" character varying COLLATE pg_catalog."default",
    "RENTAL_DATE" date,
    "RETURN_DATE" date,
    "RENTAL_FEE" numeric
)

TABLESPACE pg_default;

ALTER TABLE IF EXISTS public.rental_data
    OWNER to postgres;

ALTER TABLE IF EXISTS public.rental_data
    OWNER to postgres;

-- Insert values into table
INSERT INTO public.rental_data(
	"MOVIE_ID", "CUSTOMER_ID", "GENRE", "RENTAL_DATE", "RETURN_DATE", "RENTAL_FEE")
	VALUES
	(1,6,'Drama','2025-11-15','2026-01-02',450),
    (2,6,'Childrens Classic','2025-02-01','2025-05-04',510),
    (3,9,'Thriller','2025-04-03','2025-05-06',395),
    (4,9,'Young Adult','2025-06-05','2025-07-08',666),
    (5,8,'Historical Fiction','2025-08-07','2025-10-10',729),
    (6,3,'Romance','2025-09-09','2025-11-11',424),
    (7,1,'Fantasy','2025-07-12','2025-08-13',565),
    (8,5,'Fantasy','2025-03-14','2025-04-15',582),
    (9,7,'Romance','2025-05-16','2025-12-17',493),
    (10,4,'Drama','2025-01-18','2025-04-19',577),
    (11,4,'Thriller','2025-01-20','2025-07-20',519),
    (12,3,'Young Adult','2025-06-21','2025-10-22',486),
    (13,8,'Drama','2025-09-23','2025-11-24',514),
    (14,2,'Romance','2025-08-25','2025-09-26',490),
    (15,7,'Thriller','2025-04-27','2025-05-28',549),
    (16,1,'Romance','2025-02-28','2025-10-30',533),
    (17,9,'Historical Fiction','2025-03-31','2025-07-29',475),
    (18,2,'Fantasy','2025-05-30','2025-07-27',388),
    (19,5,'Thriller','2025-07-28','2025-12-01',503),
    (20,9,'Romance','2025-07-26','2025-11-03',428);


SELECT * FROM public.rental_data; 			-- Display Table rental_data 
-- -------------------------------------------------------
-- OLAP Operations:
-- a) Drill Down: Analyze rentals from genre to individual movie level
SELECT "GENRE", "MOVIE_ID"
    FROM rental_data
    GROUP BY "GENRE","MOVIE_ID";
-- --------------------------------------------------------------

-- b) Rollup: Summarize total rental fees by genre and then overall.
SELECT "GENRE", "MOVIE_ID",
    COUNT("RENTAL_DATE") AS Total_Rentals,
    SUM("RENTAL_FEE") AS Total_Revenue
FROM
    rental_data
GROUP BY
    ROLLUP ("GENRE", "MOVIE_ID")
ORDER BY
    "GENRE", "MOVIE_ID";
-- -----------------------------------------------
-- c) Cube: Analyze total rental fees across combinations of genre, rental date, and customer. 
SELECT "GENRE","CUSTOMER_ID", "RENTAL_DATE", 
    COUNT("RENTAL_DATE") AS Total_Rental,
    SUM("RENTAL_FEE") AS Total_Revenue
FROM
    rental_data
GROUP BY
    CUBE ("GENRE", "CUSTOMER_ID", "RENTAL_DATE");
-- ------------------------------------------------------
-- --------------------------
-- d) Slice: Extract rentals only from the ‘Action’ genre.

SELECT "GENRE",
    COUNT("RENTAL_DATE") AS Total_Rental,
    SUM("RENTAL_FEE") AS Total_Revenue
FROM rental_data
WHERE "GENRE" = 'Action'
Group By "GENRE";
-- --------------------------------

-- e) Dice: Extract rentals where GENRE = 'Action' or 'Drama' and RENTAL_DATE is in
-- the last 3 months.

SELECT "MOVIE_ID", "CUSTOMER_ID", "GENRE", "RENTAL_DATE", "RETURN_DATE", "RENTAL_FEE"
FROM
    rental_data
WHERE
    "GENRE" IN ('Action', 'Drama')
    AND "RENTAL_DATE" >= CURRENT_DATE - INTERVAL '3 months';

