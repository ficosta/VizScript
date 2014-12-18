An Introduction to Viz 3 Script
=

##Getting Started


###Creating and Running Scripts

As an example, let's start with a simple event-driven script. First, create a script object by dragging the built-in Script plugin from the Function Container pool onto one of your containers. Then, after clicking on the container's new script object, copy & paste the following code into the the script editor.

###Example 1

    Sub OnEnter()
    	Alpha.Value = 100.0
    End Sub
    
    
	Sub OnLeave()
    	Alpha.Value = 50.0
    End Sub

This script consists of two event procedures, OnEnter and OnLeave, which are recognized by the system and invoked whenever the mouse cursor enters or leaves the area occupied by the container in the output window. Let's take a closer look at what happens inside OnEnter:

	Alpha.Value = 100.0

This code sets the alpha value of the container to 100 (the maximum value). If there is no alpha object, no action occurs, so make sure that the container has an alpha object. Alpha refers to the container's alpha object, Value is a member (in this case: the only member) of the alpha object, representing its value. Changes made to Alpha.Value are immediatly made visible in the output window.

Similarly, the code in the body of OnLeavesets the alpha value back to 50.

To execute the script, click Compile & Run. Click Edit to return to editing mode.

###Example 2

	Sub OnExecPerField()
	    Dim cube As Container
	    cube = Scene.FindContainer("Cube")
	    If scaling.x > 1.5 Then
	        cube.Scaling.x = 2.0
	    Else
	        cube.Scaling.x = 1.0
	    End If
	End Sub

This example defines the event procedure OnExecPerField, which is called periodically for each field. In the first line, a variable named cube, of type Container, is defined:

Dim cube As Container
Variables of type Container can be initialized to point to a container in the scene tree. In the next line, we call the built-in function FindContainer (which is a member of the scene class, accessible via the container's Scene property) to initialize cube to point to a container named cube. If there is no such container, cube is set to Null, the consequence being that any further operations on cube will have no effect.

cube = Scene.FindContainer("Cube")
Now, we check if the x-scaling of the current container (that is, the container holding the script) is greater than 1.5, and, depending on the result, we set the x-scaling of the container named cube to 2.0 or 1.0:

	If Scaling.x > 1.5 Then
	    cube.Scaling.x = 2.0
	Else
	    cube.Scaling.x = 1.0
	End If

Note that the expression Scaling.x refers to the current container while cube.Scaling.x refers to the container referenced by the variable cube.

##Creating Script-Based Plugins

If you want to reuse a container script on other containers, you can create a plugin out of the script. To do so, select a folder other than Global in the Function Container pool and then drag the container's script icon in the pool. Plugins created this way are stored in the plugin directory under the name pluginname.vsl. They can be used like ordinary plugins and, in addition, allow the following operations.

Script-based plugins can be deleted from the plugin pool by dragging them in the trash.
The source code of a script-based plugin can copied into a script editor by dragging the the plugin from the pool to the script editor.