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
=======
=================
Command Line Tool
=================

We are providing you with a command line tool to run your code locally and check the results, instead of having
to wait in the queue for your processes.


Prerequisites
=============

You need to have Docker CE installed

The entire download takes around 1.5GB of data

Installation
============

Download the binary for your architecture from here:

.. _Executables: https://code.pragyan.org/binaries/

Upon downloading, run ``chmod +x codecharacter`` to make it an executable.


Running
=======

``./codecharacter player_code_1.cpp player_code_2.cpp``
Here ``player_code_1.cpp`` and ``player_code_2.cpp`` are the codes of two players.

By default, the executable has a map. You can change it by passing a map file

``./codecharacter player_code_1.cpp player_code_2.cpp -m map.txt``

map.txt contains 30 X 30 grid with each character space separated.
