# Multithreading
- Unity does support multithreading. There are just caveats to it.

## Coroutines
- Coroutines are fake multithreading.
  - They do not involve multithreading.
  - They return IEnumerator.
  - They can end execution of a function for one frame (by using `yield return`)
  - Corooutines are cooperative multitasking. They are willingly give up running. 
- Debug.Log is flushed at the end of a frame.

## Threading
- The advantage of a separate thread is that you are offloading the job of balancing the work between your main thread and your secondary thread (where is the job is running).
- The physics engine runs in a separate thread.
  - That's why we have a `FixedUpdate()` event function which runs according to this background thread.
  - This is where you synchronize between the main and the physics thread.
- Whenever you create another thread it should be used for a specific task. It should be a task that can be isolated.
- You can create a thread by passing the name of the function ```Thread myThread = new Thread(SlowJob)```
- Here a `SlowJob` is a variable that contains some function code.
- You start the thread by ```myThread.Start()```
- 