============
Player State
============

This page documents almost everything you'll need to write code.

It contains every data structure we use to describe the state of the game.

If you haven't already, you might want to go through the game `overview <overview.html>`_ first, to understand the core game and the various elements that you can control. Once you've got a hang of the game, this page will help you dive deeper into your code.

You might want to checkout `DoubleVec2D <doublevec2d.html>`_ as Dwell.

State
=====

.. cpp:class:: State

	This class represents the state of the game. You are given an instance of this class every round in a parameter called `state`, and you use it to tell the runtime what move you want your units to perform. You have access to view your own units, your opponent units, and the game map.


	.. cpp:member:: array<array<TerrainType, MAP_SIZE>, MAP_SIZE> map

		The map is a 2D array of `TerrainType` elements, which tell you if a particular type is **LAND**, **WATER**, or a **FLAG**. Remember, your units are not Jesus, and can't walk on Water.

		If you're unfamiliar with `std::array` in C++, simply use map like a standard 2D array in C.

		For example, to check if `(4, 5)` is not water, you could do ::

			if (state.map[4][5] != TerrainType::WATER) {
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

	.. cpp:member:: vector<Vec2D> flag_positions

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

	.. cpp:member:: SoldierState state

		The current state of the soldier. This member tells you what the soldier is doing right now, and has values **IDLE**, 
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

			state.bots[0].blast()

	.. cpp:function:: transform()

		Transform the bot at current position. A bot can transform only if,
		 
		 * The current tile bot is in has no other units in it.
		 * The current tile is not one of the spawn tiles of the players.

		 If any of the above cases fails, the bot switches to IDLE state.

		For example, transforming the first three bots ::

			state.bots[0].tower({1.2, 3.1})
			state.bots[0].tower({3.3, 4.7})
			state.bots[0].tower({4.2, 4.9})

	.. cpp:function:: transform(DoubleVec2D transform_position)

		Transform the bot at ``transform_position``. The bot moves to blast_position and then blasts. A bot can move and transform 
		only if,
		 
		 * The destination tile, which transform_position belongs to, is not one of the spawn tiles of the players
		 * The destination tile is reachable

		 If any of the above cases fails, the bot switches to IDLE state.

		NOTE: The bot does not move and transform in the same turn. If it reaches the transform_position in one turn, it transforms
		only in the next turn.

		After reaching the transform_position, the bot transforms only if that tile has no other units in it. Else, it switches to
		IDLE state.

		For example, blasting the first bot ::

			auto &bot = state.bots.front();

			bot.blast({15, 15});


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

Helpers
=======

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
