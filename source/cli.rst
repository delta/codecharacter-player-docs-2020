CLI
========

.. Hello, and welcome to the player documentation for Code Character!

.. Code Character is a strategy programming game where you control troops in a turn-based game with code you write -
.. in our favorite language C++ :)

.. Let’s get started with a quick tutorial on how to get started.


Prerequisites
-------------------
- Docker


Installation
-------------------

`Download link <https://code.pragyan.org/binaries/>`_

Linux and Darwin
^^^^^^^^^^^^^^^^^^
.. code-block:: bash

	chmod +x path/to/binary


Usage
-------------------

.. note :: The first time you execute the CLI, docker images of size above 1GB are downloaded.

.. code-block:: cpp

	bash path/to/binary PLAYER_1_CODE.cpp PLAYER_2_CODE.cpp

This will compile, execute the player code and serve it in the port given by user (Default: 3000)

You can pass a custom map as well,

.. code-block:: cpp

	bash path/to/binary -m map.txt PLAYER_1_CODE.cpp PLAYER_2_CODE.cpp

The map file is 30 X 30 with one of three characters, L (land), W (water) or F (flag).
An example with a 5 X 5 Map would be,

.. code-block:: cpp

	L L L L L

	L W F W L

	L W F W L

	L W F W L

	L L L L L

Note: End the file with a newline

To render a game which has been executed already,

.. code-block:: bash

        path/to/binary -r [ -p PORT ] LOG_DIRECTORY 

This will render the game log files in LOG_DIRECTORY
Given directory must contain the following files : game.log, player_1.dlog, player_2.dlog


Checking logs
-------------------

When the renderer is running in browser, player logs can be viewed in browser console.
