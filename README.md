# League of Legends - Analysis of Gameplay Factors for the Winning Team

## Selected Topic

The selected topic for our final project was analyzing the conditions that factored into a team win in the game League of Legends. Using a data set pulled from the Riot API with data from 10,000 games worth of gameplay, we are looking for the most important factors that made the winning team successful, and creating a model to predict the outcome of a game as it progresses based on various statistics.

## Reason why the Topic was Selected

League of Legends is one of the most popular games ever created. Since its initial publication by Riot Games in 2009, it has become a globe encompassing e-sport with 115 million active monthly players currently worldwide. The money that comes along with this popularity is substantial, with millions of dollars’ worth of prizes every year. 

Given its popularity, most players analyze their game statistics, or those of a professional player, to improve. While there are many services that offer individual-player-specific analytics, not many focus on the overall team’s objectives, because (outside of competitive teams) it is a single-player game. Our interest in this meta-level analysis to find predicting factors in hero selection and gameplay is unique compared to the other analyses of League of Legends data we have seen.

## Background Information

League of Legends is a multiplayer online battle arena game, with two teams (team “Red” and team “Blue”) comprised of five players, where each player controls a single character with a set of abilities that improve as the character’s level increases.  Each team works together to advance down the three lanes on the game’s map to destroy the buildings the comprise the other team’s base. The ultimate objective is to destroy the other team’s main base structure, called the “Nexus.” There are many defenses standing between the other team and its Nexus, including 9 protective towers (which will auto-attack anything within range of them), and relatively weak computer-controlled units called “Minions” which  march down each of the lanes, attacking anything in their way until they are killed off. Destroying defensive units, the other team’s minions, and killing other heroes all award experience points and gold which allows players to level up and get better items and abilities. 
Another way to generate experience points and gain additional benefits is to kill “monsters” which populate the area between the lanes on the map. These monsters are classified as either a standard monster or an elite monster. The elite monsters are much more difficult to kill, and often require a much larger time investment on the part of the team, but they also grant much more powerful benefits to the team who defeats them. 

## Description of source data

