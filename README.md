# DSA210

Project: In my project I will research the relation between nba players' stats and their career lengths. I will be using the data of the players that started their career in 1980 or later, because before 1980 there was no 3 point line which changed the game for good. Also, the NBA and ABA merger happened in 1976, and stats like steals and blocks started to be tracked in 1974. Moreover, since I don't want the data of active players(their career lengths are unknown.), I will not use the data of the players that didn't retire in the 2020 season or before. Finally I will not use the data of the nba players that played less than 10 games, since it would be a very small sample size of stats. With these restrictions in mind, I will use the stats of every NBA player all-time.

Motivation: I am quite interested in the nba, so I will be looking into nba data and finding out which stats are significant to a career's length.

Null Hypothesis: There is no significant relationship between an nba player's career data of points per game, assists per game, rebounds per game, body mass index(BMI), player efficiency rating(PER), steals per game, blocks per game, and turnovers per game and his career length.

Alternative Hypothesis: There is a significant relationship between an nba player's career data of points per game, assists per game, rebounds per game, body mass index(BMI), player efficiency rating(PER), steals per game, blocks per game, and turnovers per game and his career length.

Sources: I'm using data from kaggle which is in this link: https://www.kaggle.com/datasets/ryanschubertds/all-nba-aba-players-bio-stats-accolades?resource=download. This data includes the names, height, weight, points per game, assists per game, rebounds per game, starting year of career, ending year of career, career length, player efficiency rating(PER), and number of games played for every NBA player from 1947 to 2022. Also, I will extract data of steals per game, blocks per game, and turnovers per game myself using nba_api, because the kaggle data doesn't provide these stats and I need them in my tests. Then, I will match the data that I extracted from nba_api with the data from kaggle and use this dataset to do EDA's and test my hypothesis. Finally, I will apply machine learning methods while I enrich my data and make feature transformations


