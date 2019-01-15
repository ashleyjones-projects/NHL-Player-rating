# NHL-Player-rating

Playing EA sports (EAS) NHL over the years, I have often wondered how they come up with their player ratings. According to each player bio and profile, the total score (usually running from around 70 to 100) is derived from a combination of various parameters attributed to each player. For example, speed, acceleration, shot power and shot accuracy (or similar), stick handling ability, stamina, aggression, intensity, and many more are used. But, in reality, how can you measure intensity or aggression, or even stick handling ability to the point where you can score it in comparison to everyone else? The whole concept of EAS rating system (for all of their sports games) is a bit of a secret sauce to which little is known. So, I thought I'd have a go of making my own player rating system using nothing but good old player metrics

Glossary of terms of advanced hockey statistics:

I'm assuming the reader understands the concept of what basic hockey statistics are, such as Points (Goals + Assists), Average Time on Ice (per game), Block Shots, Give-away, Take-away, Hits, Penalties Drawn, Penalties Taken, Faceoffs Won, Faceoffs Lost. But, perhaps there are some more advanced metrics that are foreign and so I'll briefly explain their meaning. It should be noted that all advanced hockey stats used are for even strength situations

CF%: CF stands for "Corsi For", which is the number of shots a player's team generated when that player was on the ice as opposed to "Corsi Against" (CA), the number of shots generated for the opposing team whilst that player was on the ice. CF% is simply CF / (CF + CA).

CFQoC: The Quality of Competition's average CF. The higher the number, the higher the level of competition. A somewhat debated statistics regarding its interpretation, but I like it when it's used in context with the next metric below.

CFQoT: This is the CF% of a player's line-mates. It indicates how a player contributed to the overall play compared to his team-mates. Usually a good indicator to see if a given player made the players around him better.

OZS: Offensive Zone Starts. The percentage of times that given player started their shift in the offensive zone.

PDO: It is not acronym and was given the name by Vic Ferrari, a hockey blog writer. It is simply a proxy of luck. It is the shooting percentage plus the save percentage of the player's team whilst that player is on the ice. Nice article here for more 

xGF%: The ratio of Expected goals for versus Expected goals against. Expected goals are simply the likelihood of a goal being scored from a given shot. It provides judgement on the quality of shots. Hence, an xG with a value of 0.2, means the shot should be a goal 20% of the time

DPS: Defensive Point Shares. One of three metrics devised by Hockeyreference.com (others being Offensive Points Shares and Goalie Point Shares) that allocates a score to each point scored by a player using the concept of marginal goals for and marginal goals against. Acquiring DPS is harder than for OPS, hence defensive forwards and D-men have higher DPS than most offensive forwards. The link is given for more information

The algorithm explained:

There are 8 overall parameters which are stacked together and passed through a Sigmoid function to yield probability values between 0 and 100 (or 0 and 1 and multiplied out by 100). Each parameter has its own weight of importance. How these are chosen is arbitary, but I feel they do a fairly good job of producing an overall rating that compares well to the EA sports ratings. This analysis can be found in the player_rating.ipynb. 

So the algorithm

rate = is the projected scaling factor a player has if he didn't appear in 82 games. This number is >= 1.0. To account for players missing games, this factors when applied puts all players on the same level as if they had played 82 games.

blk and hit: are the projected number of block shots and hits a player makes through 82 games

gwy, pen, and fo: are the projected differential of "takeaways minus giveaways", "Penalties drawn minus penalties taken", and "Faceoffs won minus faceoffs lost", respectively

toi: is the average time on ice. This value is squared to give more weight to players who play long minutes. Seperates the best from the average. This is the "stamina" parameter

defn: This is the "responsability" parameter. Perhaps the most complex to explain. It uses CFQoC and multiplies this value by adjusted OZS (see below for description). It is a ratio. This numerator value provides an idea of the situation on average a player was in. Was he playing against tougher or easier opponents? and which zone was he most likely to face those opponents? The larger this number the harder the opponent and more defensive responsibility they had. It is still possible to score high if a given player started a lot in the offensive zone, but the CFQoC would have to be a magnitude of two times larger compared to the defensive zone players, which is rarely the case. The denominator is simply a set of scaling factors. This combined value is then multiplied by scaled CF%, CFQoT, and xGF% to account for how well those players did given the situation they were in whilst taking into account their line-mates. For example, players who play hard defensive minutes, have CF% > 50%, and are making their linemates better, are doing well here. Think Patrice Bergeron, Pavel Datsyuk, Henrik Zetterberg types. These guys have high scores here!

ozs_adj = OZS on average has a league-value of 50% (see player_rating.ipynb). However, this doesn't take into account individual teams. Take the LA Kings, who had a Team average of 54.4%. This is larger than 50% and means they were a good puck possession team. Perhaps the most defensive player on the team has a OZS of 50%. If we were to run this through defn without adjusting, this player would not be rated correctly as his team value is biased to the league average. We would need to remove 4% off all players on the Kings team for it to balanced. Essentially, teams that defend well have better puck possession and keep the puck in the opponents zone, but without adjusting for it, we would be ignoring that fact.

pts: Is the "productivity" parameter. This parameter carries in general the most weight. This is a weighted combination. Firstly projected points through 82 games are divided by the PDO to remove the luck bias and put everyone on an even-keel. The second metric concerns DPS which rewards players that have contributed from the defensive stand-point and is beneficial to defensemen to give more weight to their ratings. The weights chosen are arbitary, but the overall impact of DPS (~ < 10%) is small compared to the overall points metric.

GSS_adj: linear stacking of the above variables

On average, productivity accounts for about 50%, responsability about 35%, stamina about 7%, and the remaining 8% is everything else of GSS_adj. See the plots in the player_rating.ipynb

The ratings provided are a three year weighted average of individual yearly ratings over the last three years (2016, 2017,2018), such that a player that has played all three years has their individual yearly ratings weight-distributed as 50%, 30%, and 20%, for the previous year, 2 years ago, 3 years ago, respectively. Only players with 10 or more games in a given season are rated. You can find the player ratings in 2018.csv


