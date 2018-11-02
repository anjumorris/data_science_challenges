# Challenge Set 9
## Part II: Baseball Data

*Introductory - Intermediate level SQL*

--

Please complete this exercise via SQLalchemy and Jupyter notebook.

We will be working with the Lahman baseball data we uploaded to your AWS instance in class. 


1. What was the total spent on salaries by each team, each year?

   ```python
   sql_query = '''SELECT franchises.franchid,franchises.franchname,salaries.yearid, SUM(salaries.salary) AS total_spent
                  FROM salaries
                  JOIN franchises ON salaries.teamid = franchises.franchid
                  GROUP BY  franchises.franchid, salaries.teamid, salaries.yearid 
                  ORDER BY franchises.franchid, salaries.yearid 
                  LIMIT 5;'''
   
   pd.read_sql_query(sql_query, cnx)
   ```

2. What is the first and last year played for each player? *Hint:* Create a new table from 'Fielding.csv'.

   ```sql
   CREATE TABLE IF NOT EXISTS fielding (
   	playerID varchar(20) NOT NULL,
   	yearID int NOT NULL,
   	stint int DEFAULT NULL,
   	teamID text NOT NULL,
   	lgID text DEFAULT NULL,
   	POS varchar(20) NOT NULL,
   	G int DEFAULT NULL,
   	GS int DEFAULT NULL,
   	InnOuts int DEFAULT NULL,
   	PO int DEFAULT NULL,
   	A int DEFAULT NULL,
   	E int DEFAULT NULL,
   	DP int DEFAULT NULL,
   	PB int DEFAULT NULL,
   	WP int DEFAULT NULL,
   	SB int DEFAULT NULL,
   	CS int DEFAULT NULL,
   	ZR int DEFAULT NULL,
   	PRIMARY KEY (playerID,yearID,teamID,POS,stint)
   );
   
   ```

   ```sql
   sql_query = '''SELECT playerid, MIN(yearid) AS first_year, MAX(yearid) AS last_year
                  FROM fielding
                  GROUP BY  playerid
                  LIMIT 5;'''
   
   pd.read_sql_query(sql_query, cnx)
   ```

3. Who has played the most all star games?

   aaronha01/ Henry Louis

   ```sql
   # question 3
   sql_query = '''SELECT playerid, COUNT(gameid) AS all_star_games_played
                  FROM allstarfull
                  GROUP BY  playerid
                  ORDER BY all_star_games_played DESC
                  LIMIT 5;'''
   
   pd.read_sql_query(sql_query, cnx)
   ```

4. Which school has generated the most distinct players? *Hint:* Create new table from 'CollegePlaying.csv'.

   There is no 'CollegePlaying.csv' data available in the zip using SchoolsPlayer.csv instead

   ```SQL
   CREATE TABLE IF NOT EXISTS schoolsplayers (
   	playerID varchar(20) NOT NULL,
   	schoolID varchar(20) NOT NULL,
   	yearMin int DEFAULT NULL,
       yearMax int DEFAULT NULL,
   	PRIMARY KEY (playerID,schoolID)
   );
   ```



   ```sql
   sql_query = '''SELECT schoolid, COUNT (DISTINCT playerid) AS num_players
                  FROM schoolsplayers
                  GROUP BY  schoolid
                  ORDER BY num_players DESC
                  LIMIT 5;'''
   
   pd.read_sql_query(sql_query, cnx)
   ```

5. Which players have the longest career? Assume that the `debut` and `finalGame` columns comprise the start and end, respectively, of a player's career. *Hint:* Create a new table from 'Master.csv'. Also note that strings can be converted to dates using the [`DATE`](https://wiki.postgresql.org/wiki/Working_with_Dates_and_Times_in_PostgreSQL#WORKING_with_DATETIME.2C_DATE.2C_and_INTERVAL_VALUES) function and can then be subtracted from each other yielding their difference in days.

   ```sql
   # question 5
   sql_query = '''SELECT playerid, debut, finalgame, finalgame - debut AS career_duration_in_days
                  FROM mast
                  WHERE debut IS NOT NULL AND finalgame is NOT NULL 
                  ORDER BY career_duration_in_days DESC
                  LIMIT 5;'''
   
   pd.read_sql_query(sql_query, cnx)
   ```

6. What is the distribution of debut months? *Hint:* Look at the `DATE` and [`EXTRACT`](https://www.postgresql.org/docs/current/static/functions-datetime.html#FUNCTIONS-DATETIME-EXTRACT) functions.

7. ```sql
   sql_query = '''SELECT EXTRACT(MONTH FROM debut ) AS month, COUNT(playerid) AS number_of_debuts    
                  FROM mast 
                  WHERE debut IS NOT NULL
                  GROUP BY month
                  ORDER BY month
                  LIMIT 20;'''
   
   pd.read_sql_query(sql_query, cnx)
   ```

8. What is the effect of table join order on mean salary for the players listed in the main (master) table? *Hint:* Perform two different queries, one that joins on playerID in the salary table and other that joins on the same column in the master table. You will have to use left joins for each since right joins are not currently supported with SQLalchemy.

   ```sql
   sql_query = '''SELECT salaries.playerid, AVG(salary) AS avg_salary
                  FROM salaries
                  LEFT JOIN mast ON salaries.playerid = mast.playerid
                  GROUP BY salaries.playerid
                  ORDER BY salaries.playerid
                  LIMIT 5;'''
   pd.read_sql_query(sql_query, cnx)
   ```

   ```sql
   sql_query = '''SELECT mast.playerid, AVG(salary) AS avg_salary
                  FROM mast
                  LEFT JOIN salaries ON mast.playerid = salaries.playerid
                  GROUP BY mast.playerid
                  ORDER BY mast.playerid
                  LIMIT 5;'''
   pd.read_sql_query(sql_query, cnx)
   ```

   Using left join  it matters whether we join master on salary or the other way round. As we can see if we join master on salary ther are many NaN values because every playerid who is mentioned in the master may not be on the salaries data. 

