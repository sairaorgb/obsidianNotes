unlike classical search , binary search uses the fact that the array is pre sorted and for every iteration halves the search space. so the time complexity will be logarthimic which is less than linear.

three  context variables named low mid high are considered . A target condition is checked for left and right half of which one with no chance of fullfilling it or a definite answer from that is eliminated from search space.

mid is low + (high - low)/2 and low and high becomes mid + 1 or mid - 1 accordingly .

lower bound of a target element could be the value inside array that is equal or greater than target value. 

upper bound is the value inside array that is strictly greater than target value.