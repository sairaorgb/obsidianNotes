
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


