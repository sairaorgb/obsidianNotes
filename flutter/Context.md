- `BuildContext` acts as an identitiy card + gps location for widgets in widget tree.
-  It gives access to location in widget tree, Element of that widget and methods that let you traverse up the tree.
- These methods search for stuff traversing the tree through contexts. when a match is found, a dependency is mapped ( reactivity ) which notifies directly during a change.
``` dart
Theme.of(context)
Navigator.of(context)
Provider.of<Something>(context)
```
#### working of .of(context) methods 
``` dart
class Theme extends InheritedWidget {
  static ThemeData of(BuildContext context) {
    final Theme? theme = context.dependOnInheritedWidgetOfExactType<Theme>();
    return theme?.data ?? ThemeData.fallback();
  }
}
```
- Let's take `Theme.of(context)`, Flutter calls static method Theme.of() with context as arg. That method uses context which contains internal linkage to find nearest element containing Theme widget.
- `context.getElementForInheritedWidgetOfExactType<T>()` - finds matching widget
- `context.dependOnInheritedWidgetOfExactType<T>()`- dependency mapper.
#### Scope of context

- Context of current widget can't be used in `initState` cause it might not yet be built fully. But it can be used in `build` method and `didChangeDependencies` method.

#### why builder is needed sometimes

``` dart
class HomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
	  appBar: ...,
	  drawer: ...,
	  body: Builder(
	    builder: (context) => Center(
	      child: ElevatedButton(
	        onPressed: () => Scaffold.of(context).openDrawer(),
	        child: Text('Open Drawer'),
	      ),
	    ),
	  ),
	);
  }
}
```
- onPressed takes a `closure` that uses the BuildContext it captured. If it's not for builder ,it takes the context of homepage which lies above the context of scaffold and fails.   
- `findAncestorStateOfType<T>()` — walks parent pointers and checks Element.state types. **No dependency registration**, no rebuild-on-change behavior, just lookup.  ( Scaffold,...)
- `dependOnInheritedWidgetOfExactType<T>()` — used by `.of` helpers on `InheritedWidget`s; it registers a dependency so that when that `InheritedWidget` notifies. ( Theme,...)