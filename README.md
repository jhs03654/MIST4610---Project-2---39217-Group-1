# MIST4610 - Project 2 - 39217 Group 1 

## Team Name
39217 Group 1

## Team Members
1. Tejas Lingamaneni
2. Megan Chen
3. Dillan Mangabhai
4. Ceci Motley
5. Jeb Salter

## Problem Description
The purpose of this project is to design and implement a relational database that models the operations of a fictional college football league known as the United Collegiate Conference (UCC). The UCC serves as a mock athletic organization created for the purposes of this project, representing a structured system of teams, players, coaches, and games competing across multiple divisions and seasons.

The central focus of this database is to accurately capture the dynamic nature of collegiate football — including how players, coaches, and teams evolve over time. Each team within the UCC belongs to a division and competes in a structured seasonal schedule of games held at stadiums across the country. The database stores key information such as player rosters, coaching assignments, team affiliations, and player performance statistics, allowing for detailed tracking and analysis of football operations across multiple seasons.

Our goal in developing this database is not only to model these relationships with logical accuracy and referential integrity, but also to make the data functional for meaningful analysis. Once populated, the database will allow for a variety of SQL queries to be performed, such as evaluating team and player performance over time, comparing statistics between divisions, or examining changes in coaching staff across seasons.

Although the United Collegiate Conference is entirely fictional, the entities, relationships, and constraints within this database are designed to closely mirror those found in real NCAA football programs. The project emphasizes realistic data organization, relational consistency, and analytical usability — serving as both a simulation of college football operations and a foundation for advanced database querying and reporting.

## Data Model 
Our model is based on the structure of a collegiate football database designed to manage teams, players, coaches, and games across multiple seasons. The overall goal of this model is to track all essential components of a football program while maintaining historical accuracy from season to season. The entities within this database interact to represent how teams, coaches, and players relate to one another over time.

At the top level of the model, the Conference and Division entities organize teams by league hierarchy. Each conference is composed of many divisions, and each division contains multiple teams, which is represented by the one-to-many relationship between Conference → Division and Division → Team. The Team entity holds information such as team name, school name, city, state, stadium, and division membership. Because each team belongs to one division but may play in many games and seasons, Team serves as a key relational hub in the model.

The Coach and Coach_Assignment tables work together to show how coaches are assigned to specific teams and seasons. The Coach table holds static information about each coach, including their name and specialty. The Coach_Assignment table captures dynamic relationships — when a coach is employed by a specific team during a particular season and in what role (e.g., Head Coach, Offensive Coordinator, Defensive Coordinator). This design allows the same coach to appear in multiple seasons or change teams without duplicating personal information.

Within Coach_Assignment, a recursive one-to-many relationship has been created to show the coaching hierarchy within each team and season. Each Offensive Coordinator, Defensive Coordinator, and Assistant Coach contains a head_coach_id field that references the Head Coach of the same team and season. This relationship is labeled “Head Coach” and represents which head coach supervises each subordinate coach. This recursive relationship provides organizational structure and enables advanced reporting, such as identifying staff under each head coach.

Players and their participation across teams and seasons are modeled through the Player, Roster, and Player_Game_Stats entities. The Player table contains biographical and physical information for each athlete, including position and hometown. However, because players may transfer teams, redshirt, or leave mid-season, their connection to a team cannot be stored solely within the Player or Team tables. To address this, the Roster table acts as an associative entity that connects Player, Team, and Season.

A unique idroster identifier was added as the primary key for this table, rather than using the combination of Season ID and Team ID. This decision allows for greater flexibility, ensuring that players who transfer or leave mid-season can still be tracked accurately. For example, if a player begins the season on one team and later joins another, both roster entries can coexist without violating primary key constraints. This approach preserves historical integrity and prevents data conflicts when a player’s affiliation changes during the same season.

Each player’s performance is captured through the Player_Game_Stats table, which records detailed statistics for every game a player participates in. This table includes metrics such as passing yards, rushing attempts, tackles, and touchdowns. It is connected to both the Player table and the Game table, representing a many-to-one relationship from Player_Game_Stats to both Player and Game.

The Game entity records each football matchup, including the home and away teams, final scores, attendance, and stadium location. Because each game occurs within a specific season, there is a one-to-many relationship between Season and Game. Additionally, both Home Team and Away Team are foreign keys referencing the same Team table, allowing each matchup to be represented accurately while maintaining referential integrity.

The Season table defines each football year and is referenced by multiple entities including Game, Roster, and Coach_Assignment, ensuring that all seasonal data is properly grouped. The Stadium entity stores information about the venues in which games are played, including capacity and location. Each stadium can host many teams and games, establishing a one-to-many relationship with both Team and Game.

Lastly, the Position entity categorizes players by role (such as Quarterback, Linebacker, or Wide Receiver). Each player is assigned exactly one position, creating a one-to-many relationship between Position and Player.

<img width="1700" height="1359" alt="united conference" src="https://github.com/user-attachments/assets/fd39dc77-be6e-46a2-ae66-9d96d01d4e31" />

