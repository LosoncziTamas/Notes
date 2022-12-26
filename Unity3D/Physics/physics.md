### "Newton's laws for rotation"

1. With no net torque, angular velocity remains constant.
2. Torque = inertia tensor * angular acceleration.
3. Every torque has an equal and opposite torque.

- The amount of torque depends on the radius.
- Torques are 90 degrees to the spin. 
- Inertia tensor is not a single number it is a matrix. And the units are kg * m^2. Because how hard to rotate something also depends on the radius (a lot). It is a 3x3 matrix where the diagonal part is our main concern.
- The moment of inertia is not strictly a scalar value. It's a rank 2 tensor. (rank 0 - scaler, rank 1 - vector, rank 2 - matrix)
- There is more complexity to something that is spinning to something that's going in a straight line.

### Torque to rigidbodies
- Inertia tensor tells you how much angular acceleration you will get for a given torque.
- One object with having a mass of 100 kg with a scale of 1 and other object having a mass of 20 but having the scale of sqrt(5) will have the same inertia tensor. They'll spin at the same rate.

### Cross product
- Right hand rule applies: first vector - your right thumb, seconds vector - your right index finger, result - right middle finger.
- It is zero if the vectors are parallel.
- a x b = ||a||||b||sin(θ)n
- When it's 90 degrees between a and b then it's the maximum cross product.

### Magnus effect
- Angular velocity crossed the linear velocity gives the Magnus force.

### Inertia tensor
- If something has 3 different symmetries Unity assumes it aligned with the axis of simmetry. It talks about the object's local axes.
- The inertia tensor gives shows how hard to spin it in the given direction.

### Friction
- Physics materials are all about friction (except bounciness)
- Static friction
    - The more you push an object on a surface the more friction pushes back.
    - Then we get to a point where friction can no longer hold the object. This point is the limit of static friction.
    - 0.6 static friction means when we get to the 60% of the contact force then the object will start moving.
- Dynamic/kinetic friction
    -  There is less resistance after the threshold of motion.
    -  The amount of the frictional force.
- Friction combine
    - If two objects with friction touch each other how the friction value is determined.
    - In real world every two object has it's different frictional coefficient.

### Critical angle
- Ciritcal force is the maximum amount of friction a system can bear.
    - This is the static friction (stickiness between the surface) times the contact force.
- Static friction 0.6 means at 60% of the contact force things will start moving. How much will it move is question of dynamic friction.
- Slop angles
    - cirital angle equals to the tangent of the slope angle.
    - if at angle 45 something can tolerate a slope of 45 degrees it has a coefficient of static friction of one tan(45)
- Everything is a little bit stickier in Unity.

### Friction combine
- Friction combine settings determines the friction between two objects.
- What happens on conflict (different combine settings):
    - There is a precedence to what to apply.
- Bounce combine is the same principle.

### Dynamic friction
- How much resistance the movement we have once the thing starts slipping.
- Also relates to the terminal velocity (when no external force and falls at a constant speed).
- How fast the object is sliding depends on the dynamic friction.
- Stribeck curve
    - Tells us how the friction varies with speed.
    - You can change dynamic friction as you go, via script.

### Friction direction
- Doesn't work in Unity.