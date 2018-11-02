# Challenge Set 9
## Part IV: Tennis Data

*Intermediate - Advanced level SQL*

---

### Acquire data ###

Let's get some data and start playing with it!

We'll be using tennis data from [here](https://archive.ics.uci.edu/ml/datasets/Tennis+Major+Tournament+Match+Statistics).


Assuming you are working on your AWS instance, execute the following from the BASH (shell) terminal:

```bash
mkdir -p tennis/data
cd tennis/data
wget http://archive.ics.uci.edu/ml/machine-learning-databases/00300/Tennis-Major-Tournaments-Match-Statistics.zip
```

You may not have `unzip` installed on your AWS instance:

```bash
sudo apt-get install unzip
unzip Tennis-Major-Tournaments-Match-Statistics.zip
```

Before we start using SQL, open up the files with your favorite command line text editor and poke around!


### Prepare the data ###

The data has a mix of missing entries and entries that are the string `NA`. We'll use the command line program `sed` to fix that:

```bash
sed -i.bak s/NA//g AusOpen-women-2013.csv
```

Repeat this for each CSV file in the data set.

### Import the data into PostgreSQL ###

Now we start working in PostgreSQL. The remaining commands are executed from the SQL prompt.

You can create a new database with `CREATE DATABASE tennis;` -- if we use `\d`, we see there are not yet any tables (relations) in the database. So let's create one.

