# NHL-Player-rating

Playing EA sports (EAS) NHL over the years, I have often wondered how they come up with their player ratings. According to each player bio and profile, the total score (usually running from around 70 to 100) is derived from a combination of various parameters attributed to each player. For example, speed, acceleration, shot power and shot accuracy (or similar), stick handling ability, stamina, aggression, intensity, and many more are used. But, in reality, how can you measure intensity or aggression, or even stick handling ability to the point where you can score it in comparison to everyone else? The whole concept of EAS rating system (for all of their sports games) is a bit of a secret sauce to which little is known. So, I thought I'd have a go of making my own player rating system using nothing but good old player metrics

Glossary of terms of advanced hockey statistics:

I'm assuming the reader understands the concept of what basic hockey statistics are, such as Points (Goals + Assists), Average Time on Ice (per game), Block Shots, Give-away, Take-away, Hits, Penalties Drawn, Penalties Taken, Faceoffs Won, Faceoffs Lost. But, perhaps there are some more advanced metrics that are foreign and so I'll briefly explain their meaning. It should be noted that all advanced hockey stats used are for even strength situations

CF%: CF stands for "Corsi For", which is the number of shots a player's team generated when that player was on the ice as opposed to "Corsi Against" (CA), the number of shots generated for the opposing team whilst that player was on the ice. CF% is simply CF / (CF + CA).

CFQoC: The Quality of Competition's average CF. The higher the number, the higher the level of competition. A somewhat debated statistics regarding its interpretation, but I like it when it's used in context with the next metric below.

OZS: Offensive Zone Starts. The percentage of times that given player started their shift in the offensive zone.

PDO: It is not acronym and was given the name by Vic Ferrari, a hockey blog writer. It is simply a proxy of luck. It is the shooting percentage plus the save percentage of the player's team whilst that player is on the ice. Nice article here for more 

xGF%: The ratio of Expected goals for versus Expected goals against. Expected goals are simply the likelihood of a goal being scored from a given shot. It provides judgement on the quality of shots. Hence, an xG with a value of 0.2, means the shot should be a goal 20% of the time

DPS: Defensive Point Shares. One of three metrics devised by Hockeyreference.com (others being Offensive Points Shares and Goalie Point Shares) that allocates a score to each point scored by a player using the concept of marginal goals for and marginal goals against. Acquiring DPS is harder than for OPS, hence defensive forwards and D-men have higher DPS than most offensive forwards. The link is given for more information

