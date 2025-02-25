Default Code
============

Here's the default code that you're provided on first login, in case you need it again

.. code-block:: cpp

	#include "player_code/player_code.h"
	
	namespace player_code {
	
	using namespace player_state;
	
	State PlayerCode::update(State state) {
	
	    // Hello and Welcome to Code Character 2020!!
	
	    // Code Character is a strategy code writing competition, where you'll be
	    // controlling some robots that will face off against each other
	
	    // We have two kinds of units, bots and towers. You start out only
	    // with some bots though. Let's print some information about your
	    // bots.
	    logr << "Number of bots: " << state.bots.size() << endl;
	
	    // logr is like cout. Use it to print information to your console on the
	    // panel to the bottom right
	
	    // As you can see, we have an object called state, that contains some info
	
	    // State has a vector named bots, that contains information about all
	    // our bots.
	
	    // We can give commands to our bots to have them perform actions.
	    // Remember to check if your bot is alive before you make it perform
	    // anything!
	
	    // Bots can move to any reachable location on the map.
	    // Let's make bots move to all flag locations
	
	    size_t used_bots = 0;
	
	    // Range based loops are convenient to use. We use a reference to
	    // ensure that our changes are reflected and not made on a copy!
	    // You can use auto instead of an explicit type
	    for (auto &bot : state.bots) {
	        // Let's not use up all our bots just for these. So, we will only use a
	        // maximum of 18 bots here.
	        if (used_bots > state.flag_offsets.size() || used_bots >= 18)
	            break;

	        // Make sure that you do not access a vector beyond it's size or else,
	        // you'll get a segmentation fault and cost you the entire game
	
	        // State has a vector that has the locations of flag locations on the
	        // map. A bot asked to move to a location will automatically go
	        // there using the shortest path found.
	        bot.move(state.flag_offsets[used_bots]);
	        used_bots++;
	    }
	
	    // Let's say you want one of the remaining bots to blast at the spawn
	    // position of the enemies.
	
	    // Before that we ensure that we have atleast one extra bot left
	    if (state.bots.size() > used_bots) {
	        // See the usage of constant value PLAYER_BASE_POSITIONS
	        state.bots[used_bots].blast(PLAYER_BASE_POSITIONS[1]);
	        used_bots++;
	    }
	
	    // Other than blasting and moving, bots can also transform into towers
	    // Let's try to transform all of the remaining bots near the other end of
	    // the map i.e., (MAP_SIZE - 1, MAP_SIZE - 1) Note that you cannot construct or
	    // / move to coordinates where either x = MAP_SIZE or y = MAP_SIZE
	    int x = MAP_SIZE - 2, y = MAP_SIZE - 2;
	
	    // The bots can also be traversed like a usual array using an index
	    for (size_t i = used_bots; i < state.bots.size(); i++) {
	        if (y != (MAP_SIZE - 1)) {
	
	            // You can use the DoubleVec2D class to define positions and
	            // distances
	            state.bots[i].transform(DoubleVec2D(x, y));
	            x--;
	            y++;
	        }
	    }
	
	    // State also has the vector of player towers
	    // We'll blast all towers once they are constructed
	    for (auto &tower : state.towers) {
	        tower.blast();
	    }
	
	    // We will also check on every turn how many of the opponent towers are left
	
	    // Note that the state is not updated yet. It is not your usual program.
	    // For this entire turn, your state values will be the same, the changes are
	    // only reflected the next turn this update function runs.
	    logr << "Number of enemy towers: " << state.enemy_towers.size() << endl;
	
	    // While this should give you a decent start, we highly recommend
	    // reading the docs provided. It should get you up to date with
	    // all there is to State and we have some helper snippets and methods
	    // so you start competing right away!
	
	    return state;
	}
	
	}
