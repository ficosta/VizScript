#Language Overview

##Types and Variables

###Declarations

In the viz script language, every variable must be declared before it is used. The syntax for declaring a variable named varname of type type is

	Dim varname As type
####Examples:

	Dim a As Integer       ' declares a single variable of type Integer
	Dim b, c As Double     ' declares two variables of type Double
Variables that are declared this way are initialized to a default value, which is 0 for numeric data types (Integer, Double), False for Boolean, and an empty string for variables of type String.

It is possible to initialize a variable to a value other than the default value by using the syntax

	Dim
	varname As type = initexpr
####Example:

Dim a As Integer = 10
Here, a is initialized to the value 10. If an initexpr is present, you don't have to specify a type. The variable will then take the type of the initexpr.

	Dim a = 10    ' declares an Integer
	Dim b = 10.0  ' declares a Double
	Dim c = True  ' declares a Boolean
###Built-In Data Types

#####Boolean

Boolean variables are either **True** or **False**, the default value being **False**. Conditional expressions such as a > b always evaluate to **Boolean**. For example, the code

	Dim c As Boolean
	c = a > b
	If c Then Println "a > b"
has the same effect as the more concise

	If a > b Then Println "a > b"
Boolean variables often appear as data members of types such as Container:

	Dim c As Container = Scene.FindContainer("Cube")   ' find the container named "Cube"
	c.Open = True                                      ' open the container in the scene tree
#####Integer

A 32-bit signed integer. The default value is 0.

#####Double

A 64-bit floating point number. The default value is 0.0. Note that numeric literals are interpreted as being of type Double only if they contain a decimal point, otherwise they are interpreted as being of type Integer.

	Dim a = 10     ' declares an Integer
	Dim b = 10.0   ' declares a Double
	Dim c = 10.2   ' declares a Double
#####Vertex

A vertex consisting of three Double members: x, y, z. By default, the members are 0.0.

If we declare v as a Vertex,

	Dim v As Vertex
we can fill the members as follows:

	v.x = 9.0
	v.y = 2.0
	v.z = 1.0
There is also a function, CVertex, that constructs a Vertex object from three Double values:

	v = CVertex(9.0, 2.0, 1.0)
has the same effect as the previous piece of code. To fill all three members with the same value, write

	v = CVertex(3.0)    ' same as v = CVertex(3.0, 3.0, 3.0)
#####Matrix

A 4x4 matrix of Double values. The default value is the identity matrix. See the script reference for details.

#####Array[Type]

A dynamic, one-dimensional array holding values of type Type. Use the index operator [] to access individual elements. Various member procedures are available to insert new elements to or to erase elements form the array. See the script reference for details. Example:

Dim a As Array[Integer]
Dim i As Integer

Fill "a" with 10 random numbers from the range 1 ... 5
	
	For i = 1 To 10
	    a.PushBack(Random(1, 5)) ' insert new element at the end of the array
	Next

Iterate through the array to print the random numbers
(note that indices start at 0; the index of the last index
can be obtained through the "ubound" property)
For i = 0 To a.ubound
    Println a[i]
Next

#####Color

A color value consisting of four Double members: Red, Green, Blue, Alpha, each of them ranging from 0.0 to 1.0. The default value is 0.0 for Red, Green, Blue and 1.0 for Alpha.

#####Viz Object Types

Viz object types refer to a viz object, examples being Container, Texture, Scene. All viz object types share a common set of data members, which are accessible through the base type Base. Objects hold an idintifier (accessible through the VizId property) that identifies the object referred to. To check whether the object reference is valid, compare against the predefined Null constant.

	Dim cube As Container
	cube = Scene.FindContainer("Cube")
	If cube = Null Then println "Container not found."

#####User-Defined Types

You can create your own data types by combining several data members into a structure. The following example defines a new data type called Planet that defines the data members mass, position, and direction.

Structure Planet
    mass As Double
    position, direction As Vertex
End Structure
You can now declare and use variables of that type as follows.

Dim p As Planet
' ...
p.mass = 1000.0
p.direction.x = 20.0
' ...
Type Conversion

There are several built-in functions available for converting an expression to a different type:

Function Call	Converts x to type ...

- CInt(x)	Integer
- CDbl(x)	Double
- CBool(x)	Boolean
- CStr(x)	String
- CType(x, type)	type

