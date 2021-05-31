# Path Finding

## Units (https://youtu.be/dn1XRIaROM4)
- Units in a game can request paths from the path finding script.
- Then follow the paths along the game.
- Multiple units requesting paths at the same time could cause lags.
- Better to take requests and spread them out.
- Create a `PathRequestManager` that handles the dispatching. It uses a queue for storing and processing the requested paths.
- A request object has a path start, path end, and a callback which is triggered when the path has been determined.
- Update the existing Pathfinding code:
	- Create StartFindPath method
	- Turn FindPath into a Coroutine
	- Make RetracePath return an array of vectors, aka the waypoints.
	- Simplifying the path, only place waypoints wherever the path changes direction. Create SimplfyPath method that does this transformation.
