#Viz 3 Script - Callback Procedures

Here is a list of callback procedures recognized by the script engine. When defined in a script, these procedures are called by the system in response to events such as keyboard and mouse input. All of them are supported by container scripts, while only five (OnInitParameters, OnInit, OnExecPerField, OnExecAction, OnParameterChanged) are meaningful in scene scripts.

Sub **OnInitParameters**()

This procedure must contain all Register... calls (such as RegisterParameterInt), which are used to define the script's parameters. For locally defined scripts, this callback is invoked once for every script instance, immediately before OnInit, while for plugin scripts, it is called at system startup.

Sub **OnInit**()

Called after a script instance has been created (e.g. by pressing Compile and Run or by loading a scene with a script). Initialization code should be placed here.

Sub **OnEnter**()

Called when the mouse cursor enters the area occupied by the script's container in the output window. If the container's ExactPicking property, which is true by default, is set to false, it is sufficient that the mouse cursor hits the container's bounding box for OnEnter to be called.
Attention: Due to the necessary object hit-testing (picking) using this callback can have impacts on the overall render performance.

Sub **OnLeave**()

The reverse of OnEnter: Called when the mouse cursor leaves the container's area. This procedure is called only if OnEnter has previously been called for the script's container.
Attention: Due to the necessary object hit-testing (picking) using this callback can have impacts on the overall render performance.

Sub **OnEnterSubContainer**(subContainer As Container)

Called when the mouse cursor enters the area occupied by a sub-container of the script's container. Otherwise identical to OnEnter.
Attention: Due to the necessary object hit-testing (picking) using this callback can have impacts on the overall render performance.

Sub **OnLeaveSubContainer**(subContainer As Container)

Called when the mouse cursor leaves the area occupied by a sub-container of the script's container.
Attention: Due to the necessary object hit-testing (picking) using this callback can have impacts on the overall render performance.

Sub **OnExecPerField**()

Called once for every field.

Sub **OnExecAction**(buttonId As Integer)

Called when the user clicks on any push button defined by RegisterPushButton. If the script defines several push buttons, use buttonId to determine which button has been clicked on.

Sub **OnParameterChanged**()

Called whenever the user changes the value of script parameter defined by any of the RegisterParameter... functions.

Sub **OnGuiStatus**()

Called if the GUI wants to refresh the state of the UI. This would be the proper place where the plugin can set the UI state to enabled/disabled with SendGuiStatus or shown/hidden with SendGuiParameterShow.

Sub **OnKeyDown**(keyCode As Integer)

Called whenever the user presses a key. keycode identifies the key pressed. Possible values are: KEY_A, KEY_B, ..., KEY_Z, KEY_0, ..., KEY_9, KEY_HOME, KEY_END, KEY_PAGEUP, KEY_PAGEDN, KEY_UP, KEY_DOWN, KEY_LEFT, KEY_RIGHT, KEY_INSERT, KEY_DELETE, KEY_BEGIN, KEY_MULTIPLY, KEY_DIVIDE, KEY_ESCAPE, KEY_RETURN, KEY_ENTER, KEY_SEPARATOR, KEY_SPACE, KEY_BACKSPACE, KEY_TAB, KEY_CONTROL, KEY_MENU, KEY_ALT, KEY_SHIFT, KEY_F1, ... KEY_F12, KEY_SCROLL_LOCK, KEY_PAUSE, NUMPAD_INSERT, NUMPAD_END, NUMPAD_DOWN, NUMPAD_PGDN, NUMPAD_LEFT, NUMPAD_BEGIN, NUMPAD_RIGHT, NUMPAD_HOME, NUMPAD_UP, NUMPAD_PGUP, NUMPAD0, ..., NUMPAD9

Sub **OnKeyUp**(keyCode As Integer)

Called whenever the user releases a key.

Sub **OnButtonDown6DOF**(button As Integer, pos As Vertex, rot As Vertex)

Called when the user clicks on a scene grid. button defines the ID of the pressed button, pos specifies the 3D world position of the cursor and rot gives you the actual rotation.

Sub **OnButtonUp6DOF**(button As Integer, pos As Vertex, rot As Vertex)

Called whenever the user releases a mouse button.