## Data Dictionary 
<img width="6375" height="8250" alt="1" src="https://github.com/user-attachments/assets/d3bdc879-f460-4166-8303-c0d48c9d3766" />
<img width="6375" height="8250" alt="2" src="https://github.com/user-attachments/assets/8f7ddba9-f8ee-40b0-a1b8-bef325719d5e" />
<img width="6375" height="8250" alt="3" src="https://github.com/user-attachments/assets/e7eba482-40eb-46af-a90a-839817fc27ad" />
<img width="6375" height="8250" alt="4" src="https://github.com/user-attachments/assets/b462e30a-4d83-435e-94d4-b6b92ac012d9" />
<img width="6375" height="8250" alt="5" src="https://github.com/user-attachments/assets/48b91531-41e6-443e-aaae-f2ee74e5535e" />

## Queries 
<img width="567" height="308" alt="Features" src="https://github.com/user-attachments/assets/693624d1-e3e8-4f9e-9b29-e5602b1c4d3e" />

#1. This query retrieves the top ten offensive players ranked by total receiving yards in a given season. It combines data from multiple tables — player_game_stats, player, roster, team, and season — to display each player’s first and last name, team, season year, total receptions, total receiving yards, total receiving touchdowns, and average receiving yards per game. Aggregate functions (SUM and AVG) are used to calculate season totals and averages, and the results are grouped by player, team, and season. The output is ordered in descending order by total receiving yards to identify the most productive wide receivers in the league.

<img width="890" height="536" alt="Query1" src="https://github.com/user-attachments/assets/b5f4b3fe-e2a0-4e08-8456-8af13e9868c2" />

This query provides coaches, analysts, and scouts with a quick snapshot of the top-performing offensive players in the conference. By viewing the leaders in receiving yards and touchdowns, staff can evaluate player consistency and overall offensive contribution. Additionally, the calculated average yards per game helps identify which players are most efficient week-to-week — valuable for making lineup, recruiting, or award-related decisions.

#2. This query retrieves every team in the 2023 season along with the first and last names of their respective head coaches. It joins the team, coach_assignment, coach, and season tables to connect each team to its assigned coaching staff for a specific season. The REGEXP operator filters the results to only include coaches whose role matches “Head Coach,” and the WHERE condition restricts the data to the 2023 season. The final output lists each team’s name alongside its head coach, providing a clear snapshot of team leadership for that year.

<img width="846" height="444" alt="Query2" src="https://github.com/user-attachments/assets/46887834-40e9-432a-8cf4-2e9969bf3053" />

This query gives administrators, analysts, and fans an organized overview of all teams and their head coaches during the 2023 season. It’s useful for verifying staff assignments, preparing official conference materials, and analyzing leadership continuity across seasons. This type of query could also help identify trends in coaching performance by comparing year-over-year changes in head coaches for each team.

#3. This query displays all teams from the 2024 season along with their average number of points scored during home games. It joins the team, season, and game tables to link each team to its home game data. The LEFT JOIN ensures that even teams that have not hosted a home game in 2024 are still included in the results, with their average score displayed as NULL. The AVG() function calculates the average home score for each team, rounded to two decimal places, and the results are grouped by team name. Finally, the teams are ordered in ascending order based on their average home points.

<img width="965" height="537" alt="Query3" src="https://github.com/user-attachments/assets/ef514e30-2149-4400-b16f-e5871f866f45" />

This query allows analysts and conference officials to assess team performance at home during the 2024 season. By displaying the average points scored, it becomes easier to identify which teams have strong offensive showings at home versus those that struggle. Including teams without home games ensures data completeness and provides a full view of all conference members. This information could inform discussions around scheduling balance, offensive strategy, and overall competitiveness.

#4 This query displays every Assistant Coach, Offensive Coordinator, and Defensive Coordinator along with their corresponding Head Coach, team, and season. The query uses a self-join on the coach_assignment table to relate subordinate coaches (child) to their supervising Head Coach (parent) through the head_coach_id field. It also connects to the coach, team, and season tables to retrieve coach names, team names, and season years. Each row represents a staff member’s role under their head coach for a specific team and season, ordered by season year, team name, and role.

<img width="865" height="860" alt="Query4" src="https://github.com/user-attachments/assets/936e4d95-2cfd-4e57-9700-8d066ac5f525" />

This query provides a clear and organized view of the coaching hierarchy within each football program. It allows administrators and analysts to confirm proper reporting structures, identify coaching assignments for each season, and understand how each head coach’s staff is composed. This information can support internal reporting, hiring reviews, and analysis of staff consistency across multiple seasons.

#5 This query calculates and displays the average game attendance for each division during the 2024 season. It joins the season, game, team, and division tables to connect games to the teams and divisions they belong to. The AVG() function is used to determine the average number of spectators per game for each division, and the results are rounded to zero decimal places for clarity. The query groups results by division name to provide one row per division and orders them in descending order based on average attendance, highlighting the divisions with the highest fan turnout.

