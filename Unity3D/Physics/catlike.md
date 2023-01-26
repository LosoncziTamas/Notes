### Rigidbody

There are two general ways to approach controlling a character in combination with a physics engine. First is the rigidbody approach, which is to have the character behave like a regular physics object while indirectly controlling it, either by applying forces or changing its velocity. Second is the kinematic approach, which is to have direct control while only querying the physics engine to perform custom collision detection.

If we want to use the physics engine then we should let it control the position of our sphere. Adjusting the position directly would effectively be teleporting.

We have to indirectly control the sphere, either by applying forces to it or by adjusting its velocity.

PhysX doesn't prevent collisions, it instead detects them after they happened and then moves rigidbodies so they're no longer intersecting

If movement is really fast then the sphere might end up passing through the wall entirely or get depenetrated toward the other side, which is more likely with a thin wall. You can prevent this by changing the Collision Detection mode of the Rigidbody, but that's usually only needed when moving really fast.

When FixedUpdate gets invoked Time.deltaTime is equal to Time.fixedDeltaTime.

Setting the Rigidbody's `Interpolate` to Interpolate makes it linearly interpolate between its last and current position, so it will lag a bit behind its actual position according to PhysX. The other option is Extrapolate, which interpolates to its guessed position according to its velocity, which is only really acceptable for objects that have a mostly constant velocity.

Each physics step begins with invoking all FixedUpdate methods, after which PhysX does its thing, and at the end the collision methods get invoked.

### Dot product, vector projection

- The length of projection (w) times the length of u is the dot product of u and v.
    - |w| = |v|cos(angle)
    - u * v = |u||v|cos(angle)
    - u * v = |u||w|
    - |w| = (u * v)/|u|
- You can use the dot product to calculate the length of a vector and for many other things:
    - a dot b = a1*b1 + a2*b2
    - a dot b = |a|*|b|*cos(a)
    - |a| = sqrt(a dot a)
- You can use the dot product to project a vector over another (unit length) vector. This allows us to create a new coordinates (non x and y) 
vector dot unit axes = coordinate
- This comes from the vector projection formula:
    - (v dot n) / ( ||n||^2) * n
- To reflect vectors we are using the following formula:
    - v’ = v - 2 * v dot r * r
    - Where r is a unit length vector, the norm of the surface (wall), v is the vector we want to reflect. 
