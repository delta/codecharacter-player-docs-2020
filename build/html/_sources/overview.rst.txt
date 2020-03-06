Overview
========

Hello, and welcome to the player documentation for Code Character!

Code Character is a strategy programming game where you control troops in a turn-based game with code you write -
in our favorite language C++ :)

Let’s get started with a quick tutorial on how to get started.


Dashboard Interface
-------------------

Once you log in, you’ll see your dashboard, as shown in the image below.

.. figure:: _static/dashboard_idle.png
   :width: 800px
   :height: 400px
   :alt: Code Character Interface

**On the left is the editor**, where you can type your code. You’ll notice on logging in, that you’re provided some default code. It doesn’t do much in terms of strategy, but it uses most of the important elements of the code API, so a quick read through it will help.

**On the bottom right is the debug window**, it shows your compilation errors at compile time and your debug logs and errors at runtime.

**On the top right is the renderer window**, which actually displays your game. Use the mouse to pan around, and view your game after it’s complete.

Click on the :guilabel:`?` icon on the bottom left to get a tour of your dashboard.

We’ll begin with a quick run through of the concepts.


Game Rules
----------

Codecharacter is a game of strategic resource management. The objective of the game is to occupy the flag area on the
map and defend it from the enemy troops.

Your troop consists of **Bots** and **Towers**.

**Bot** - Unit that can move across the map and attack opponent units by blasting. A bot can also transform to a tower.

**Tower** - Stationary and more powerful units that can suicide blast and act as a barrier to bots from moving through.

.. figure:: _static/actors.png
   :width: 800px
   :alt: Player Actors

Towers occupy a 1x1 unit square area on the map and bots cannot move through a tower. Towers are created when a bot transforms 
into one.

The map has three types of terrain : **Land**, **Water** and **Flag**. Water terrain is inaccessible to bots.
Land or flag terrain that is occupied by a tower is also not accessible to bots.

.. figure:: _static/tiles.png
   :width: 800px
   :alt: Tiles

Tower can only be constructed on land/flag. A specific number of bots are spawned every turn.

This is how a typical game map looks like

.. figure:: _static/map.png
   :width: 800px
   :alt: Typical map

The goal of the game is to occupy majority of the flag area on the map.

You are given a fixed number of instructions you can execute every turn. Exceeding the limit on a turn makes you skip the turn. 
Exceeding the total instructions limit by an excessive amount makes you lose the entire match, so ensure that you keep your code
as short and efficient as possible!

.. note:: This is probably enough for you to get a start, but you might want to take the time to read the complete rules in the Rules section.

Code Guide
----------

The way you interact with the game is through your code for the ``update`` function, which is called every turn of the game. 
Here, you can issue commands to your bots and towers, such as blast, move or transform.

All the data about the current state of the game is stored in a variable called ``state``. This variable is simply a struct, 
so you can read any of its members. The state is also how you’ll represent the output of your code, which will be in the
form of command variables that you set each turn.

.. code-block:: cpp

	// Get the properties of first bot
	// Notice that you can use auto instead of a type name
	auto bot_id = state.bots[0].id;
	auto bot_hp = state.bots[0].hp;
	
	// Checking if the last tile of the map is valid to construct a tower on
	// Notice how constants like MAP_SIZE exist for your ease. See the complete
	// list of constants in the constants tab to the left
	if (state.map[MAP_SIZE - 1][MAP_SIZE - 1] == TerrainType::LAND) {
		...
	}

	// Issuing a command to your second bot to move to position (3, 3) in map
	// Note that, you'll have to add a check to ensure that bots has 
	// atleast two elements to access.
	// Otherwise, you'll end up in a segmentation fault!
	state.bots[1].move({3, 3});

	// Issuing command to move and blast at location (2, 3). Any enemy units 
	// close to this bot will incur damage.
	state.bots[2].blast({2, 3});


	// Issuing a command to send a bot to a flag location and transforming
	// to a tower. Notice the usage of Vec2D, a utility class that's predefined.
	// All representations of positions and offsets in the game are DoubleVec2D.
	DoubleVec2D flag_position = state.flags[0];
	state.bots[0].transform(flag_position);

	// Issuing a specific command to all towers to blast

	// Notice that range based for-loops can be used.
	// Remember to add the & while iterating, otherwise you'll be modifying
	// be modifying a copy of the tower.
	for (auto& tower : towers) {
		tower.blast();
	}

	// Or, you can use standard iterator based for-loops
	for (int i = 0; i < state.towers.size(); i++) {
		state.towers[i].blast();
	}

More details about ``state`` can be found in `Player State <player_state.html>`_.

Competetion Guide
-----------------

Ultimately, Codecharacter is a game of competetion! The objective is to challenge other players and fight
your way to the top of the leaderboard. To help you along this process, we offer pre-programmed AIs, against
which you can test your code. Additionally, you can also try testing your code against itself!

You can run code on three different maps, against either `your` own code, or against one of our preprogrammed AIs.

Once you’re satisfied with your code and want to compete on the leaderboard, hit :guilabel:`Submit Code`. This will allow you to 
challenge anyone on the leaderboard with the submitted code. To challenge another player, simply click the challenge button
next to their nickname on the leaderboard. You can keep submitting and updating your code whenever you want.

Note that once you `submit code`, anyone can challenge you at anytime, and a match will automatically be simulated between you
and the opposing player. You will receive a notification once the match ends, and you can view it in the :guilabel:`Battle TV`.

Players are divided into two divisions

* Division 1 - Rating > 1700
* Division 2 - Rating <= 1700

Every 6 hours, every Division 1 player is matched with every other player in the same division. So, it's better to keep your 
best code submitted. (NOTE: This feature will be available from 12am IST 10th March, 2020)

You can also save different versions of your code by using the :guilabel:`Commits Tab` on the dashboard. A match can be initiated by you 
against your own previous code version.

For each of your matches, 5 games are played on 5 different maps. You can only see the first three games, the last two are mystery 
maps! If you win the best of five, you win the match and your rating will increase. Challenge and defeat players with higher ratings
to boost your rating further.

The list of matches you've played and top rated matches by other players are also available to watch on the :guilabel:`Battle TV`.

.. warning:: Running the same code against itself might not lead to a TIE always. This is due to the limitations of
	the precision of double variables. As long as you are competing against a different code, this difference
	will not affect the game result.

The leaderboard evaluates your position using your rating, which is based purely on the outcomes of your matches with other players.
The Glicko ranking mechanism is used to calculate ranks. Players who are more actively playing matches are rewarded by this rating 
system, they tend to suffer lesser fall in ratings.