Example:

	Dim a As Integer, b As Double
	' ...
	a = CInt(b) ' convert b to a by truncating the floating point part of 'b'
	a = CType(b, Integer) ' same thing

In addition, C-style typecasts are also supported:

	Dim a As Integer, b As Double
	' ...
	dim s = (String)a
	dim a = (Integer)b
#####Operators

The following is a list of operators supported by the script language. The operators are listed in order of their precedence (from highest to lowest).


| Operator | Operation | Supported Operand Types |
|:--------|:---------|:-----------------------------|
| ^ | Exponentiation | Integer, Double, Matrix |
| - | Unary minus | Integer, Double, Vertex, Matrix |
| *, / | Multiplication, division | Integer, Double, Vertex, Matrix |
| Cross | Cross product | Vertex |
| \ | Integer division | Integer, Double |
| Mod | Modulus | Integer, Double |
| +, - | Addition, subtraction | Integer, Double, Vertex, Matrix |
| & | String concatenation | String |
| <<, >> | Bit shift | Integer |
| =, <> | Comparison | Boolean, Integer, Double, Vertex, Matrix, String, Color |
| <, <=, >, >= | Comparison | Integer, Double, String |
| Not | Logical/bitwise not | Boolean, Integer |
| And | Logical/bitwise and | Boolean, Integer |
| Or, Xor | Logical/bitwise or, xor | Boolean, Integer |
| ^=, *=, /=, \=, +=, -=, <<=, >>=, &= | Compound assignment | See corresponding non-compound operators |
| = | Assignment | All types |		
		
Please note the following:

All operators are binary (that is, they take two operands), expect for unary minus (-) and Not, which are unary (that is, they take only one operand).
The comparison operators (=, <>, >=, etc.) return a Boolean value that represents the result of the comparison.
Whenever = is used in a conditional expression, it is treated as a comparison operator, otherwise it is treated as the assignment operator. If you want to two two values for equality outside a conditional expression, use == instead of =.
When used on Vertex values, the multiplication operator * computes the inner product (aka dot product) of the two vertices, returning a Double value.
The string concatenation operator & can be used with operand types other than String, in which case the operands are first converted to String.
The operators Not, And, Or, Xor are logical operators when used on Boolean values, and bitwise operators when used on Integer values.
Procedures

There are two types of procedures:

Sub procedures that perform actions but do not return a value to the calling code.
Function procedures that return a value to the calling code.
Sub Procedures

The syntax for declaring a sub procedure is:

	Sub SubName(parameterList)
	    Code
	End Sub
parameterList is a list of comma-separated parameter declarations, each having the following syntax:

[ByVal | ByRef] parameterName As parameterType
The keywords ByVal and ByRef specify whether the argument is passed by value or by reference (more on that later). Passing by reference is the default.

Example: A sub procedure taking no arguments:

	Sub MoveLocalContainer()
	    Position.x += 10.0   ' move the local container 10 units along the x-axis
	End Sub
To call this procedure, write

MoveLocalContainer()
or, as the parentheses are optional,

MoveLocalContainer
Example: A sub procedure with two parameters.

	Sub MoveContainer(c As Container, d As Double)
	    c.Position.x += d    ' move the container "c" "d" units
	End Sub
	The calling syntax is
	
	Dim someContainer As Container
	' ...(intitalize "someContainer")
	MoveContainer(someContainer, 20.0)
	Again, the parentheses are optional:
	
	Dim someContainer As Container
	' ...
	MoveContainer someContainer, 20.0
To call a user-defined procedure that is defined in a different container script, use the Script property to access that script. Suppose, for example, that the following script is defined on a container named mycontainer.

	Sub MoveX(x As Double)
	    Position.x += x
	End Sub
Then we can call the function from within another container's script as follows.

	Dim c As Container
	c = Scene.FindContainer("mycontainer")
	c.Script.MoveX(10.0)
Exit Sub lets you exit a function from within the body of the sub procedure.

	Sub MySub(i As Integer)
	    If i < 0 Then Exit Sub
	    ' ...
	End Sub
Function procedures

The syntax for declaring a function procedure is:

	Function FunctionName(parameterList) As returnType
	    Code
	End Function
To return a value to the calling code, assign the value to the function name.

Example: A function taking two Vertex parameters and returning a Double value.

	Function Distance(a As Vertex, b As Vertex) As Double
	    Distance = Sqrt((a.x - b.x) ^ 2 + (a.y - b.y) ^ 2 + (a.z - b.z) ^ 2)
	End Function