We have to specify the schema of our table, with detail about [data types](http://www.postgresql.org/docs/9.3/static/datatype.html).

```sql
CREATE TABLE  aus_ladies_2013 (
      player1 VARCHAR(255),
      player2 VARCHAR(255),
      round INT,
      result INT,
      fnl1 DOUBLE PRECISION,
      fnl2 DOUBLE PRECISION,
      fsp_1 DOUBLE PRECISION,
      fsw_1 DOUBLE PRECISION,
      ssp_1 DOUBLE PRECISION,
      ssw_1 DOUBLE PRECISION,
      ace_1 INT,
      dbf_1 INT,
      wnr_1 INT,
      ufe_1 INT,
      bpc_1 INT,
      bpw_1 INT,
      npa_1 INT,
      npw_1 INT,
      tpw_1 INT,
      st1_1 INT,
      st2_1 INT,
      st3_1 INT,
      st4_1 INT,
      st5_1 INT,
      fsp_2 DOUBLE PRECISION,
      fsw_2 DOUBLE PRECISION,
      ssp_2 DOUBLE PRECISION,
      ssw_2 DOUBLE PRECISION,
      ace_2 INT,
      dbf_2 INT,
      wnr_2 INT,
      ufe_2 INT,
      bpc_2 INT,
      bpw_2 INT,
      npa_2 INT,
      npw_2 INT,
      tpw_2 INT,
      st1_2 INT,
      st2_2 INT,
      st3_2 INT,
      st4_2 INT,
      st5_2 INT);
```

Now load data from a CSV file into this table:

```sql
COPY 
      aus_ladies_2013
FROM 
      '/home/anjali/tennis/data/AusOpen-women-2013.csv'
DELIMITER 
      ','
CSV HEADER;
```

Repeat this process for all of the other tables. *Hint:* You can make a new table with the same schema as an existing table. For example:

```sql
CREATE TABLE 
      aus_men_2013 
(LIKE 
      aus_ladies_2013);
```

```
AusOpen-men-2013.csv        Tennis-Major-Tournaments-Match-Statistics.zip
AusOpen-women-2013.csv      USOpen-men-2013.csv
AusOpen-women-2013.csv.bak  USOpen-women-2013.csv
FrenchOpen-men-2013.csv     Wimbledon-men-2013.csv
FrenchOpen-women-2013.csv   Wimbledon-women-2013.csv
```

```
COPY 
      french_women_2013
FROM 
      '/home/anjali/tennis/data/FrenchOpen-women-2013.csv'
DELIMITER 
      ','
CSV HEADER;
```

```
sed -i.bak s/NA//g AusOpen-men-2013.csv
```





Extension: Can you make the tennis data tidy?


### Some practice SQL queries ###

The following SQL commands can be used to explore the data. To maximize understanding of the syntax, type these rather than copying and pasting.

```sql
SELECT 
      player1, player2, result 
FROM 
      us_men_2013 
LIMIT 5;


SELECT
      player1, result 
FROM 
      us_men_2013 
WHERE 
      player1 = 'Richard Gasquet';


SELECT 
      player1, player2, result
FROM
      us_men_2013 
WHERE 
      player1 = 'Richard Gasquet';


SELECT
      player1, player2, result 
FROM 
      us_men_2013 
WHERE 
      player1 = 'Richard Gasquet' OR player2 = 'Richard Gasquet';


SELECT 
      COUNT(*)
FROM
      us_men_2013;


SELECT
      player1, COUNT(*) 
FROM 
      us_men_2013 
GROUP BY 
      player1;


SELECT 
      player1, AVG(result) 
FROM 
      us_men_2013 
GROUP BY
      player1;


SELECT 
      player1, player2, result 
FROM 
      us_men_2013 
WHERE 
      result = 1 
LIMIT 5;


SELECT 
      COUNT(*) 
FROM 
      us_men_2013 
WHERE 
      result = 1;


SELECT 
      player1, player2, result 
FROM 
      french_men_2013 
WHERE 
      result = 0 
LIMIT 5;


SELECT 
      us_men_2013.player1, us_men_2013.tpw_1, french_men_2013.tpw_1 
FROM 
      us_men_2013, french_men_2013 
WHERE 
      us_men_2013.player1 = french_men_2013.player1;


SELECT 
      us_men_2013.player1, 
      SUM(us_men_2013.tpw_1) AS us_points, 
      SUM(french_men_2013.tpw_1) AS french_points
FROM 
      us_men_2013, french_men_2013 
WHERE 
      us_men_2013.player1 = french_men_2013.player1 
GROUP BY 
      us_men_2013.player1;


SELECT
      us_men_2013.player1, 
      SUM(us_men_2013.tpw_1) AS us_points, 
      SUM(french_men_2013.tpw_1) AS french_points, 
      SUM(us_men_2013.tpw_1 + french_men_2013.tpw_1) AS total_points 
FROM
      us_men_2013, french_men_2013 
WHERE 
      us_men_2013.player1 = french_men_2013.player1
GROUP BY
      us_men_2013.player1;
```


## The challenges!

This challenge uses only SQL queries. Please submit answers in a markdown file.

```sql
sql_command = '''
CREATE TABLE final AS

SELECT  player1 AS name,
      'M' AS gender,
      'US' AS tournament,
      result AS win,
      FSP_1 AS fsp,
      DBF_1 AS dbf,
      UFE_1 AS ufe
FROM    us_men_2013

UNION ALL

SELECT  player2 AS name,
      'M' AS gender,
      'US' AS tournament,
      1-result AS win,
      FSP_2 AS fsp,
      DBF_2 AS dbf,
      UFE_2 AS ufe
FROM    us_men_2013

UNION ALL

SELECT  player1 AS name,
      'M' AS gender,
      'AUS' AS tournament,
      result AS win,
      FSP_1 AS fsp,
      DBF_1 AS dbf,
      UFE_1 AS ufe
FROM    aus_men_2013

UNION ALL

SELECT  player2 AS name,
      'M' AS gender,
      'AUS' AS tournament,
      1-result AS win,
      FSP_2 AS fsp,
      DBF_2 AS dbf,
      UFE_2 AS ufe
FROM    aus_men_2013

UNION ALL

SELECT  player1 AS name,
      'M' AS gender,
      'French' AS tournament,
      result AS win,
      FSP_1 AS fsp,
      DBF_1 AS dbf,
      UFE_1 AS ufe
FROM    french_men_2013

UNION ALL

SELECT  player2 AS name,
      'M' AS gender,
      'French' AS tournament,
      1-result AS win,
      FSP_2 AS fsp,
      DBF_2 AS dbf,
      UFE_2 AS ufe
FROM    french_men_2013

UNION ALL

SELECT  player1 AS name,
      'M' AS gender,
      'wimbledon' AS tournament,
      result AS win,
      FSP_1 AS fsp,
      DBF_1 AS dbf,
      UFE_1 AS ufe
FROM    wim_men_2013

UNION ALL

SELECT  player2 AS name,
      'M' AS gender,
      'wimbledon' AS tournament,
      1-result AS win,
      FSP_2 AS fsp,
      DBF_2 AS dbf,
      UFE_2 AS ufe
FROM    wim_men_2013

UNION ALL

SELECT  player1 AS name,
      'F' AS gender,
      'wimbledon' AS tournament,
      result AS win,
      FSP_1 AS fsp,
      DBF_1 AS dbf,
      UFE_1 AS ufe
FROM    wim_women_2013

UNION ALL

SELECT  player2 AS name,
      'F' AS gender,
      'wimbledon' AS tournament,
      1-result AS win,
      FSP_2 AS fsp,
      DBF_2 AS dbf,
      UFE_2 AS ufe
FROM    wim_women_2013

UNION ALL

SELECT  player1 AS name,
      'F' AS gender,
      'French' AS tournament,
      result AS win,
      FSP_1 AS fsp,
      DBF_1 AS dbf,
      UFE_1 AS ufe
FROM    french_women_2013

UNION ALL

SELECT  player2 AS name,
      'F' AS gender,
      'French' AS tournament,
      1-result AS win,
      FSP_2 AS fsp,
      DBF_2 AS dbf,
      UFE_2 AS ufe
FROM    french_women_2013

UNION ALL

SELECT  player1 AS name,
      'F' AS gender,
      'AUS' AS tournament,
      result AS win,
      FSP_1 AS fsp,
      DBF_1 AS dbf,
      UFE_1 AS ufe
FROM    aus_ladies_2013

UNION ALL

SELECT  player2 AS name,
      'F' AS gender,
      'AUS' AS tournament,
      1-result AS win,
      FSP_2 AS fsp,
      DBF_2 AS dbf,
      UFE_2 AS ufe
FROM    aus_ladies_2013

UNION ALL

SELECT  player1 AS name,
      'F' AS gender,
      'US' AS tournament,
      result AS win,
      FSP_1 AS fsp,
      DBF_1 AS dbf,
      UFE_1 AS ufe
FROM    us_women_2013

UNION ALL

SELECT  player2 AS name,
      'F' AS gender,
      'US' AS tournament,
      1-result AS win,
      FSP_2 AS fsp,
      DBF_2 AS dbf,
      UFE_2 AS ufe
FROM    us_women_2013;
          '''
cnx.execute(sql_command)
```



1. Using the same tennis data, find the number of matches played by
   each player in each tournament. (Remember that a player can be
   present as both player1 or player2).

   ```sql
   sql_query = '''SELECT name,tournament, COUNT(name) AS matches_played
                  FROM final
                  GROUP BY  name,tournament
                  ORDER BY name ASC
                  LIMIT 10;'''
   
   pd.read_sql_query(sql_query, cnx)
   ```

2. Who has played the most matches total in all of US Open, AUST Open, 
   French Open? Answer this both for men and women.

   Rafeal Nadal and Serena Williams

   ```sql
   # question 2
   sql_query = '''SELECT name,gender, COUNT(name) AS matches_played
                  FROM final
                  GROUP BY  name,gender
                  ORDER BY matches_played  DESC
                  LIMIT 10;'''
   
   pd.read_sql_query(sql_query, cnx)
   ```

3. Who has the highest first serve percentage? (Just the maximum value
   in a single match.)

   S Errani

   ```sql
   sql_query = '''SELECT name, MAX(fsp) AS highest_fsp
                  FROM final
                  GROUP BY  name
                  ORDER BY highest_fsp  DESC
                  LIMIT 10;'''
   
   pd.read_sql_query(sql_query, cnx)
   ```

4. What are the unforced error percentages of the top three players
   with the most wins? (Unforced error percentage is % of points lost
   due to unforced errors. In a match, you have fields for number of
   points won by each player, and number of unforced errors for each
   field.)

   0%

   ```sql
   # question 4
   sql_query = '''WITH top_players AS (SELECT name, SUM(win) AS total_wins
                               FROM final
                               GROUP BY name 
                               ORDER BY total_wins  DESC
                               LIMIT 3)
   
                   SELECT name, SUM(ufe*100)/SUM(ufe + dbf) AS percentage
                   FROM final
                   WHERE name IN (SELECT name FROM top_players)
                   GROUP BY name
                  ;'''
   
   pd.read_sql_query(sql_query, cnx)
   ```


*Hint:* `SUM(double_faults)` sums the contents of an entire column. For each row, to add the field values from two columns, the syntax `SELECT name, double_faults + unforced_errors` can be used.


*Special bonus hint:* To be careful about handling possible ties, consider using [rank functions](http://www.sql-tutorial.ru/en/book_rank_dense_rank_functions.html).