<img width="763" height="352" alt="Query5" src="https://github.com/user-attachments/assets/7ce01749-b6bf-4117-97e4-d90207f913ec" />

This query provides valuable insight into fan engagement and stadium attendance trends across divisions. Conference administrators and marketing teams can use this information to identify which divisions draw the largest crowds, evaluate the effectiveness of promotional efforts, and allocate resources accordingly. It also helps track changes in attendance patterns from season to season, supporting strategic planning for scheduling, media coverage, and facility investment.

#6 This query identifies all teams whose average home game attendance in the 2024 season was higher than the overall league average. It calculates each team’s average home attendance by joining the game, team, and season tables, grouping by team name. The HAVING clause contains a subquery that computes the average attendance across all games in the 2024 season. Only teams with an average higher than this subquery result are included in the output. The results are then ordered in descending order by average attendance to highlight the most popular home teams.

<img width="716" height="347" alt="Query6" src="https://github.com/user-attachments/assets/86f41dda-efc4-4018-a848-47dfe3ae3e19" />

This query helps conference administrators and marketing analysts determine which football programs have the strongest fan engagement and most consistent home game turnout. Understanding which teams attract above-average attendance can guide marketing efforts, sponsorship opportunities, and game scheduling decisions. It can also highlight programs with growing fan bases or successful promotional strategies worth replicating across other teams.

#7 This query identifies all games that were won by teams whose quarterbacks accumulated fewer than 4,000 total passing yards during the season. It joins the team, roster, player, position, player_game_stats, and game tables to link teams with their players’ performance data. The WHERE clause filters results to only include quarterbacks (position_name = 'QB') and winning teams, whether the team played at home or away. Using the HAVING clause, the query groups results by team and game and filters for teams whose total passing yards fall below 4,000. Results are ordered chronologically by game date.

<img width="2048" height="1150" alt="Query7" src="https://github.com/user-attachments/assets/fdd29792-0662-4596-b1e7-133035eb2155" />

This query allows analysts and coaching staff to identify teams that succeed despite limited passing productivity, highlighting programs that rely on strong defense, rushing performance, or overall team balance. It provides insight into different winning strategies across the league and can be used by performance analysts to compare offensive styles and efficiency. This information could also guide scouting, training focus, and future game-planning decisions.

#8 This query identifies all coaches who were not assigned to any team during the 2024 season. It retrieves each coach’s first name, last name, and area of specialty from the coach table. The NOT EXISTS condition checks for the absence of a matching record in the coach_assignment table for the 2024 season by using a correlated subquery that links coach.idcoach to coach_assignment.idcoach and filters by season_year = 2024. Only coaches without a 2024 assignment appear in the final output. The results are ordered alphabetically by last name.

<img width="683" height="410" alt="Query8" src="https://github.com/user-attachments/assets/9fdb73f2-6e22-4d51-878f-c0dedfd216ef" />

This query is valuable for administrative and operations purposes, helping league or team managers identify coaches who were not active during a specific season. The results can assist with staffing analysis, hiring decisions, and maintaining an up-to-date record of available or inactive coaches. Additionally, it ensures data accuracy by confirming that every active coach has an appropriate assignment recorded for the season.

#9 This query calculates the total number of games hosted at each stadium during every season. It joins the game, stadium, and season tables to associate games with their locations and corresponding seasons. Using the COUNT() aggregate function, it determines how many games were played in each stadium for each year. The results are grouped by stadium name and season, then sorted in descending order by total games hosted, allowing for quick identification of the most active venues across the league.

<img width="581" height="571" alt="Query9" src="https://github.com/user-attachments/assets/4d0abc78-13d0-45cf-9904-fadc6e4dc094" />

This query provides valuable information for event scheduling, facility management, and revenue planning. By identifying which stadiums host the most games, conference officials can better understand venue usage patterns, evaluate wear and maintenance needs, and plan improvements or expansions accordingly. It also offers insight into geographic scheduling balance and assists in determining fair allocation of home-field opportunities for teams.

#10 This query identifies all players whose last names begin with the letter “S” and who maintain an above-average receiving yard total compared to all players in the league.
It joins the player, roster, team, and player_game_stats tables to connect players to their teams and game performance.
The REGEXP '^S' condition filters by name pattern, and a subquery in the HAVING clause calculates the league-wide average receiving yards, ensuring only those who exceed that benchmark are displayed.
A secondary condition in the subquery excludes players with zero receiving yards to maintain accuracy in the comparison.

<img width="879" height="322" alt="Query10" src="https://github.com/user-attachments/assets/a7a769eb-e9e8-41db-909c-00c72a637786" />

This query combines text-based filtering and statistical comparison to deliver actionable insight. Analysts or coaches could use this to quickly identify standout wide receivers or tight ends whose production surpasses league averages — useful for recruiting, awards consideration, or performance reviews.
It also demonstrates flexible analytical filtering (pattern-based and numeric) across multiple dimensions of the dataset.

## Database Information
Name of the database: cs_jhs03654

