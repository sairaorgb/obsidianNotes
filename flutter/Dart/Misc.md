
**Iterables** (where,..)

**String Buffer**

- `+` or string interpolation can be used when dealt with multiple strings of less number.
- But when large numbers of strings are dealt, a string buffer which allocates a buffer space to accomadate new strings instead of rewriting them all again.
``` dart
var buffer = StringBuffer('hello');
buffer.write('sabar');
buffer.writeln('inserts text on new line');
buffer.writeAll(listentity,',');
var result = buffer.toString();
```

**Closures**

- A closure is a function that can remember variables from the scope where it was created, even after that scope has finished executing.
- when a function returns a function that use the variables declared in outer function , the returned function is called closure which stores those variables.

``` dart
Function makeCounter() {
  var count = 0;

  return () {
    count++;
    return count;
  };
}

void main() {
  var counter1 = makeCounter();
  var counter2 = makeCounter();

  print(counter1()); // 1
  print(counter1()); // 2
  print(counter2()); // 1  <-- different memory!
}
```

- small quirk in dart , for - loops in dart reintialize variables for each iteration. so , in the below example each closure uses different value ( 0,1,2 ) unlike js where it is (3,3,3).

``` dart
void main() {
  var functions = [];

  for (var i = 0; i < 3; i++) {
    functions.add(() => print(i));
  }

  for (var f in functions) {
    f();
  }
}
```

**Pattern matching**

``` dart
final Candidate(:name, :yearsExperience) = someCandidate;
print(name + yearsExperience);
final Candidate(:name: canName, :yearsExperience: canExp) = someCandidate;
print(canName);

// for loop
for (final Candidate(:name , :yearsExperience) in candidates) {}

// switch case 
switch (candidate) {
  case Candidate(:name, :yearsExperience):
    print('$name has $yearsExperience years of experience.');
}
```

- Candidate(:name, :yearsExperience) is an object pattern that destructures Candidate objects. ( type safe and concise )

