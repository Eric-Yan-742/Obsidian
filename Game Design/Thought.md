- When aligning two objects, use 3D perspective mode to move the object to the about right position. Then try to use the scene view gizmos and isometric view to align the object in different perspectives. Top, left, right, back, front, bottom. 
- Variant and the actual object are two different things?
	- ![[Pasted image 20241229104231.png]]
	- ![[Pasted image 20241229104239.png]]
	- The actual middle tube is in the right orientation, but the "middle tube variant" is not. 
	- ![[Pasted image 20241229104253.png]]
	- After I click the -> arrow, the variant is actually elsewhere. 
- You can use local coordinate to rotate the object to the right orientation. Its local y-coordinate should point straight up. 
- A **collider** is a component that defines the shape of an object for the purpose of physical collisions and interactions within the environment. Without a collider, an object has no physical boundaries and other objects can go right through it — like a cloud. 
- With a Rigidbody component, objects will have mass and respond to gravity.
- RigidBody 2D: Difference between mass and linear/angular damping
	- Mass: Mass is about how resistant the object is to changes in its motion (inertia).
	- Damping: Damping determines how quickly velocity (linear or angular) decreases over time due to resistance
	- Why Unity treats them differently: 
		1. A light object (e.g., a feather) can have high damping to simulate air resistance.
		2. A heavy object (e.g., a bowling ball) can have low damping to allow it to roll smoothly.
- Duration of a particle effect
	- ![[Pasted image 20250129205139.png]]