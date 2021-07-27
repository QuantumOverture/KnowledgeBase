# Godot 🤖

⌚ Last Updated on: 07/26/2021

***
### Godot Basics
- Godot is a game engine: an application that facilitates in the creation of games with its suite of tools.
- The most basic building block or unit in Godot is a `Node`. They can have:
	- Names.
	- Properties that can be modified.
	- Be given functions that add behavior.
	- Can be added/connected to another node as a child or parent.
	- Receive a call to perform an action/process every frame (i.e screen update).
- The next abstraction up the list are `Scenes`. They are a group of hierarchically ordered (in order is a the form of a tree of nodes) set of nodes. To run a game in Godot means to be **running a scene**. There can be lots of scenes but a game needs at least one main scene to start.
	- A scene always has a root node.
	- It can be saved to the disk(called `Packed scenes`) and loaded back in RAM.
	- Can be instanced:
		- Scenes can be treated like blueprints or classes. You can make copies of these scenes known as `instances` in other scenes. You can modify these instances individually or as a group.
- When making Godot games, it is recommended to forgo any sort of rigid design pattern. Make games in the most natural way possible. Start by focusing on visual elements -> create a diagram where all these elements are possibly interlinked and connections show "ownership" or inheritance properties -> create a scene for each node on your diagram.  
- `Scripts` are things that can be added to nodes to give them behavior (this is where "coding" comes into play). The script inherits all the functions provided by the node (e.g if you have a 2dnode then you might get functions to change around various properties that are specific to that node). 
- `Signals` are emitted when an action occurs and can be connected to any function for any script instance (which in turn change the behavior of nodes). They are mostly used in GUI nodes and you can make your own custom signals.

### The Godot IDE/Editor
- Change the size of the viewable window your game: After creating anew project, click the `Project` tab then `Project Settings` then search for `Display` in the menu. Finally, set `Width` & `Height` to whatever suites your game.
	- In the case of a 2D game, also set `Mode` to 2d and `Aspect` to keep to keep a consistent game window size across different monitors.
- For bigger projects it is recommended to keep game files in different folders.

***

### 🥽🥼 Resources Used:

- https://docs.godotengine.org/en/stable/getting_started/step_by_step/your_first_game.html (*Your first game* by `Juan Linietsky, Ariel Manzur and the Godot community`)
-	https://docs.godotengine.org/en/stable/getting_started/step_by_step/scenes_and_nodes.html#doc-scenes-and-nodes (*Scenes and nodes*  by `Juan Linietsky, Ariel Manzur and the Godot community`)
-	https://docs.godotengine.org/en/stable/getting_started/step_by_step/instancing.html (*Instancing* by `Juan Linietsky, Ariel Manzur and the Godot community`)
-	https://docs.godotengine.org/en/stable/getting_started/step_by_step/instancing_continued.html (*Instancing (continued)* by `Juan Linietsky, Ariel Manzur and the Godot community`)
-	https://docs.godotengine.org/en/stable/getting_started/step_by_step/scripting.html (*Scripting* by `Juan Linietsky, Ariel Manzur and the Godot community` )
