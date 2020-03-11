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

`Download link <https://code.pragyan.org/binaries/>`_  or 
`Github releases <https://github.com/delta/codecharacter-cli/releases/tag/1.0.0>`_

Linux and Darwin
^^^^^^^^^^^^^^^^^^
.. code-block:: bash

	chmod +x path/to/binary


Usage
-------------------

.. code-block:: bash

	path/to/binary [ -p PORT ] [ -m MAP_FILE ] PLAYER_1_CODE PLAYER_2_CODE

This will compile, execute the player code and serve it in the port given by user (Default: 3000)


Checking logs
-------------------

Player logs can be viewed in browser console
