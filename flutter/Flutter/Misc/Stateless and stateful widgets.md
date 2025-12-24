### what is state

- widget itself is immutable, connected to element which infact is connected to state.
- `State` is the mutable data ( non-final ) associated with a widget that determines how the UI renders and updates over time ( only for stateful ) .
- Apart from this local state, App state ( shared across widgets, providers ) , Ephemeral state ( short live UI stuff like textfield ) and persistent state ( on disk ) also exsist. 

### Widget types

###### Stateless widgets

- A `Stateless widget` is used when the UI does not change after it is built . Depends on constructor data and inherited data from parent widgets.

``` dart
class GreetingCard extends StatelessWidget {
  final String name;

  const GreetingCard({super.key, required this.name});

  @override
  Widget build(BuildContext context) {
    return Text('Hello, $name!');
  }
}
```

###### Stateful widgets

- A `StatefulWidget` is used when the UI changes dynamically. State object included to hold mutable data. 

``` dart
class GreetingCard extends StatefulWidget {
  const GreetingCard({super.key});

  @override
  State<GreetingCard> createState() => _GreetingCardState();
}

class _GreetingCardState extends State<GreetingCard> {
  String name = 'John';

  void changeName() {
    setState(() {
      name = 'Mary';
    });
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text('Hello, $name!'),
        ElevatedButton(
          onPressed: changeName,
          child: const Text('Change Name'),
        ),
      ],
    );
  }
}
```

### Lifecycle 

 - A `Widget` is just a configuration describing how flutter should render UI , but the widget itself doesn't paint. Flutter then builds a render tree from these descriptions.
 - Both widgets are immutable in terms of fields. when data changes flutter doesn't modify the widgets it rather creates a new one and repaints the changed stuff. So, `State` object in case of stateful objects persist between rebuilds.

![[Pasted image 20251128131421.png]]

``` scss
createState() ->  initState() -> didChangeDependencies() -> build() -> (setState() → build()) many times... -> didUpdateWidget() (if widget config changes)-> deactivate() -> dispose()
```

- In terms of understanding , context can be considered as a getway to element. It's just a bundle of data related to position, parent and child widgets.

**CreateState()**

- `State` is created . But No element , No context and no position. 
- Context is not yet created.

**initState()**

- Called only once when widget is inserted into widget tree.
- Intializing controllers, start animations, Fetch initial data and set up listeners.
```dart
@override
void initState() {
  super.initState();
  print("Hello, UI world.");
}
```
- context is available but dependencies  ( Inherited widgets ) might not yet wire their systems.

**didChangeDependencies()**

- Called right after `initState()` and again whenever an inherited widget above changes.
- Inherited widgets like Provider, Theme and MediaQuery can be accesed since the context is fully available here.

**build()**

- called after `initState()` , `didChangeDependencies()`,`setState()`, when parent widget rebuilds or when dependencies change.
- This method must be pure. No side-effects, No API calls and No heavy computations.

**didUpdateWidget(oldWidget)**

- called when parent rebuilds, same widget type but new parameters
- state is reused but widget config is changed.

**setState()**

- When you call setState() the State object marks its Element dirty. 
- Flutter schedules a frame, then during the build phase that Element runs your build() and reconciles its children. 
- After all dirty elements rebuild, the rendering pipeline (layout → paint → compositing → raster) runs and the new pixels appear.

**addPostFrameCallback()**

**dispose()**

- called when the widget is permanently removed.
- controllers, Streams, FocusNodes, Animations and Timers are cleared.

``` dart
@override
void dispose() {
  _controller.dispose();
  super.dispose();
}
```




