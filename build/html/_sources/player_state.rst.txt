============
Player State
============

This page documents almost everything you'll need to write code.

It contains every data structure we use to describe the state of the game.

If you haven't already, you might want to go through the game `overview <overview.html>`_ first, to understand the core game and the various elements that you can control. Once you've got a hang of the game, this page will help you dive deeper into your code.

You might want to checkout `DoubleVec2D <doublevec2d.html>`_ as well.

State
=====

.. cpp:class:: State

	This class represents the state of the game. You are given an instance of this class every round in a parameter called `state`, and you use it to tell the runtime what move you want your units to perform. You have access to view your own units, your opponent units, and the game map.


	.. cpp:member:: array<array<TerrainType, MAP_SIZE>, MAP_SIZE> map

		The map is a 2D array of `TerrainType` elements, which tell you if a particular type is **LAND**, **WATER**, or a **FLAG**. Remember, your units are not Jesus, and can't walk on Water.

		If you're unfamiliar with `std::array` in C++, simply use map like a standard 2D array in C.

		For example, to check if `(4, 5)` is not water, you could do ::

			if (state.map[4][5].type != TerrainType::WATER) {
				...
			}

		You can still go through the entire map using a normal or range based for-loop ::

			for (auto &row : state.map) {
				for (auto &element : row) {
					// Do something with element
				}
			}

	.. cpp:member:: vector<Bot> bots

		A vector of your own bots. Check down below for more information about the `Bot` object.

	.. cpp:member:: vector<Bot> enemy_bots

		A vector of the opponent's bots. Remember, you shouldn't modify this data, and you can't perform moves on an opponent unit.

	.. cpp:member:: int64_t num_bots

		Number of bots.

	.. cpp:member:: int64_t num_enemy_bots

		Number of enemy bots.

	.. cpp:member:: int64_t score

		Integer containing your current game score. You can only see your own score, and can't peek at your opponent's!

	.. cpp:member:: vector<Tower> towers

		A vector of your own towers. Check down below for more information about the `Tower` object.

	.. cpp:member:: vector<Tower> enemy_towers

		A vector of the opponent's towers. Remember, you shouldn't modify this data, and you can't perform moves on an opponent unit.

	.. cpp:member:: int64_t num_towers

		Number of towers.

	.. cpp:member:: int64_t num_enemy_towers

		Number of enemy towers.

		A vector of the opponent's towers. Remember, you shouldn't modify this data, and you can't perform moves on an opponent unit.

	.. cpp:member:: vector<Vec2D> flag_offsets

		A vector containing the center of offsets for the flag tiles on the map. You can iterate through the map and check for `TerrainType::FLAG`, or you can use this vector instead.

		For example, to make the first two bots each transform at the first two flag locations on the map, you would do ::

			state.bots[0].transform(flag_offsets[0]);
			state.villagers[1].transform(flag_offsets[1]);

	.. cpp:member:: int64_t score

		Integer containing your current game score. You can only see your own score, and can't peek at your opponent's!


Inside the `state` object, we have vectors of bots and towers as shown above. We'll continue with a description of each of those classes.

Bot
=======

.. cpp:class:: Bot

	The Bot is a mobile unit. Use your bot to build towers of your own and destroy the opponents.

	.. cpp:member:: int64_t id

		A unique ID associated with this bot. It will never change.

	.. cpp:member:: DoubleVec2D position

		The bot's current position on the map.

	.. cpp:member:: int64_t hp

		The bot's curent HP. Note that the max value of hp can be accessed from `MAX_BOT_HP`.

	.. cpp:member:: BotState state

		The current state of the bot. This member tells you what the soldier is doing right now, and has values **IDLE**, 
		**BLAST**, **TRANSFORM**, **ATTACK** AND **DEAD**.

		For example, to check for all your bots who are currently moving, you could do ::

			for (auto &bot : state.bots) {
				if (bot.state == BotState::MOVE) {
					// Do something
				}
			}

	.. cpp:function:: move(DoubleVec2D destination)

		Specify a position to which the bot should move. Note that you don't need to keep moving your bot or find a path.
		The engine handles path finding for you. Simply tell the bot where to go!

		For example, let's say you want the first bot to move to the position where the first enemy bot is ::

			DoubleVec2D first_enemy_bot_pos = state.enemy_bots[0].position;

			state.bots[0].move( first_enemy_bot_pos );

	.. cpp:function:: blast()

		Blast the bot at current position. A bot can blast anytime and anywhere in the map it can reach.

		For example, blasting the first bot ::

			state.bots[0].blast()

	.. cpp:function:: blast(DoubleVec2D blast_position)

		Blast the bot at ``blast_position``. The bot moves to blast_position and then blasts. If the position is not
		reachable, the bot switches to IDLE state and does not move from current position.

		NOTE: The bot does not move and blast in the same turn. If it reaches the blast_position in one turn, it blasts
		only in the next turn.

		For example, blasting the first bot ::

			auto &bot = state.bots.front();

			bot.blast({15, 15});

	.. cpp:function:: transform()

		Transform the bot at current position. A bot can transform only if,
		 
		 * The current tile bot is in has no other units in it.
		 * The current tile is not one of the spawn tiles of the players.

		 If any of the above cases fails, the bot stays in TRANSFORM until the conditions are satisfied.

		For example, transforming the first three bots ::

			state.bots[0].transform()
			state.bots[1].transform()
			state.bots[2].transform()

	.. cpp:function:: transform(DoubleVec2D transform_position)

		Transform the bot at ``transform_position``. The bot moves to blast_position and then blasts. A bot can move and transform 
		only if,
		 
		 * The destination tile, which transform_position belongs to, is not one of the spawn tiles of the players
		 * The destination tile is reachable

		If any of the above cases fails, the bot stays in TRANSFORM until the conditions are satisfied.
		
		NOTE: The bot does not move and transform in the same turn. If it reaches the transform_position in one turn, it transforms
		only in the next turn.

		After reaching the transform_position, the bot transforms only if that tile has no other units in it. Else, it switches to
		IDLE state.

		For example, transforming the first three bots ::

			state.bots[0].transform({1.2, 3.1})
			state.bots[1].transform({3.3, 4.7})
			state.bots[2].transform({4.2, 4.9})


