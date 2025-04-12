This widget can be used to stack any widget on other widget. 
Significant thing is alignment of different widgets which can be done through different ways.


### 1. **Global Alignment (applies to all children)**

```dart 
Stack(   
	alignment: Alignment.center,   
	children: [     
	Container(width: 200, height: 200, color: Colors.red),     
	Icon(Icons.star, size: 64), // centered because of alignment   
	], 
)
```

### 2. **Using `Positioned` to place children manually**

``` dart 
`Stack(   
	children: [     
	Container(width: 200, height: 200, color: Colors.blue),     
	Positioned(       
	top: 20,       
	right: 20,       
	child: Icon(Icons.close, color: Colors.white),     
	),   
], )`
```


If you're using `Positioned`, make sure the Stack is inside a widget that gives it size (like `SizedBox`, `Container`, or a `Scaffold` body), otherwise it might render with zero size.