To call this function, write

	Dim d As Double
	Dim v1, v1 As Vertex
	...
	d = Distance(v1, v2)

Exit Function lets you exit a function from within the function body.

Passing Variables by Value and by Reference

There are two mechanisms for a passing a variable to a procedure: by value and and by reference. If the variable is passed by value, a copy of the variable is passed. Consequenty, changes made to the corresponding paramter variable within the procedure only affect the copy of the variable passed. Specify the keyword ByVal with a parameter name to indicate that variables should be passed by value.

	Sub CountToZero(ByVal i As Integer)
	    Do While i >= 0
	        Println i
	        i -= 1   ' modifies only local copy of 'i'
	    Loop
	End Sub
In contrast, if a parameter is marked ByRef, only a reference of the variable is passed to the function. Thus, changes made to the parameter affect the corresponding variable in the calling code.

	Sub Clamp(ByRef x As Double)
	    If x > 1.0 Then x = 1.0
	    If x < 0.0 Then x = 0.0
	End Sub
You can use this procedure as follows.

	Dim d As Double
	d = 1.5
	Clamp(d)
	' d is now 1.0
	d = -0.4
	Clamp(d)
	' d is now 0.0
Note that by default variables are passed by reference. Note also that if you pass a constant or an expression rather than a variable to a procedure, it is always passed by value, even if the corresponding paramter is declared ByRef.

####Control Structures

#####If ... Then ...

Use If ... Then ... to execute a code block depending on the Boolean value of a condition. To execute only one statement if a condition is True, you can use the single-line version of If ... Then.

If a > b Then a += 1
Here, the statement a += 1 is executed if the condition a > b evaluates to True. The multi-line version of If ... Then lets you execute more than one statement:

	If a > b Then 
	    a += 1 
	    b += 1 
	End If
Use an Else block to define code that is executed if the condition is False.

	If a > b Then
	    a += 1
	Else
	    b += 1
	End If
Use ElseIf to test additional conditions if the first condition is False.

	If a = 1 Then
	    Println "case 1"
	ElseIf a = 2 Then
	    Println "case 2"
	ElseIf a = 3 Then
	    Println "case 3"
	Else
	    Println "default"
	End If
#####Select Case ...

Select Case ... runs one of several groups of statements, depending on the value of an specified test expression. With the help of Select Case ... you are able to avoid huge ElseIf constructs.

The following example does the same as the previous ElseIf code block but in a more elegant way.

	Select Case a
	Case 1
	    Println "case 1"
	Case 2
	    Println "case 2"
	Case 3
	    Println "case 3"
	Case Else
	    Println "default"
	End Select
The Case Else in the previous example is optional and is called only if none of the other Case expressions fit.

It is also allowed to define multiple Case expressions in a comma separated list, you can use Is (where Is stands fo the test expression) for doing boolean comparisons and you are able to define Case ranges with To.

	Select Case a
	Case 1
	    Println "case 1"
	Case 2, 3, 4
	    Println "case 2, 3 or 4"
	Case 5 To 7
	    Println "case 5, 6 or 7"
	Case Is > 7
	    Println "case > 7"
	End Select
You can leave any Select Case code block with Exit Select. The program continues after the End Select statement.

#####Do ... Loop

Do ... Loop executes a code block an indefinite number of times depending on the Boolean value of a condition. Use Exit Do to exit a loop prematurely. There are four versions of Do ... Loop.

(1) The condition is checked before the is entered. The loop code is repeated as long as the condition is True.

	Dim a = 0
	
	Do While a <= 10
	    Println a
	    a += 1
	Loop
(2) The condition is checked after the loop code has been executed at least once. The loop code is repeated as long as the condition is True.

	Dim a = 0
	
	Do
	    Println a
	    a += 1
	Loop While a < 10
(3) The condition is checked after the loop code has been executed at least once. The loop code is repeated until the condition is True.

	Dim a = 0
	
	Do
	    Println a
	    a += 1
	Loop Until a > 10
(4) An infinite loop. Exit Do must be used to exit the loop.

	Dim a = 0
	
	Do
	    Println a
	    a += 1
	    If a > 10 Then Exit Do
	Loop
	For ... Next

