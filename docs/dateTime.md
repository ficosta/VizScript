#Alpha

Base type [Base](base.md)

##Description

An alpha object. This is used to control a container's alpha value.
Properties

| Properties  | Desc |
|:-----|:-----|
| Active As Boolean                                |                                                                    |
| Scene As Scene [read-only]                       | (Inherited from [Base](base.md)) Gets the current scene.                      |
| Stage As Stage [read-only]                       | (Inherited from [Base](base.md)) Gets the current stage.                      |
| System As System [read-only]                     | (Inherited from [Base](base.md)) Gets system wide data.                       |
| Valid As Boolean [read-only]                     | (Inherited from [Base](base.md)) Returns true if the object is valid.         |
| Value As Double                                  | The alpha value [0.0, 100.0]                                       |
| VizCommunication As VizCommunication [read-only] | (Inherited from [Base](base.md)) Gets the VizCommunication object.            |
| VizId As Integer                                 | (Inherited from [Base](base.md)) Gets or sets the internal id of this object. |

##Member Procedures

| Method | Desc |
|:---|:---|
| Function FindChannelOfObject(channelName As String) As Channel | (Inherited from [Base](base.md)) Finds an animation channel acting on this object. If this function is called on a type that cannot be animated (such as Director), it returns null. You can limit the search to a particular director by using the syntax "directorName$channelName" for the channelName argument. Nested directors may be specified like this: "directorName1$directorName2$channelName". |
| Function FindKeyframeOfObject(keyframeName As String) As Keyframe | (Inherited from [Base](base.md)) Finds a keyframe acting on the object. If this function is called on a type that cannot be animated (such as Director), it returns null. You can limit the search to a particular director by using the syntax "directorName$keyframeName" for the keyframeName argument. Nested directors may be specified like this: "directorName1$directorName2$keyframeName". |
| Function FindOrCreateChannelOfObject(channelName As String) As Channel | (Inherited from [Base](base.md)) This function works like FindChannelOfObject except that if no animation channel with the specifed name exists, a new one is created. |
| Function GetChannelsOfObject([out] v As Array[Channel]) As Integer | (Inherited from [Base](base.md)) Fills the array v with the animation channels acting on this object, returning the number of channels. If this function is called on a type that cannot be animated (such as Director), v will be empty. |
| Function IsAnimated() As Boolean | (Inherited from [Base](base.md)) Returns true if there are animation channels for this object. If this function is called on a type that cannot be animated (such as Director), it always returns false. |
| Sub SetChanged() | (Inherited from [Base](base.md)) Mark this object as changed. |