Sub **OnMove6DOF**(button As Integer, pos As Vertex, rot As Vertex)

Called when the user moves the cursor on a scene grid. button defines the ID of the pressed button, pos specifies the 3D world position of the cursor and rot gives you the actual rotation.

Sub **OnMoveRelative6DOF**(button As Integer, pos As Vertex, rot As Vertex)

Called when the user moves the cursor on a scene grid. button defines the ID of the pressed button, pos specifies the offset vector to the last 6DOF position of the cursor and rot gives you the actual rotation.

Sub **OnScale6DOF**(button As Integer, scale As Vertex)

Called when the user performs a scale operation (e.g.: with a multi-touch device) on a scene grid. button defines the ID of the pressed button and scale specifies the actual scale factor.

Sub **OnMTHit**(stroke As Integer, x As Integer, y As Integer)

Called when the user touches this object (where this scriptplugininstance resides on) at a multi-touch device. stroke gives you the stroke ID of the multi-touch operation. x and y specify the hit position in screen coordinates. This callback is used to instantiate a certain control in the multi-touch server.
Attention: Due to the necessary object hit-testing (picking) using this callback can have impacts on the overall render performance.

Sub **OnMTMenu**(x As Integer, y As Integer)

Called when the user performs a menu gesture on the multi-touch device. x and y specify the hit position in screen coordinates.
Attention: Due to the necessary object hit-testing (picking) using this callback can have impacts on the overall render performance.

Sub **OnMTControlPZR2D**(x As Integer, y As Integer rot As Vertex, scale As Vertex, pressure As Double)

Called when a PZR2D control was registered. x and y specify the hit position in screen coordinates, rot gives you the object rotation, scale holds the scale factor and pressure contains the current mean contact pressure.

Sub **OnMTControlButton**(strokes As Integer, pressure As Double)

Called when a Button control was registered. strokes contains the current hit count on the button object and pressure defines the mean contact pressure.

Sub **OnMTControlInactive**()

Called when a registered multi-touch control became inactive (all strokes were removed from the object).

Sub **OnTouchTrace**(trace As Trace, touch As Touch)

Called when a new trace is created or when a touch is added to an existing trace.
touch contains the new touch.
The newly added touch can be also retrieved easily with trace.LastTouch.
Note that trace IDs and stroke IDs of other callbacks are identical (e.g.: OnMTHit can be easily used for hit testing of traces).

Sub **OnGesture**(gesture As Gesture)

After a GestureRecognizer was fed with an input Trace this callback 
will be executed with the resulting events.
gesture contains all properties of the current event.

Sub **OnMouseMove**(x As Integer, y As Integer)

Called whenever the mouse cursor is moved within the output window. x and y specify the cursor position in screen coordinates.

- Sub **OnLButtonDown**()
- Sub **OnMButtonDown**()
- Sub **OnRButtonDown**()

Called whenever the user presses the left/middle/right mouse button.
Attention: Due to the necessary object hit-testing (picking) using these callbacks can have impacts on the overall render performance.

- Sub **OnLButtonUp**()
- Sub **OnMButtonUp**()
- Sub **OnRButtonUp**()

Called whenever the user releases the left/middle/right mouse button.
Attention: Due to the necessary object hit-testing (picking) using these callbacks can have impacts on the overall render performance.

Sub **OnMouseWheel**(distance As Integer)

Called whenever the mouse wheel is rotated within the output window.

sub **OnSharedMemoryVariableChanged**(map As SharedMemory, mapKey As String)

Called when a variable in a SharedMemory map is changed. The SharedMemory map and the name of the variable are passed to this procedure as parameters. The name of the variable must previously be registered to the SharedMemory map by calling its RegisterChangedCallback procedure.

sub **OnSharedMemoryVariableDeleted**(map As SharedMemory, mapKey As String)

Called when a variable in a SharedMemory map is deleted. The SharedMemory map and the name of the variable are passed to this procedure as parameters. The name of the variable must previously be registered to the SharedMemory map by calling its RegisterChangedCallback procedure.

sub **OnGeometryChanged**(geom As Geometry)

Called when a geometry changed which was registered with RegisterChangedCallback or RegisterTextChangedCallback before.