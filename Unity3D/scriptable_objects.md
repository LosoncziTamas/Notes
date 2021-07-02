# Overthrowing the MonoBehaviour Tyranny

## The MonoBehaviour tyranny

### MonoBehaviours
- Scripts attached to game objects.
- Live in scenes or prefabs.
- Store the state of the game.
- Get callbacks from Unity.

### Problems
- Shared vs. non-shared state.
	- There are data which are shared across multiple objects and you don't want them to be different, a consistent shared bit of information.
	- The problems rises when all these kind of infomation is combined into mono behaviours.
- Any change you make in play mode is lost when you exit.
- Monobehaviours have sub-file granularity.
	- When you have a MonoBehaviour in a file, it's not the only thing in the file (ex.: scenes have other obects, prefabs have bunch components, etc.)
	- It makes collaboration hard. Merge conflicts.
- Callback chaos
	- Hard to keep track of what is happening at any given frame.
	- Which code is present in the scene, which code is being executed.

### Uninstantiated prefabs
- Prefabs with monobehaviours but not put in the scene.
	- Doesn't get callbacks.
	- Can be a place to store shared states.
- Kind of works, but it abuses the Prefab concept.
- Accident-prone & can be confusing.
- Still has sub-file granularity.

### C# statics
- Static variable where you store your shared information.
- No serialisation, no visualization (loading, saving information)
- Get lost on domain reload (ex.: recompiling in play mode the static variables are wiped out).

### ScriptableObject
- On the engine side it is represented the same way as a MonoBehaviour, it just not attached to a game object.
- Custom class that doesn't have to be attached to a GO, doesn't have to live in the scene.
- Cannot be attached to a GO.
- Doesn't get callbacks.
	- Can simplify things (ex.: debugging)
- Can be serialised and inspected.

### How SO saves us pain
- Encourages separation of shared and non-shared states.
- Doesn't live in the scene, so doesn't get reset.
	- You can tweak parameteres during play mode.
- You can have control over sub-file granularity, ex.: ene object per file.
- No callbacks, no chaos.

### How to declare & reference

```Csharp
public class MySO : UnityEngine.ScriptableObject
{
	public int someVar;
}
```

```Csharp
public MySO mySOInstance;
```

### How to create
- You can create them in-memory with `ScriptableObject.CreateInstance<MySO>()`
	- It's kind a like an 'add component'
	- It's the same as calling someting like `new Material()`
	- No constructor
- You can bind them to .asset files in the editor
	- `AssetDatabase.CreateAsset()` or `AssetDatabase.AddObjectToAsset()`
	- You can pack these, that's how you have control.
- You can also use `[CreateAssetMenu]` makes instance & binds to file.

### SO Callback
- `OnEnable()` called when first created, loaded, after domain reload,
- `OnDisable()` called before being destroyed, shutting down for assembly reload
- `OnDisable()` called when destroyed

### SO Lifecycle
- Same as any other asset.
	- Stay alive as long as you use it.
- Resources.UnloadUnusedAssets()
	- You can have a control over that.

### Destroy()/DestroyImmediate()
- Unity is not a .NET Engine. It's a C++ engine with a .NET layer.
	- All objects have a C++ representation.
	- All SO objects have a managed side & native side.
		- C++ takes part of identity, serialization.
- When you call `Destroy()` on an SO it kills the C++ part. You cannot kill the C# part.
- Managed part still kept around.
- When you are destroying things also null them out. So the reference get's cleaned up as well.

## Patterns

### Data Objects and Tables
- Plain data-holding class bound to a .asset file.

### Extendable Enums
- Empty scriptable objects bound to asset files.
- You can compare empty objects, use them as dictionary keys.
- Natural progression to data objects. Going from empty to non-empty objects.
- You cannot add extra data to enums.
- Supports null.

### Dual Serialisation
- Supported by JsonUtility.

```Csharp
class LevelLayout: ScriptableObject
{
	public Vector2[] platformPositions;
	public Vector2[] enemyPositions;
	public Vector2[] coinPositions;
	public Vector2 startPosition;
}
```
```Csharp

```
- 




