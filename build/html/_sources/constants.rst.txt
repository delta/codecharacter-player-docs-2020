=========
Constants
=========

These are the magic constants that affect the game logic. You are free to access any of these constants from your code - they are in global scope.

Map Constants
================

You'll probably be using this constant frequently while writing code.

.. cpp:var:: long MAP_SIZE = 30

	Number of tiles along the side of the map.


Bot Constants
=============

.. cpp:var:: long MAX_BOT_HP = 200

	Maximum HP for a bot.

.. cpp:var:: long MAX_NUM_BOTS = 250

	Maximum number of bots per player.

.. cpp:var:: long NUM_BOTS_START = 20

	Number of bots the player begins the game with.

.. cpp:var:: long BOT_SPEED = 6

	Units of distance the bot covers per turn.

.. cpp:var:: long BOT_BLAST_IMPACT_RADIUS = 2

	The maximum distance in tiles around the bot when on blasting the bot inflicts damage

.. cpp:var:: long BOT_BLAST_DAMAGE_POINTS = 50

	The maximum damage a bot can inflict on an enemy unit on blasting

.. cpp:var:: long BOT_SPAWN_FREQUENCY = 1

	Number of bots spawned per turn.

Tower Constants
===============

.. cpp:var:: long MAX_TOWER_HP = 600

	Maximum HP for a tower.

.. cpp:var:: long MAX_NUM_TOWERS = 150

	Maximum number of towers per player.

.. cpp:var:: long TOWER_BLAST_IMPACT_RADIUS = 6

	The maximum distance in tiles around the tower when on blasting the tower inflicts damage

.. cpp:var:: long TOWER_BLAST_DAMAGE_POINTS = 500

	The maximum damage a tower can inflict on an enemy unit on blasting

.. cpp:var:: long TOWER_MIN_BLAST_AGE = 3

	The minimum number of turns a tower must be alive to blast
