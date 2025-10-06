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
