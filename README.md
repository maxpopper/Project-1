 # Project-1
By Max Popper, Kimani Philips, Kaleb Tesfay, and Maruti Biswas
Random Number List Generator:
https://www.calculatorsoup.com/calculators/statistics/random-number-generator.php


## Background
There's a common debate among basketball fans about who the greatest player of all time is. Usually, people are either firmly rooted in the belief that Michael Jordan is the greatest, or that LeBron James is the greatest. Jordan fans will point to the fact that Jordan has won more championships and has led the NBA in scoring more than LeBron has. LeBron fans will argue back that no one has been as consistent as LeBron for how long he's been playing. These arguements are very subjective, and people often pick and choose comparative metrics that favor their preferred player. One statistic that people often overlook is VORP, or "Value Over Replacement Player."

According to www.basketball-reference.com (the hub for all basketball statistics), VORP is "a box score estimate of the points per 100 TEAM possessions that a player contributed above a replacement-level (-2.0) player, translated to an average team and prorated to an 82-game season." Essentially, VORP is a quantitative metric that measures the individual value of a player. Going off of VORP alone, not only is LeBron's VORP is higher than Jordan's (148 vs 116), it is the greatest VORP of all time... since the 1973-74 season.

VORP's main limitation as a statistic to cite when talking about the greatest players of all time is that it can only date back to the 1973-74 season. Key statistics in calculating a players VORP like blocks and steals (among others), were not recorded prior to that season. This means that we do not have accurate career VORPs for all-time great players like Wilt Chamberlain, Oscar Robertson, Walt Frazier, Jerry West, and Kareem Abdul-Jabar. Interestingly, despite Kareem having played a portion of his career in the "pre-VORP era," he sits at 8th all-time on the career VORP list (85.72). He began his career in the 1969-70 season and went on to play for 20 seasons. This means he played 80% of his career (16 seasons) in the "VORP era." What if we could calculate his career VORP? How would the all-time career VORP list change if we were able to estimate the VORPs of players who played some or all of their career prior to the 1973-74 season? This is the question we set out to answer.

## Methods
We figured that the best way to go about estimating players' VORPs prior to the 1973-74 season was by creating a regression. We gathered a random sample of 199 players who played their entire career in the VORP era, and recorded several important statistics that were also kept in the pre-VORP era. In addition to these 199 players' VORPs, we kept track of how many seasons and total they played over their career, their career average points per game (**PPG**), assists per game (**APG**), rebounds per game (**RPG**), minutes per game (**MPG**), field goal % (**FG%**), field goals per game (**FGPG**), games per season (**GPS**), how many championships they won, and if they were inducted into the Hall of Fame (**HOF**). 

Ultimately, we had to eliminate championships due to confounding variables. Our initial idea for recording these variables was that players with high VORPs tend to be inducted into the Hall of Fame more than players with unimpressive VORPs, and that the best players tend to win more championships. Proving our assumptions wrong, 2 of our 199 players won 3 championships each, despite having a negative career VORP. These players happened to play for the Chicago Bulls in the mid-1990's during their historic championship run that saw them win 6 championships that decade. Additionally, we eliminated FG% due to a lack of correlation. Below are a boxplot (which also compares our data to the predicted) and histogram of the VORPs in our dataset:
![vorp boxplot][actual_vs_pred_VORP_boxplot.png]
![vorp histogram][actual_VORP_hist.png]

Using these variables as they are gave us a model with an *r^2 value of ~.75*, which we were not satisfied with. We realized that Seasons Played had a high correlation with career VORP, so we decided to weight all of the "per Game" statistics, multiplying them by Seasons Played. PPG became PPGxSP, APG became APGxSP, etc. Our model ended up consisting of PPGxSP, APGxSP, RPGxSP, MPGxSP, FGPGxSP, GPS, and HOF, and has an *r^2 value of ~.88*, which we were satisfied with. Below are our training and testing split models:
![training model][train_scatter.png]
![testing model][test_scatter.png]



##Outcome
We selected a random sample of 30 pre-VORP players, as well as hand-picking several all-time great players who were on the recent NBA 75th Anniversary team (consisting of 75 of the greatest players of all time, spanning the entire 75 year history of the NBA). We had to eliminate 9 of the 30 randomly selected players due to the fact that rebounds were not recorded in the very early days of the NBA, so we are left with 27 pre-VORP players in our dataset (including the HOFers). Below is a scatterplot showing the positive correlation between Estimated VORP and Total Seasons Played in our pre-VORP dataset:
![pre vorp scatterplot][est_vorp_scatter.png]

Some players in our dataset played a portion of their career in the VORP era. We took this into account by only recording their statistics for the pre-VORP years of their career, and adding their estimated partial VORP to their actual partial VORP. Additionally, if a HOFer played a portion of their career in the VORP era, we multiplied their **HOF** coefficient by the portion of their career spent in the pre-VORP era. For example, Kareem Abdul-Jabaar pleyed 20% of his career in the pre-VORP era, so we would multiply his **HOF** coefficient by *.2*. Below is the same scatterplot as above, but color-coordinated by each player's Hall of Fame status:
![HOF est vorp scatterplot][est_vorp_HOF_vs.png]

Our model was very good at estimating the VORPs of the players who did not have notable careers, but seemed to be inaccurate at estimating the VORPs of HOFers. It's implausible that a player with the career statistics of Wilt Chamberlain would have a VORP of less than 20. We have realized that the majority of our dataset consisted of players with negligible careers, and that highly valuable players tended to be outliers. It is unreasonable for a regression model to be able to accurately predict outliers
