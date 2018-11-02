# Challenge Set 9
## Part III: Soccer Data

*Introductory - Intermediate level SQL*

--

Please complete this exercise using sqlite3 and Jupyter notebook.

Download the [SQLite database](https://www.kaggle.com/hugomathien/soccer/downloads/soccer.zip) and load in your notebook using the sqlite3 library. 

1. Which team scored the most points when playing at home? 

   Real Madrid CF

   ```sql
   sql_query = '''SELECT Team.team_api_id,Team.team_long_name, SUM(Match.home_team_goal) AS goals_scored
                  FROM Team
                  JOIN Match ON Team.team_api_id = Match.home_team_api_id
                  GROUP BY  Team.team_api_id,Team.team_long_name
                  ORDER BY goals_scored DESC
                  LIMIT 5;'''
   
   pd.read_sql_query(sql_query, conn).head()
   ```



​    

2. Did this team also score the most points when playing away?  

   No. They did not FC Barcelona had the highest away goals

   ```sql
   sql_query = '''SELECT Team.team_api_id,Team.team_long_name, SUM(Match.away_team_goal) AS goals_away_scored
                  FROM Team
                  JOIN Match ON Team.team_api_id = Match.away_team_api_id
                  GROUP BY  Team.team_api_id,Team.team_long_name
                  ORDER BY goals_away_scored DESC
                  LIMIT 5;'''
   
   pd.read_sql_query(sql_query, conn).head()
   ```

3. How many matches resulted in a tie?  

   6596

   ```sql
   sql_query = '''SELECT COUNT(match_api_id) AS total_number_of_ties
                  FROM Match
                  WHERE home_team_goal=away_team_goal
                  LIMIT 5;'''
   
   pd.read_sql_query(sql_query, conn).head()
   ```


4. How many players have Smith for their last name? How many have 'smith' anywhere in their name?

   Last name smith = 15

   any smith = 18

   ```sql
   sql_query = '''SELECT COUNT(player_api_id) AS number_of_people_with_last_name_smith 
                  FROM Player
                  WHERE player_name LIKE '% smith'
                  LIMIT 5;'''
   
   pd.read_sql_query(sql_query, conn)
   
   sql_query = '''SELECT COUNT(player_api_id) AS number_of_people_with_smith_in_name
                  FROM Player
                  WHERE player_name LIKE '%smith%'
                  LIMIT 5;'''
   
   pd.read_sql_query(sql_query, conn)
   ```

5. What was the median tie score? Use the value determined in the previous question for the number of tie games. *Hint:* PostgreSQL does not have a median function. Instead, think about the steps required to calculate a median and use the [`WITH`](https://www.postgresql.org/docs/8.4/static/queries-with.html) command to store stepwise results as a table and then operate on these results. 

   ```
   
   ```

6. What percentage of players prefer their left or right foot? *Hint:* Calculate either the right or left foot, whichever is easier based on how you setup the problem.

   75% prefer their right foot

   ```sql
   sql_query = '''WITH table1 AS(
                 SELECT  DISTINCT (player_api_id) as playerid,  preferred_foot
                 FROM Player_Attributes
                 GROUP BY   playerid
                 )
                 SELECT (
                100*SUM(CASE WHEN preferred_foot ='right' THEN 1 ELSE 0 END)/COUNT(preferred_foot)) AS PERC_rightfoot
                  FROM table1
   
   
               ;'''
   pd.read_sql_query(sql_query, conn)
   ```


