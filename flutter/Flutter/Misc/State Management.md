## Provider

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

- Context gives both location and reactivity for providers.

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
- Provider takes any Dart value (raw or class instance) and wraps it inside a hidden [[Inherited widget]] that knows when to rebuild and how to let descendants access that data through their BuildContext.

### Types of Providers

###### `Provider<T>` – the simplest, static one

- Just holds a value or object and make it available to  descendants. It does not automatically rebuild when the values change (immutable). 
- static configs , services (firebase auth, cloud api's) and constants. 
``` dart
// Example
Provider<int>(
  create: (_) => 42,
  child: MyApp(),
);
// usage
final answer = context.read<int>();
```

#### ChangeNotifierProvider< T> extends ChangeNotifier

- It manages a class that extends ChangeNotifier, meaning the class can notify listeners when its internal state changes.
``` dart
// Class inside provider
class Counter extends ChangeNotifier {
  int count = 0;
  void increment() {
    count++;
    notifyListeners(); // tells Provider to rebuild dependents
  }
}
// Example
ChangeNotifierProvider(
  create: (_) => Counter(),
  child: MyApp(),
);
// Usage
final counter = context.watch<Counter>();
Text('${counter.count}');
```

- watch creates a dependency registration and gets notified during changes.

#### Future Provider
#### Stream Provider



### Multi Provider && Hirearchy of Provider

- Fancy way to nest multiple providers at same level. 
- Providers are evaluated top to bottom .so any provider class cannot use data from a provider below it. Here, A cannot use B 
``` dart
MultiProvider(
  providers: [
    Provider(create: (_) => A()),
    Provider(create: (_) => B()),
    Provider(create: (_) => C()),
  ],
  child: App(),
);
```

**Closest Ancestor** is picked when a two provdiers with same class is tried for a value through read or watch. 

- In a unconvention way , if outer provider is needed then the read must be exercised with context which sits above inner provider.

``` dart
final outerContext 
= context.findAncestorWidgetOfExactType<SomeParent>()!.context;
final outerModel = Provider.of<MyModel>(outerContext, listen: false);
```



### Consumers

- when widget cant access a provider above materialApp, enclose it in builder.
- `Consumer2` and `Consumer3` can be used to use multiple providers simultaneously. 

##### Vanilla Consumer
 
 - `context.read<T>()` and `context.watch<T>()` are used in primitive way.  One just gets the data and another subscribes to changes. 
- `read` is appropriate for callbacks to onTap, onPressed where rebuilts are unnecessary. 

-  `context.select<T, R>(selector)` is used when part of the provider class is only needed for update during changes. `T` is provider class , `R` is field of provider class and selector is a callback.
``` dart
final name = context.select<UserModel, String>((u) => u.name);
```
- subscribes to only u.name , can be used in the context of Lists, chat apps, dashboards, big models

#### Consumer< T>

``` dart
Consumer<Counter>(
  builder: (_, counter, __) => Text('${counter.count}'),
);
```
- Only rebuilds part of widget. Context can be added to builder for lower level BuildContext. 

#### Selector< T>

- Consumer + select built into one absolute unit. 
``` dart
Selector<Counter, int>(
  selector: (_, counter) => counter.count,
  builder: (_, count, __) => Text('$count'),
);
```
-  Tracks only selected value and builds the widget when it gets notified. 

##### Provider.of< T>(context)

- old school , listen: true mimics watch
``` dart
final model = Provider.of<Counter>(context); // listen = true
final model = Provider.of<Counter>(context, listen: false); // no rebuild
```

