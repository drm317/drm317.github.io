---
layout: page
title: Software Craftsmanship - Mini Project
tagline:
categories:
- blog
tags:
- tdd
- craftsmanship
- kata
---

<h3>Classic Football Management Game</h3>

<h4>Estimated Duration: 1 day</h4>

<h4>Author: Daniel Marlow</h4>

<h4>Language / Stacks - Any</h4>

<h3>Summary</h3>

Develop a classic football management game using test-driven development.
Refactor your code applying the SOLID design principles.

Write a game that will allow you to play as the manager of a football (soccer) team.

The game should have the following features:

1. Allow you to play as the manager of a team initially in the bottom league
of a five-league tournament. There are 20 teams in each league. The computer manages all the other teams.

2. Each season consists of 40 games. Each team in every league plays each other twice per season. The winning team scores more goals than their
opposing team.

3. Three points are awarded for a win. One point is awarded for a draw to
each team. No points are awarded to losing teams. Goal difference
(goals for vs goals against) is counted when teams have equal points in a league.

3. Each team can have up to 20 players. Only 11 players play in games.
Teams consist of one goalkeeper and any combination of defenders, midfielders
and strikers.

5. Footballers have the following attributes: price, attack skill, defence skill. The total combination of attack skill across all the footballers in a team determines how likely the team is to score. The total combination of defence skill across all the footballers in a team determines how likely
the team will prevent the other team from scoring.

6. You choose the players in your team before each game. When you start your
game, all other games in the different leagues are played at the same time.

7. Football players can be bought and sold with other teams. Players
become randomly available to buy after each game. You start with 100,000 credits.

8. At the end of each season three teams are promoted to the next league
and three are relegated. Three new teams enter the bottom league. The
top three teams in the top league receive 1,000,000 credits.

9. Teams in the top league receive 50,000 credits per game from tickets.
All other teams receive 10,000 credits per game.

10. Players have a 5% chance of being injured in a game. This affects
a player's attack skill or defence skill by between 10% and 100%. Injuries last
for between one to four games.

11. The league tables should be recalculated after each game. They should
show the team  name, number of games played, number of wins, number of
draws, number of loses, goal difference and total points.

If time permits, it could have more advanced features:

1. Minute-by-minute commentary of your team's games. Each game lasts 90 minutes.

2. Super cup competition consisting of the top three teams from the top
league after the first season and the top three teams from the highest league from 10 other countries.

3. Graphical representation of the players in the your team's games.
