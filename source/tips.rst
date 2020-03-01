============
Tips and Tricks
============

Here's some more stuff that you might find useful, not covered in the other sections


Persisting Variables
--------------------

If you need a variable that retains it's value over multiple turns, you can either make it a global variable, outside your update function, or make it a ``static`` variable.

For example, a common requirement is to know what the current turn is. Let's do this two different ways:

**Global Variable** ::

	int turn = 0;
	
	State PlayerCode::update(State state) {
		turn++;
		logr << "Turn : " << turn << std::endl;
	}

**Static Variable** ::

	State PlayerCode::update(State state) {
		static int turn = 0;
		turn++;
		logr << "Turn : " << turn << std::endl;
	}

Use the Standard Library!
-------------------------

The C++ Standard Library is immense and powerful. Use it to your advantage! Just make sure you ``#include`` the right headers. Anything upto C++14 is available.

Let's say for whatever reason, you'd like to sort your bots by hp. Since ``state.bots`` is a ``std::vector``, we can use ``std::sort`` with a custom comparator to achieve this. ::

	// Remember to make a COPY of the bots. Don't mess up the original state!
	auto my_bots = state.bots;

	// Use a custom comparator lambda to sort by HP
	std::sort(my_bots.begin(), my_bots.end(), 
		[](const auto &a, const auto &b) -> bool { 
			return a.hp < b.hp; 
		}
	); 

	// Log the bots
	for (auto bot : my_bots) {
		logr << bot << endl;
	}

DoubleVec2D has several methods
-------------------------

``DoubleVec2D`` has several common operations, like calculating distance and scalar multiplication programmed in. Use these methods to avoid writing more code.::

	DoubleVec2D position1(3.0, 5.1);
	DoubleVec2D position2(4.11, 23.9);

	double distance = position1.distance(position2);

Check out the `DoubleVec2D <doublevec2d.html>`_ page if you haven't already.
