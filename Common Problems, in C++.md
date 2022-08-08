# Common Problems, in C++
## Numbers and Math
### Roman Numerals to Integers
To convert Roman numerals (VI, MCM, etc.) to integers, we can use a map that stores characters and integers:

```c++
#include <iostream>
#include <map>

int roman_to_int(std::string &s){
    // map values of numerals to ints
    // case insensitive optional
    std::map<char,int> roman;
    roman['M'] = 1000;
    //roman['m'] = 1000;
    roman['D'] = 500;
    //roman['d'] = 500;
    roman['C'] = 100;
    //roman['c'] = 100;
    roman['L'] = 50;
    //roman['l'] = 50;
    roman['X'] = 10;
    //roman['x'] = 10;
    roman['V'] = 5;
    //roman['v'] = 5;
    roman['I'] = 1;
    //roman['i'] = 1;
    // initiate return value
    int r = 0;
    // for the size of the string
    for(int i = 0; i < s.size(); i++){
        // if the next numeral is greater in value, decrement
        if(roman[s[i]] < roman[s[i + 1]]){
            r -= roman[s[i]];
        }
        // else add value
        else{
            r += roman[s[i]];
        }
    }
    return r;
}
```

### Two Sum
Given a vector of integers 
`nums`
, an integer 
`target` 
is determined that is the sum of two separate int values in the vector. Return a vector with the indices of the two numbers that add up to target. 

```c++
#include <iostream>
#include <vector>

std::vector<int> twoSum(std::vector<int>& nums, int target) {
    // assume input will have exactly one solution
    // may not use same element twice
    std::vector<int> result;
    for(int i = 0; i < nums.size(); i++){
        for(int j = 0; j < nums.size(); j++){
            if((nums[j] == target - nums[i]) && (i != j)){
                result = { i, j };
                return result;
            }
        }
        
    }
    return result;
}
```