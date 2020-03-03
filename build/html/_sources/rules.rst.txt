======
Rules
======

Game Board/Map
==============

The map is a square 30x30 grid of tiles. The origin is at the top-left corner. The X and Y coordinates in Vec2D corresponding to moving *down* and moving *right* respectively, similar to how a 2D array is indexed.

.. Tip:: You can get the width / height of the square map through the ``MAP_SIZE`` constant

Positions
---------

Each bot unit occupies a specific position ``(x, y)`` at any given turn where ``(0 <= x, y < MAP_SIZE)`` and the coordinates
can be floating point values.

Each tower unit occupies one tile of the map bounded by ``(x, y)``, ``(x + 1, y)``, ``(x, y + 1)`` and ``(x + 1, y + 1)``, where
x and y are integral values. You will be given the midpoint of the tower as the position of the tower. 

The map has two different types of terrain - Land and Water. Water is inaccessible to all units.
A bot cannot be at the position blocked by a tower.

Units
=====

There are two types of units - bots and towers.

Bots
---------

Bots can move and blast. They have low HP, blast range and blast damage. They can move to any position reachable 
in the map and blast, and incurring damage only to the enemy units nearby i.e., there is no friendly fire. The blast 
damage reduces linearly as distance from bot reduces. Hence, the closer the bot is to the enemy unit, higher the damage incurred.

.. Tip:: You can access maximum bot blast damage and range using ``BOT_BLAST_IMPACT_DAMAGE`` and ``BOT_BLAST_IMPACT_RADIUS`` respectively.

Bots also have the ability to transform to a tower. The HP of the transformed tower is proportional to the remaining HP
of the bot that transformed.

A bot can only transform if the tile corresponding to the position it is transforming in is not occupied by any other unit.
For e.g., if a bot issues transform at (2.2, 2.5), there should be no unit of either player lying in the range 
``(2 < x < 3)`` and ``(2 < y < 3)``. The created tower will occupy the tile from ``(2,2)`` to ``(3,3)``.

.. Tip:: You can use ``state.getClosestUnoccupiedOffset(position)`` to get a tile nearby which isn't occupied by any unit.


After blasting / transforming, the bot dies. It is removed from the player state in the next turn.

Towers
--------

Towers can only blast. They have much higher HP and blast power compared to a bot. Similar to bot, on blasting, a tower 
incurs damage to all enemy units nearby and the blast damage decreases as distance to enemy unit increases.

A newly created tower cannot blast for a specific number of turns since its built (``MIN_TOWER_AGE``).

Towers can be strategically placed to defend your flag area.

After blasting, the tower dies. It is removed from the player state after one turn.

Starting the Game
=================

Each player begins with a set numbers of bots ``NUM_BOTS_START`` in their corner of the map ``PLAYER_BOT_POSITIONS[0]``. 

Bots are auto spawned at a given rate every turn at the player's base position.

Bots can move, blast and transform to a tower. Any tile on the map can contain only one tower. Towers are stationary but 
can blast with much higher power than a bot. Check the previous sections for more details regarding bot and tower.

The API we provide is such that you need not worry about which side of the map you are on - it will always appear as if
you are on the top-left corner and the enemy on the bottom right, regardless of if you’re playing as Player 1 or Player 2.

At all times, assume your commands are being carried out as if you are Player 1. If you're playing as Player 2, we'll flip
all of your commands and positions for you.

Goal
====

Every turn the player's ``turn score`` is calculated by the number of bots and towers in flag tiles.

The turn score is calculated as follows,

.. code-block:: python

	turn_score = (num_bots_in_flag_tiles * BOT_SCORE_MULTIPLER) 
		+ (num_towers_in_flag_tiles * TOWER_SCORE_MULTIPLER)

At the end of a turn, the game score of whichever player has higher ``turn score`` is increased by one.

The player with highest score at the end of the game wins!

A Turn
======

Players take turns giving commands to their troops. On each turn, your code's ``Update`` method is called, and you can use the state of the game to make decisions on what you'd like your units to do in that turn.

You can issue issue commands to your bots and towers at every turn in the game.

You can give any commands to any of your units in each turn, but note that each actor can only accept one command. You cannot command 
a bot to blast and transform in the after one for example.

There are a total of ``NUM_TURNS`` turns in a game.

Bot Commands
--------------

You can issue exactly one command to each of your bots in a turn (that is, each bot can do one thing). 
You can either order your bot to

1. Move to a location on the map (``bot.move(location)``), where ``location`` is of type ``DoubleVec2D``
2. Blast at the current position (``bot.blast()``)
3. Move and Blast at a different position (``bot.blast(position)``), where ``position`` is of type ``DoubleVec2D``
4. Transform to a tower in the current tile (``bot.transform()``)
5. Transform to a tower at a different position (``bot.transform(position)``)

Tower Commands
-----------------

You can issue exactly one command to each of your towers in a turn. 
A towers can be ordered to do the following - 

1. Blast at the current position (``tower.blast()``)


Instruction Limit
===================

The number of instructions executed by each player's code per turn is counted while the match is being simulated. This is because
there is a limit on the number of instructions that a player can execute per turn.

There are two instruction limits - a turn limit and a game limit. Crossing the turn limit (10 million instructions) on any turn makes
that particular turn invalid. Crossing the game limit (100 million instructions) totally in the entire game makes the player lose 
the entire match. You can see your instruction count for any turn as the game is executing.

End of the Game
=================

The game ends after running for 1000 turns. The player with the highest score at the end of the game wins. 
If both players have an equal score at the end of 1000 turns, the match is declared a draw.

The game can also end prematurely due to any one player crossing the game instruction limit.

Furthur Reading
===============

Now that you know the rules of the game, you can try writing some code! To better understand the game API, please visit the `Player State <player_state.html>`_ page, which details how to read the game state information and issue commands to your units.