You can use a For ... Next loop to execute a code block a specific number of times. A For ... Next loop uses a counter variable that increases or decreases in value during each repetition of the loop. The counter variable must be of type Integer or Double.

	Dim i As Integer
	
	' produce the sequence 0, 1, 2, ... 10
	For i = 0 To 10
	    Println i
	Next
By default, the counter increases by 1 at the end of each repetition. Use a Step value to specify a different increment.

Dim i As Integer

' produce the sequence 10, 8, 6, 4, 2, 0
For i = 10 To 0 Step -2
    Println i
Next
Exit For can be used to exit a for loop prematurely.

Declaring the counter variable is optional. If you don't declare it, it will automatically assume the type of the value with which it is initialized. For example, the following code works even though i is never formally declared.

	For i = 0 To 10    ' i is implicitly declared as an Integer
	    Println i
	Next
	For Each ... Next

This control structure is specially designed for iterating through the elements of an array in a sequential, read-only fashion. For example:

	Dim a As Array[Integer]
	a.Push(12)
	a.Push(-6)
	a.Push(7)
	
	For Each i In a    ' As with ordinary For ... Next, 'i' need not be explicitly declared
	    Println i
	Next
Script Interoperability

Dynamic Procedure Calls

A container script may call a procedure defined in another container script. For example, the script of container A defines the sub procedure:

	Sub RotateAroundX(d As Double)
	    Rotation.X += d
	End Sub
To call this function from within container B's script, call the function via A's Script member:

	Sub OnExecPerField()
	    dim ContainerA = Scene.FindContainer("A")
	    ContainerA.Script.RotateAroundX(3.0)
	End Sub
Note that since container scripts are compiled independently from each other, procedure calls between different scripts are resolved at run-time rather that at compile-time. This means that you must make sure that the procedure's name is written correctly and the arguments passed have the correct types, or else the call will result in a run-time error.

Accessing Scene Members

Procedures, member variables, and structures defined by a scene script can be used from within any container script belonging to the same scene. Use the syntax Scene.MemberName to access such a member. For example, if the scene script looks like this:

	Dim counter As Integer
	
	Function GetNameOfRootContainer() As String
	    GetNameOfRootContainer = RootContainer.Name
	End Sub
	you can access counter and GetNameOfRootContainer from within a container script as follows.
	
	Sub OnExecPerField()
	    Scene.counter += 1
	    println Scene.GetNameOfRootContainer()
	End Sub

####Data Sharing via Memory Maps

Objects of any type may be shared between scripts (even across the network) using memory maps. Please refer to Data Sharing for more information.

#####Calling script procedures via INVOKE

Sometimes you may want to call a user-defined script procedure through the command interface, usually from within an external control application (which may be Viz script running on a remote machine). This can be done by sending an INVOKE command to the script object, followed by a list of arguments that are passed to the function. For example, suppose this script is defined on a container named ScriptContainer:

	Sub SetText(i As Integer, s As String)
	     Geometry.Text = i & ": " & s
	End Sub

This function can be called through the command interface using this command:

<scene_location>*TREE*$ScriptContainer*SCRIPT INVOKE SetText 3 "test"

Similarly, if the the procedure is a member of the scene script, you can call it this way:

<scene_location>*SCRIPT INVOKE SetText 3 "test"

###GUI

To make your scripts control- and configure-able for users you are able to define parameters which are shown in a dynamically generated graphical user interface.

#####Registering Script Parameters

You'll find all parameter registration calls under Global Procedures. Each parameter or push-button must have a unique name so that viz can assign its value or ID correctly. Labels can consist of any characters but names have to be alphanumeric and mustn't contain any whitespaces.

Here is a short example which demonstrates the use of parameters:

	sub OnInitParameters()
	    RegisterParameterInt("index", "Index", 1, 1, 6)
	    RegisterParameterDouble("alpha", "Alpha", 50.0, 0.0, 100.0)
	    RegisterPushButton("action", "Action", 1)
	end sub

	sub OnExecAction(buttonId As Integer)
	    dim c as Container
	    dim i, index as integer
	    dim a as double
	
	    index = GetParameterInt("index")
	    a = GetParameterDouble("alpha")
	
	    i = 1
	    c = ChildContainer
	    do while c <> null
	        if i = index then
	            c.alpha.value = 100.0
	        else
	            c.alpha.value = a
	        end if
	        i = i + 1
	        c = c.NextContainer
	    loop
	end sub