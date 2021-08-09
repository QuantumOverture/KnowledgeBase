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
- A scene's root node should reflect the object's desired functionality - what the object is (you need to create this when making a new scene).
	- An Area2D node will allow us to detect objects that overlap or run into entites in our game. (from the official doc: 2D area that detects CollisionObject2D nodes overlapping, entering, or exiting.)
	- An AnimatedSprite node can be added as a child and will handle the appearance abd animations for our player. It requires a SpriteFrames resource which is a list of sprites it can display.
		- Inspect the sprite node and click the Frames attribute and add new sprite frames(then click it again to show the SpriteFrame panel. You can add animations and sprite images here.
	- Add CollisionShape2D to add hitboxes to parent nodes (click shape and select the most suitable shape).
	
- Double clicking a node will allow you to change its name.
- Make sure to click the "the object's children are not selectable" when working(i.e adding child nodes) with nodes in a scene. This is to avoid accidentally moving and resizing things.

- You can add scripts to nodes via a scroll-like button near the search bar above the scene's node tree.
	- The export keyword along with the variable declaration will allow us to view it in the node inspector.
	- The `_ready()` function gets run when the node first enters the scene tree.
	- Use `get_viewport_rect().size` to get the current game window size.
	- The `_process()` function is called every frame, so we use it to update elements of our game(we also check for input here).
		- Input actions are deifned in the Project Settings under "input map". You can define custom events and assign differnt keys,mouse events and other inputs.
		- You can detect key presses via `Input.is_action_pressed([KEY HERE])` which returns a boolean depending if the input event occured or not.
	- The `Vector2()` in a 2D game will allow you to change the node's movement vector(note not the *position* vector).
		- Doing the following will allow you normalize the vector(set its value to 1) and adjust the movement via the speed parameter. This also stops super fast diagnol motion: `velocity.normalized() * speed` (where velocity is the velocity vector).
	- `$NAME_OF_NODE` is a shorthand for `get_node(NAME_OF_NODE)`. It returns a node at the relative path from the current node or null if the node cannot be found.
		- For example to play animations: `$AnimatedSprite.play()`.
	- Remember that position/displacement equals velocity \* time -> once you have made a velocity vector and added the appropriate velocity : `position += velocity * delta`. `delta` is the time elapsed between frames in `_process()`
		- Also remember to clamp(i.e create a fixed range) the target node to stop it from leaving the screen: e.g `clamp(position.x, 0, screen_size.x)` 
	- To test your current scene -> press F6
	
***

### 🥽🥼 Resources Used:

- https://docs.godotengine.org/en/stable/getting_started/step_by_step/your_first_game.html (*Your first game* by `Juan Linietsky, Ariel Manzur and the Godot community`)
-	https://docs.godotengine.org/en/stable/getting_started/step_by_step/scenes_and_nodes.html#doc-scenes-and-nodes (*Scenes and nodes*  by `Juan Linietsky, Ariel Manzur and the Godot community`)
-	https://docs.godotengine.org/en/stable/getting_started/step_by_step/instancing.html (*Instancing* by `Juan Linietsky, Ariel Manzur and the Godot community`)
-	https://docs.godotengine.org/en/stable/getting_started/step_by_step/instancing_continued.html (*Instancing (continued)* by `Juan Linietsky, Ariel Manzur and the Godot community`)
-	https://docs.godotengine.org/en/stable/getting_started/step_by_step/scripting.html (*Scripting* by `Juan Linietsky, Ariel Manzur and the Godot community` )
