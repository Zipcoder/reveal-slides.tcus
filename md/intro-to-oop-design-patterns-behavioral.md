
-
## Behavioral Patterns: Observer
* Register observer
* Remove observer
* Notify observer on state change


-
## Behavioral Patterns: Command
* Command interface
	* Decoupling "what is done" from "when it is done"
	* Concrete commands objects are tasks; i.e. - `UNDO_TASK`

-
## Behavioral Patterns: Command
You'll see command being used a lot when you need to have multiple undo operations, where a stack of the recently executed commands are maintained. To implement the undo, all you need to do is get the last Command in the stack and execute it's undo() method.

You'll also find Command useful for wizards, progress bars, GUI buttons and menu actions, and other transactional behavior.  
