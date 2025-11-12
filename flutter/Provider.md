- Does the `dependency injection` job of passing data directly to widget rather than propagating through parents all the way. Based on [[Inherited widget]].
- Provider handles updateShouldNotify, dependency tracking, and context lookups automatically.

#### Mechanism

In case of Inherited widget 
```dart
class MyData extends InheritedWidget { ... }
```
But that becomes this , which includes all the necessary methods required to mimic that
``` dart
Provider<MyDataClass>(
  create: (_) => MyDataClass(),
  child: MyApp(),
);
```

#### Usage

- context gives both location and reactivity for providers.

``` dart
void main() {
  runApp(
    Provider<String>(
      create: (_) => "Hello Sairaorg!",
      child: MyApp(),
    ),
  );
}
```

- Provider`<T>` stores data of type T in the widget tree. 
- Provider takes any Dart value (raw or class instance) and wraps it inside a hidden InheritedWidget that knows when to rebuild and how to let descendants access that data through their BuildContext.
