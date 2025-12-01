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
<img width="572" height="304" alt="feature" src="https://github.com/user-attachments/assets/b644b57a-6450-46e5-a43e-29eb4ff5dba4" />

#1. This query creates a view, attendance_metrics_view, that summarizes seasonal attendance performance for every team in the league. It joins the team, stadium, game, and season tables to gather all relevant information about home games, stadium capacity, and attendance figures. It then calculates key performance metrics such as the average attendance, minimum and maximum attendance, total attendance, and the number of home games played. A particularly important metric produced by the view is average stadium utilization, which measures how well each team fills its stadium relative to maximum capacity. By grouping results by team and season, the query produces a clear and structured dataset that captures long-term attendance trends and stadium efficiency across the league.

<img width="1091" height="897" alt="query1" src="https://github.com/user-attachments/assets/22a7fc2d-8f11-45f9-a072-16878de82ba6" />


Attendance analytics play a major role in evaluating the financial and operational health of college football programs. Stadium utilization directly influences revenue, game-day staffing, concession planning, ticket pricing strategies, and facility management. By creating a view that aggregates these metrics season-by-season, athletic departments can identify trends such as growing fan interest, rivalry-driven spikes, or underperforming seasons. Programs rely on this information to determine whether stadium upgrades are justified, which marketing campaigns work, and how to optimize seating or ticket structures. This query produces the foundational dataset needed for these analyses.

#2. This query produces a season-level snapshot of each team’s passing tendencies by calculating the average number of pass attempts, completions, and passing yards recorded by their players. By joining the team, roster, season, player_game_stats, and game tables, the query connects each player’s game-level passing output to their team and season. The WHERE clause filters out games with zero attempts to avoid skewing the results with players who did not participate in the passing game. Finally, the data is grouped by team and season, generating clear aggregated metrics that highlight how frequently and effectively each team throws the football in a given year.

<img width="1259" height="904" alt="query2" src="https://github.com/user-attachments/assets/f14c26d5-1f41-461b-8f65-d7a864340aff" />

Passing volume and efficiency are key indicators of a team’s offensive identity. Teams that rely heavily on the passing game often feature high-tempo offenses, aggressive play-calling, or elite quarterback talent. By examining average attempts, completions, and yards per game, coaches and analysts can quickly identify how a team prefers to move the ball through the air across different seasons. This snapshot also helps identify trends, such as increasing pass-heavy strategies, quarterback development, or systemic offensive changes under new coordinators.

#3 This query analyzes player size trends across position groups and seasons, focusing specifically on players who weigh more than the league-wide average. It joins four different tables—player, position, roster, and season—to associate each athlete with their position group and the season in which they were rostered. The query uses a subquery to calculate the average weight of all players in the database, then filters the dataset to include only above-average-weight players, ensuring that the results highlight the physically larger athletes in the league. By grouping the results by position group and season, the query calculates the number of heavier-than-average players in each group as well as their average height and weight. This produces a season-by-season snapshot of how size varies across positions, such as offensive line, defensive line, or linebackers.

<img width="1360" height="825" alt="query3" src="https://github.com/user-attachments/assets/608642a7-2b99-43a8-b696-6e5a9b71d526" />

Player size is a defining characteristic of football strategy, especially when evaluating position groups such as offensive line, defensive front seven, or tight ends. By isolating only athletes who weigh more than the league average, this query highlights the biggest and most physically imposing players at each position. Coaches use this information to understand roster composition, track how certain roles evolve over time (e.g., heavier edge rushers), and evaluate whether their recruiting or development aligns with league trends. This data also supports strength-and-conditioning decision-making by benchmarking player size expectations across position groups and years.

#4 This stored procedure, GetStadiumAttendance, provides a flexible and dynamic way to analyze stadium attendance trends across seasons. It joins the stadium, game, and season tables to retrieve attendance figures and hosting information for every stadium in the league. By accepting three input parameters—minimum number of hosted games, minimum average attendance, and an optional season filter—the procedure allows analysts to generate custom attendance summaries without modifying the underlying SQL each time. The procedure uses a HAVING clause to enforce attendance thresholds, making it useful for identifying high-performing venues or evaluating candidate sites for major events. This approach encapsulates complex logic inside a single callable object, demonstrating advanced SQL design.

<img width="1347" height="923" alt="query4" src="https://github.com/user-attachments/assets/c146f6cd-2f4b-457d-9fa2-0e819fc25015" />

Attendance performance is one of the most important financial and operational metrics for athletic departments. This procedure allows analysts to identify stadiums that consistently draw large crowds, host high-profile games, or demonstrate strong fan engagement. With configurable thresholds, it can help administrators evaluate stadium utilization, choose neutral-site game locations, make decisions about facility upgrades, and study trends across different seasons. The flexible nature of the procedure makes it a practical tool for repeated reporting and deeper attendance analysis.

#5 This query evaluates stadium capacity across college football divisions and conferences while highlighting divisions that contain multiple teams with above-average stadium sizes. It joins the division, conference, team, and stadium tables to map each team to its division and associated venue. The WHERE clause uses a subquery to filter results to only those divisions whose teams play in stadiums larger than the league-wide average capacity — a metric computed independently within the subquery. After filtering, the GROUP BY clause aggregates the total number of teams and cumulative seating capacity for each division. Finally, a HAVING clause restricts the results to divisions containing at least two teams, thereby focusing the analysis on meaningful groupings rather than divisions with a single team. This produces a clear summary of how stadium size and team distribution vary across conferences and divisions.

