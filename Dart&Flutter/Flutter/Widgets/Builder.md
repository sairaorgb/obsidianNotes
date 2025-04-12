
## What is `Builder`?

- A widget that accepts a **builder function**: `(BuildContext context) => Widget`
- Creates a **new BuildContext** below its parent in the widget tree
- Useful when:
  - You need access to inherited widgets (`ScaffoldMessenger`, `Theme`, etc.)
  - You want lazy or scoped widget creation
  - You want local rebuild optimization

A `BuildContext` is like a **reference point** in the widget tree. It tells Flutter:
> "I live _here_ in the widget tree. Please give me stuff that's _above_ me."

---

## 🧠 Why Use `Builder`?

### 1. **Accessing the Correct BuildContext**

Problem:
```dart
Scaffold(
  body: ElevatedButton(
    onPressed: () {
      ScaffoldMessenger.of(context).showSnackBar(...); // ❌ Error
    },
  ),
);
```
## solution
``` dart
Scaffold(
  body: Builder(
    builder: (context) {
      return ElevatedButton(
        onPressed: () {
          ScaffoldMessenger.of(context).showSnackBar(...); // ✅ Works!
        },
        child: Text("Click Me"),
      );
    },
  ),
);
```

Here the context used by scaffold has an extra layer on the buildcontext received from parent widget . so it can access scaffold properties inside it. 

## 2. **Lazy Widget Creation**
``` dart
if (shouldShowWidget)
  Builder(
    builder: (context) {
      print("Widget actually built now!");
      return SomeExpensiveWidget();
    },
  );

```

Only builds when needed, avoids unnecessary tree evaluation.

## 3. **Isolated Rebuilds**
``` dart
Column(
  children: [
    Text("Static"),
    Builder(
      builder: (context) {
        return Consumer<MyModel>(
          builder: (context, model, _) => Text("Count: ${model.count}"),
        );
      },
    ),
  ],
);
```
Only `Builder`'s subtree rebuilds on change.

## Under the Hood
``` dart
class Builder extends StatelessWidget {
  final WidgetBuilder builder;

  const Builder({required this.builder});

  @override
  Widget build(BuildContext context) {
    return builder(context);
  }
}
```


