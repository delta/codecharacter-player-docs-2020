=====
DoubleVec2D
=====

We've written a handy utility class ``DoubleVec2D`` used to represent 2D Vectors (components with floating point X and Y directions).
You can do almost anything you'd do with a normal data type::

	DoubleVec2D a(1, 0);           // a.x = 1, a.y = 0
	DoubleVec2D b(2, 2);

	DoubleVec2D swap_a = DoubleVec2D(a.y, a.x);
							 // Swap x and y into a new DoubleVec2D

	DoubleVec2D c = a + b;         // c is 3, 2
	c = a - b;               // c is -1, -2
	c = a + 5                // Scalar addition, each component is added
	                         // to the scalar, c = 6, 5

	c = a - 5                // Scalar subtraction, c = -4, -5
	c = a * 5                // Scalar multiplication, c = 5, 0

	bool t = a == b          // You can compare vectors, their respective
	                         // components are compared, t = false
	bool t = a != b          // t = true

	double d = a.dot(b)      // Dot product
	d = a.magnitude()        // Magnitude of a, d = sqrt(a.x*a.x + a.y*a.y)
	d = a.distance(b)        // Euclidean distance between a and b

	logr << a;               // Prints "(1, 0)" without the quotes

We also have ``Vec2D``, in case you'd like use some integral points::

	Vec2D x(5, 3)  // You can do anything you could do with Vec2D

	Vec2D x_int = c.to_int() // Round your DoubleVec2D down to a Vec2D

	DoubleVec2D rounded_float = x_int.to_double();
				 // Convert any Vec2D into a DoubleVec2D with ease

These classes are already included in your source file, so prepare yourselves to experience the euphoria of effortless Vector arithmetic :)

Bonus
=====

The DoubleVec2D and Vec2D are simply aliases of the the Vector class, which is a templated type. Vec2D corresponds to `Vector<int64_t>` and DoubleVec2D is `Vector<double>`.

This means that you can do things like ``Vector<uint8_t>(3, 4)``, if for any reason you need

In case you want to use any other numeric type, feel free to do `Vector<your_numeric_type>` but you should be fine using just Vec2D and the occasional DoubleVec2D.