Tower
========

.. cpp:class:: Tower

	Tower is a stationary strategic unit. A sequence of towers can block bots from passing through and acts as a defense unit.
	The tower's blast is also much powerful than a bot's blast.

	.. cpp:member:: int64_t id

		A unique ID associated with this tower. It will never change.

	.. cpp:member:: DoubleVec2D position

		The tower's current position on the map.

	.. cpp:member:: int64_t hp

		The tower's curent HP. Note that the max value of hp can be accessed from `MAX_TOWER_HP`.

	.. cpp:member:: TowerState state

		The current state of the tower. This member tells you what the tower is doing right now, and has values **IDLE**, **BLAST**, **DEAD**.

		For example, to check for all your towers who are idle, you could do ::

			for (auto &tower : state.towers) {
				if (tower.state == TowerState::IDLE) {
					// Do something
				}
			}

	.. cpp:function:: blast()

		Blast the tower. The tower cannot blast until it has attained a specific age ``TOWER_MIN_BLAST_AGE``

		For example, blasting all towers ::

			for (auto &tower : state.towers) {
				tower.blast()
			}

Logging Variables
=================

Note that *everything* shown above is printable. You can log any of this to output and view it in your game log. (Remember to use ``logr`` and not ``cout``!)

For example, if you want to print out the properties of a particular Villager, you could do::

	logr << state.bots[0] << '\n';

*Output* ::

	Bot(id: 2) {
		position: (5.0, 5.0)
		hp: 80
		state: IDLE
	}



You can even log the entire `state` variable. Keep in mind, your output will be quite large.

Helpers
=======
Along with the game state, we’ve included some helper methods to make it easier for you to implement your logic.

Something useful for you when implementing your logic would be to find the nearest flag position from any given position.

.. cpp:function:: findNearestFlagPosition( const State &state, DoubleVec2D position)

Returns the nearest flag position from a given position. This function can be used by bots to find the closest flag to which they 
can move to ::

	auto &bot = state.bots[0];
	auto nearest_flag = findNearestFlagPosition(state, bot.position);

	// Bot finds the nearest flag and moves into the flag area
	bot.move(nearest_flag)

.. cpp:function::  getBotById(State &state, int64_t bot_id)

Returns the corresponding ``Bot`` by reference when given the bot's id. This comes in handy when assigning different bots different 
tasks and keeping track of their progress

It can be used in the following way ::

	auto &bot_blast = getBotById(state, 1); // Returns a reference to bot with actor id 1
	bot_blast.blast(DoubleVec2D(10, 10)); // Making this bot blast

	auto &bot_transform = getBotById(state, 2); // Reference to bot with id 2
	bot_transform.transform(DoubleVec2D(3, 7)); // Making this bot move to position (3, 7) and transform three

	auto &bot_move = getBotById(state, 3); // Bot with actor id 3
	bot_move.move(DoubleVec2D(5, 5)); // Making this bot move into a specific position like a flag

.. note:: Returns ``Bot::null`` if no bot exists with the given bot id. ``Bot::null`` is basically a bot constructed with an id of
 ``-1``

.. cpp:function:: getTowerById(State &state, int64_t tower_id)

Returns the corresponding ``Tower`` by reference when given the tower's id. It can be used in the following way ::  

	auto &tower_blast = getTowerById(state, 12); // Returns reference to tower with actor id 12
	tower_blast.blast();

.. note :: Returns ``Tower::null`` if no tower exists with the given tower id . ``Tower::null`` is a tower constructed with an id of ``-1``

Bonus
======

.. cpp:function:: findNearestOffset(const State &state, Vec2D position, std::function<bool(TerrainType type, uint64_t position_count)>)

A general purpose function with which you can find the nearest target position from the source position which satisfies a condition 
defined by you. 

Each offset in the map will be checked against this function which is passed the terrain type of that offset and the total
number of bots or towers in that offset  

It may look a bit scary but can be utilized using this function using C++ Lambda functions. Let us look at an example where we find 
the nearest position where there are no bots or towers ::

	auto nearest_desolate_position = [](TerrainType terrain, uint64_t actor_count){
		// Returns any position with actor count of 0 irrespective of terrain
		return (actor_count == 0); 
	}
