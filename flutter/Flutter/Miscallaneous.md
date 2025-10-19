**SystemChrome**

SystemChrome.setPreferredOrientaions() 
- locks the app to specific oritentation.

SystemChrome.setEnabledSystemUIMode() 
- controls visibility of system overlays (status , nav bars).

SystemChrome.setSystemUIOverlayStyle()
- changes style of statusbar and navbars (color,icon brightness).

SystemChrome.setApplicationSwitcherDescription() 
- changes display info at os side.

**Keys**

- flutter stores widgets as elements in widget tree which contains a refernce to widget (immutable config) , state and renderObject(layout/paint object)
- old widgets are compared against newer ones based on runtimeType and key 
- if both runtimeType and key match then the element is brought from old tree with state intact , if keys are not present flutter only considers changes in arguments and uses same state which might cause random errors.


### Stateful widgets

Includes the widget state and mutable state .. former one is responsible for creating the widget body whereas the later one maintains state variable that'll give the essence of statefulness.

#### Widget Lifecycle Steps:
1. `StatefulWidget` constructor is called.
2. `createState()` → creates `State<MyWidget>`.
3. **Class-level variables are initialized.**
4. `initState()` is called (override point).
5. `build()` is called to render the UI.

Class level Initializations can only include run-time unrelated ones . Since, widget state might not be created by then.
asyncs , widget related and  other should be initialized in initstate which happens after declaring class level variables.