Our data is originally sourced from the Riot API and includes match data from 10,000 games, which we found on [Kaggle](https://www.kaggle.com/benfattori/league-of-legends-diamond-games-first-15-minutes). This includes which team (team Red or team Blue) ultimately won, as well as statistics regarding how many times an enemy team’s heroes were killed, how many “minions” or other objectives that would allow a player or team to increase their level were attained, and how many of a team’s structures were destroyed before the destruction of the final “Nexus” structure which wins the game.

The data is pulled from games by players playing within the "Diamond" level tier on the League of Legends ranking system. There are nine tiers within League of Legends which indicate the skill level of the player: Iron, Bronze, Silver, Gold, Platinum, Diamond, Master, Grandmaster, and Challenger. Being ranked in one of these levels allows the game's AI to match players of a similar skill level against each other. Master, Grandmaster, and Challenger tiers each comprise a single division of the most skilled and competitive League of Legends players and the rankings change every 24 hours, making Diamond the highest ranked standard tier (2.4% of the player base). We chose data from Diamond level players because the high level of skill required to achieve the rank removes other player-specific variables and assumes a certain level of understanding and mastery of the players' selected Champions and team's objectives. It is therefore a good baseline to analyze the data from. 

## Questions we hope to answer with the data

With our analysis of this League of Legends data set, we hope to identify what factors contribute most to a team’s victory in the current game patch. We hope to then be able to, given a set of parameters, confidently predict the outcome of a match. While ultimately the individual players win or lose the game, having objectives to work towards increases team cohesion and will enable better gameplay. 

## Method

***Extracting, Transforming, and Loading the League of Legends Data***

Our data set provides 39 columns, giving statistics on how the teams played. Each of these terms is applied to both red and blue teams in the data set. The list is as follows:

Column Term | Definition
------------ | -------------
gameId	| The unique ID that acts as the primary key to this data set.
Wins		| Whether the Blue or Red team won.
FirstBlood		| Which team got the first kill of a Champion in the game.
Kills		| The number of times a member of either Red or Blue team killed a member of the other team.
Deaths		| The number of times a member of the Red or Blue team died. 
Assists		| The number of times a member of the Red or Blue team did damage which resulted in a kill, but did not get the final hit on the kill.
EliteMonsters		| The number of times a member of the Red or Blue team killed an Elite Monster (a Drake, Elemental Dragon, etc.)
Dragons		| The number of times a member of the Red or Blue team killed a dragon - which grant special abilities and change the layout of the map.
Heralds		| The number of times a member of the Red or Blue team killed a Herald.
TowersDestroyed		| The number of times a member of the Red or Blue team destroyed one of the other team's towers. There are 11 towers for each team.
TotalGold		| The amount of gold the Red or Blue team accumulated over the course of the game.
AvgLevel		| The average level the members of the Red or Blue team attained.
TotalExperience		| The amount of experience points the Red or Blue team accumulated over the course of the game.
TotalMinionsKilled		| The number of Minions the Red or Blue team killed over the course of the game.
TotalJungleMinionsKilled		| The number of Jungle Minions the Red or Blue team killed over the course of the game.
CSPerMin		| The number of Minions (including Jungle Minions) that the Red or Blue team last-hit to collect resources from over the course of the game.
GoldPerMin		| The average rate at which the Red or Blue team collected gold over the course of the game.
	
After our initial look through the data, we decided that to best make our data ready for our model, we should look at the differences between team performance instead of individual team performance. The reasoning behind this decision is that many of the mechanics listed in the above table are reflective of game time. For example, each team’s Nexus spawns Minions beginning at 1:05 of the game, and continues to spawn them at a rate of 6 or 7 for the remainder of the game. As the death of these Minions generates experience points for the other team, the longer a game progresses the more experience each team is going to collect. We decided the best way to judge performance as a factor in the statistics, as opposed to keeping time as a factor, was to instead take the difference of the two teams’ numbers measuring against Blue team’s performance in each category.

We ended up with the following table column headers for our analysis based on the original numbers pulled from Riot:

Column Term | Definition
------------ | -------------
blueKills_diff | The difference between the Kills columns for red and blue team (see previous table).
blueDeaths_diff | The difference between the Deaths columns for red and blue team (see previous table).
blueAssists_diff | The difference between the Assists columns for red and blue team (see previous table).
blueEliteMonsters_diff | The difference between the EliteMonsters columns for red and blue team (see previous table).
blueDragons_diff | The difference between the Dragons columns for red and blue team (see previous table).
blueHeralds_diff | The difference between the Heralds columns for red and blue team (see previous table).
blueTowersDestroyed_diff | The difference between the TowersDestroyed columns for red and blue team (see previous table).
blueAvgLevel_diff | The difference between the AvgLevel columns for red and blue team (see previous table).
blueTotalMinionsKilled_diff | The difference between the TotalMinionsKilled columns for red and blue team (see previous table).
blueTotalJungleMinionsKilled_diff | The difference between the TotalJungleMinionsKilled columns for red and blue team (see previous table).
blueCSPerMin_diff | The difference between the CSPerMin columns for red and blue team (see previous table).
blueGoldPerMin_diff | The difference between the oldPerMin columns for red and blue team (see previous table).

```


#drop the columns that are redundant because if blue team wins then red team loses and all variables are differentials
diff_df = df.drop(drop_columns,axis=1)
diff_df
```
	
***Machine Learning Model***

The first model we generated was based on the differences on the Blue Wins target variable. 

```
#create the variables
y = diff_df['blueWins']
X = diff_df.drop('blueWins',axis=1)

#create training and testing sets
X_train,X_test,y_train,y_test = train_test_split(X,y,random_state=1)

#create the machine learning model then fit and predict
brfc = BalancedRandomForestClassifier(n_estimators=100,random_state=1)
brfc.fit(X_train,y_train)
y_pred = brfc.predict(X_test)
```

Our first attempts at creating a model resulted in an above average accuracy score of 72%. 

In the coming week we will work on trying other models and experiment with visualizations in tableau to try to understand the best fit model for our data.