<img width="1022" height="851" alt="query5" src="https://github.com/user-attachments/assets/cf7e5b3d-f3ff-46c9-ae3b-8c18e44aa67a" />

Stadium capacity is a major factor in financial stability, media appeal, and competitive equity within college football conferences. Larger stadiums typically generate higher ticket revenue, greater game-day atmosphere, and increased visibility for athletic programs. This query identifies divisions that not only house above-average stadiums but also include multiple teams with substantial seating capacities. Such insights can influence scheduling decisions, conference realignment discussions, TV contract negotiations, and investment prioritization for facility upgrades.

## Data Visualization
<img width="2048" height="1279" alt="Screenshot 2025-12-01 at 12 34 14 AM" src="https://github.com/user-attachments/assets/b8fa5c78-b6e5-4ff3-9d8a-47b92ad80d7e" />

This dashboard brings together three complementary visualizations that collectively describe how each team’s stadium is performing, how fan engagement is changing over time, and how structural factors like stadium capacity influence attendance outcomes. The goal is to provide a clear, data-driven narrative about stadium utilization, attendance momentum, and capacity efficiency across the league.

1. Stadium Utilization (Top Visualization)

The Stadium Utilization bar chart provides a high-level snapshot of how efficiently each team fills its stadium during home games. By comparing average attendance to total capacity, this visualization highlights which teams achieve the highest utilization rates and which are trending closer to the league average.

In the current view, nearly all teams maintain utilization levels between 89% and 92%, indicating strong fan turnout across the league. The Coal Ridge Wildcats and Sierra Nevada Timberwolves lead all programs with utilization above 91%, while teams such as the Frontier Plains Bison and Tri-Cities Hawks fall slightly below the league pack. This visualization establishes the baseline for understanding each team’s fan engagement and stadium efficiency.

2. Year-over-Year Attendance Change (Bottom Left)

The YoY Attendance Trends chart measures changes in average attendance between the 2023 and 2024 seasons. Positive percentage changes indicate increased fan engagement, while negative values reveal a decline.

This chart immediately surfaces directional momentum that cannot be seen in utilization alone. The Copperhead State Rattlers and Great Lakes Tech Engineers show the strongest YoY increases (up 7.7% and 6.7%, respectively), suggesting rising fan interest or improved on-field performance. In contrast, teams such as the Coast Ridge Wildcats and Bayview Mariners saw attendance decline slightly, even while maintaining strong utilization levels. These contextual shifts help explain whether a team’s strong utilization is sustainable or slipping.

3. Capacity vs. Utilization Scatter Plot (Bottom Right)

The Capacity vs Utilization scatter plot adds structural context by visualizing each team’s stadium size, average attendance, and utilization in one view. Bubble size represents stadium capacity, the X-axis denotes capacity, the Y-axis shows average attendance, and bubble color encodes stadium utilization rate.

This visualization highlights meaningful outliers and performance patterns. The Red Valley Raptors, for example, stand out dramatically: despite having one of the largest stadiums in the league, their attendance remains exceptionally high — demonstrating both scale and efficiency. Meanwhile, smaller-capacity programs such as Tri-Cities Hawks struggle to reach comparable attendance levels, placing them in the bottom-right corner with lower utilization and lower fan draw. This scatter plot clarifies whether utilization outcomes stem from stadium size, local market strength, or fan demand.

Specific Takeaways & Applications

Taken together, the visualizations on this dashboard form a unified analytical story about stadium performance, fan behavior, and infrastructure efficiency across the league.

The Stadium Utilization chart establishes that most teams operate at very high efficiency, consistently filling roughly 89–92% of their available seats. This demonstrates strong baseline demand and suggests that league-wide fan engagement is healthy. However, utilization alone does not reveal whether teams are trending upward or downward in popularity.

The Year-over-Year Attendance Change visualization fills that gap by highlighting momentum. Teams like the Copperhead State Rattlers and Great Lakes Tech Engineers show clear positive growth, posting increases of 6–8%. This upward trend may indicate effective marketing, improving team performance, or increased community support — making these programs candidates for expanded ticketing initiatives or premium seating investments. Conversely, teams with slight declines, such as the Bayview Mariners and Coast Ridge Wildcats, may benefit from promotional campaigns, enhanced game-day experiences, or targeted outreach to re-engage fans.

Finally, the Capacity vs Utilization scatter plot reveals why some attendance patterns look the way they do. For example, the Red Valley Raptors stand out as a major outlier — boasting both one of the largest stadiums and one of the highest average attendances. Their placement suggests that demand may exceed current capacity, making them a strong candidate for stadium expansion or added premium seating. On the opposite end, the Tri-Cities Hawks struggle with both low attendance and small stadium size, identifying them as a priority for fan engagement improvement and potentially reevaluating stadium investment strategy.

Overall Takeaway

The dashboard shows that high utilization alone does not tell the whole story; true performance emerges only when utilization, growth trends, and structural capacity are considered together. This multi-view approach enables decision makers to:

identify teams ready for stadium expansion or facility upgrades,

spot early signs of fan engagement growth or decline,

benchmark programs against similarly sized stadiums, and

allocate marketing or promotional resources more strategically.

This makes the dashboard not just descriptive, but actionable, helping athletic directors, operations managers, and marketing departments make data-informed decisions to strengthen fan attendance and optimize stadium performance.

## Database Information
Name of the database: cs_jhs03654
