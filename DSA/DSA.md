
1. standard sort function in stl has a time complexity of o(NLogN) . it uses a hybrid sorting algorithm called Introsort, which combines QuickSort, HeapSort, and InsertionSort. whereas inserting into a ordered map takes o(logN) so both timeC are same.
2. Storing(i.e. insertion) and fetching(i.e. retrieval) in a C++ map, both take always O(logN) time complexity.But the unordered_map in C++ and HashMap in Java, both take O(1) time complexity to perform storing(i.e. insertion) and fetching(i.e. retrieval).But in the worst case, this time complexity will be O(N) for unordered_map. If unordered_map gives a time limit exceeded error(TLE), we will then use the map.
3. In the map data structure, the data type of key can be anything like int, double, pair<int, int>, etc. But for unordered_map the data type is limited to integer, double, string, etc. We cannot have an unordered_map whose key is pair<int, int>.

#### Intro
- Both \n and endl can be used to add a new line, but escape character newline is a low level operation and just adds a newline whereas endl , apart from adding new line clears the output buffer which becomes costly
- All the libraries in bits/stdc++.h are compiled everytime the code is compiled so this particular preprocessor should be used judicially.
- value in a switch case should be int or char (M_PI is the value of pi) .
- 

#### Syntax

  - slicing of string 
	- string.substr(startIndex, length);
- casing of strings 
	- tolower(ch) and toupper(ch) for only character
	- can use a range based loop iterating the pointer to modify the entire string
	- using std::transform(input_begin, input_end, output_begin, unary_function);
			- **`input_begin`**: Iterator to the beginning of the input range 
			- **`input_end`**: Iterator to the end of the input range (e.g., `str.end()`).
			- output_beg: Iterator to the start of the range where the result will be stored 
			- **`unary_function`**: A callable (e.g., function pointer, lambda, or functor) that takes one argument and returns the transformed value.
	
