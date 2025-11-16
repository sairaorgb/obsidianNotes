- An Inherited Widget is : a specialized widget that stores data and notifies subscribed descendants when that data changes, enabling efficient state propagation through the widget tree. 
- Inherited widget element store the dependencies and notifies resp descendants about changes.

#### Mechanism

- Rough config of Inherited widget looks like
``` dart
class MyConfig extends InheritedWidget {
  final String themeColor;

  const MyConfig({
    required this.themeColor,
    required Widget child,
  }) : super(child: child);

  static MyConfig? of(BuildContext context) {
    return context.dependOnInheritedWidgetOfExactType<MyConfig>();
  }

  @override
  bool updateShouldNotify(MyConfig oldWidget) {
    return themeColor != oldWidget.themeColor;
  }
}
```
- Now, anywhere in the descendant we can do
``` dart
final config = MyConfig.of(context);
print(config?.themeColor);
```

- `dependOnInheritedWidgetOfExactType` walks up the tree to find nearest MyConfig and registers a dependency that adds current widget element to `updateShouldNotify` subscriber list.
- `Theme`,`Navigator` and `Provider` all use this as base principle.
- `getElementForInheritedWidgetOfExactType<T>()` to just read data. 

#### Extra Info

- `context` is used in traversing the widget tree.
-  static method inside inherited widget returns the object of inherited class.
- element of inherited widget stores the info about widgets that registered a dependency.


