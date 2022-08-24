# Common Problems, in C++
## Numbers and Math
### Binary Add
Add binary values contained as strings and output as a string:
<details>
    <summary>Solution:</summary>
#include <iostream>
#include <cstring>
#include <algorithm>

std::string addBinary(std::string a, std::string b) {
    //contains # of characters in each string
    int asize = a.size() - 1, bsize = b.size() - 1;
    std::string result;
    int carry = 0;
    //while there are still numbers left to process in each string
    //or if there are any carried values to deal with
    while (asize >= 0 || bsize >= 0 || carry > 0){
        if (asize >= 0) {
            carry += a[asize] - '0';
            asize -= 1;
        }
        if (bsize >= 0) {
            carry += b[bsize] - '0';
            bsize -= 1;
        }
        result += (carry % 2) + '0';
        carry /= 2;
    }
    //reverse for final result
    reverse(result.begin(), result.end());
    return result;
}

int main(){
    std::cout << addBinary("01", "10") << '\n';
    std::cout << addBinary("1", "11") << '\n';
    std::cout << addBinary("1010", "1011") << '\n';
    return 0;
}
</details>

### Roman Numerals to Integers
Convert Roman numerals (VI, MCM, etc.) to integers:

<details>
    <summary>Solution:</summary>
    
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
</details>

### Two Sum
Given a vector of integers 
`nums`
, an integer 
`target` 
is determined that is the sum of two separate int values in the vector. Return a vector with the indices of the two numbers that add up to target. 

<details>
    <summary>Solution:</summary>
    
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
</details>

## Strings and Chars
### Needle in a Haystack
Given two strings needle and haystack, return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack. If needle is an empty string, return 0.

<details>
    <summary>Solution:</summary>

```c++
int strStr(std::string haystack, std::string needle) {
    if(needle.empty() == true){ //if needle is an empty string
        return 0;
    }
    std::size_t found = haystack.find(needle);
    if(found!=std::string::npos){
        return found;
    }
    else{
        return -1;
    }
}
```

Note:

The find function of `std::string` returns `std::string::npos` if no matches are found. `std::size_t` is an unsigned integral type that can identify the index where there is a match.  

</details>

### Valid Hex Code
Write a function that determines if a string is a valid hex code. 
* Must have # as first character
* Exactly 6 characters after, within A-F (upper and lower case) and 0-9

<details>
    <summary>Solution:</summary>
    
```c++
#include <iostream>
#include <array>

std::array<char, 21> arr = {'A', 'a', 'B', 'b', 'C', 'c', 'D', 'd', 'E', 'e', 'F', 'f',
'1', '2', '3', '4', '5', '6', '7', '8', '9'};

bool isValidHex(std::string hex){
    // catches any length errors
    if(hex.length() != 7) {
        return false;
    }
    // catches first character not being hash
    if(hex[0] != '#'){
        return false;
    }
    // first for loop iterates through input string
    for(int i = 1; i < hex.length(); i++){
        // second for loop iterates through valid input array
        for(int j = 0; j < arr.size(); j++){
            // if match, continue to next input string character
            if(hex[i] == arr[j]){
                break;
            }
            // if no match by end of valid input array
            else if(j == 20){
                return false;
            }
        }
    }
    return true;
} 
```
    
</details>

## Vectors
### Uno Hand 
Write a function that takes:
* player's current hand of cards
* current face card

A player can use a card if the color or number match. Function should return true if player can use one of their cards, else false. If a player's hand is empty, it should return false.

<details>
    <summary>Solution:</summary>
    
```c++
#include <iostream>
#include <vector>

bool canPlay(std::vector<std::string> hand,std::string face){
    std::string curr;
    // if there are no cards in hand
    if(hand.size() == 0){
        return false;
    }
    // loops through hand vector
    for(int i = 0; i < hand.size(); i++){
        // set string to current string in vector
        curr = hand[i];
        // if catches color matches, as the first letter would match
        //TODO catch bad input
        if(face[0] == curr[0]){
            return true;
        }
        // else if catches number matches
        else if(face.back() == curr.back()){
            return true;
        }
    }
    // if there are no matches, returns false
    return false;
}
```
</details>

<details><summary>Example with tests:</summary>
<p>

```c++
   #include <iostream>
#include <vector>

bool canPlay(std::vector<std::string> hand,std::string face){
    std::string curr;
    // if there are no cards in hand
    if(hand.size() == 0){
        return false;
    }
    // loops through hand vector
    for(int i = 0; i < hand.size(); i++){
        // set string to current string in vector
        curr = hand[i];
        // if catches color matches, as the first letter would match
        //TODO catch bad input
        if(face[0] == curr[0]){
            return true;
        }
        // else if catches number matches
        else if(face.back() == curr.back()){
            return true;
        }
    }
    // if there are no matches, returns false
    return false;
}

void print(bool b){
    if(b == true){
        std::cout << "true\n\n";
    }
    else{
        std::cout << "false\n\n";
    }
}

int main(){
    std::vector<std::string> s = {"yellow 3", "yellow 5", "red 8"};
    print(canPlay(s, "red 2")); // true
    std::vector<std::string> t = {"yellow 3", "yellow 5", "red 8"};
    print(canPlay(t, "blue 5")); // true
    std::vector<std::string> u = {"yellow 3", "blue 5", "red 8", "red 9"};
    print(canPlay(u, "green 4")); // false
    std::vector<std::string> v = {"yellow 3", "red 8"};
    print(canPlay(v, "green 2")); // false
    std::vector<std::string> w = {};
    print(canPlay(w, "green 2")); // false
    return 0;
}
```

</p>
</details>
