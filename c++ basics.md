# c++ basics

## inputs, outputs, and strings
At the very simplest, we output text:
```
#include <iostream>

int main(){
    std::cout << "Hello World!\n";
    return 0;
}
```
We can also take in text from the user:
```
#include <iostream>

int main(){
    int a;
    std::cout << "Type a number: ";
    std::cin >> a;
    return 0;
}
```
This, however, doesn't allow for any spaces. For that, we need getline(). We'll also use a string to store the input:
```
#include <iostream>
#include <string>

int main(){
    std::string newName;
	std::cout << "Type your full name: ";
	getline(std::cin, newName);
    return 0;
}
```
It's good to note when using getline(), the extration operator >> leaves \n hanging, and getline() reads input until a newline or \n is found. This can leave a getline() non-functional. To resolve this, std::cin.ignore() can be used:
```
#include <iostream>
#include <string>

int main(){
    std::string newName;
    int age;
    std::cout << "Type your age: ";
    //cin takes input as an int
    std::cin >> age;
	std::cout << "Type your full name: ";
	std::cin.ignore();
    //cin takes input as a string
	getline(std::cin, newName);
    return 0;
}
```
There are times it may be useful to convert a string to a numerical value and vice/versa. The stringstream functionality allows for this:
```
#include <iostream>
#include <string>
#include <sstream>

int main(){
	std::string mystr;
	float price;
	int quantity;

	std::cout << "Enter price: ";
	getline(std::cin, mystr);
	std::stringstream(mystr) >> price;
	std::cout << "Enter quantity: ";
	getline(std::cin, mystr);
	std::stringstream(mystr) >> quantity;
	std::cout << "Total price: " << price * quantity << '\n'; 
	return 0;
}
```
## booleans, if statements, and loops
Booleans are a binary data type, usually regarded as either TRUE (returns 1) or FALSE (returns 0). These are useful, among other applications, for if statements.
```
#include <iostream>

int main(){
    int test = 10;
    if(test == 10){
        std::cout << "True\n";
    }
    else{
        std::cout << "False\n";
    }
	return 0;
}
```
If statements can be more complicated, with multiple conditions before an else (else if), or no else at all. Consider:
```
#include <iostream>

int main(){
    int test = 21;
	if (test < 21) {                
		std::cout << "Hit?\n";
	}
	else if (test == 21) {
		std::cout << "Blackjack!\n";
	}
	else {
		std::cout << "Bust!\n";
	}
    return 0;
}
```
You can also use a ternary operator for simple if/else statements:
```
#include <iostream>
#include <string>

int main(){
    int test = 90;
    //variable = (condition) ? expressionTrue : expressionFalse;
	std::string result = (test > 89) ? "Pass!\n" : "Try again!\n";
	std::cout << result;
    return 0;
}
```
Switches are a good option if a particular int is meaningful:
```
#include <iostream>

int main(){
    int day = 4;
	//break stops the loop running, otherwise it continues
	switch (day) {                  
	case 1:
		std::cout << "Monday";
		break;
	case 2:
		std::cout << "Tuesday";
		break;
	case 3:
		std::cout << "Wednesday";
		break;
	case 4:
		std::cout << "Thursday";
		break;
	case 5:
		std::cout << "Friday";
		break;
	case 6:
		std::cout << "Saturday";
		break;
	case 7:
		std::cout << "Sunday";
		break;
    //default sets a condition if none of the switches activate
	default:
		std::cout << "Help!";
	}
    return 0;
}
```
A while loop is useful for repeating an action while a specific condition remains true:
```
    #include <iostream>

    int main(){
    int i = 0;
	//continues while condition is true
	while (i < 5) {
		std::cout << i << '\n';
		i++;
	}
    return 0;
}
```
A do/while loop is different because the do condition will be executed at least once:
```
#include <iostream>

int main(){
    int i = 0;
    do {
        std::cout << i << '\n';
        i++;
    } while (i <5);
    return 0;
}
```
For loops are incredibly useful, one use case is when you want to loop a set number of times.
```
#include <iostream>

int main(){
    //for(s1; s2; s3){
        //code block
    //}
    //s1 is executed once before the code block
    //s2 defines the condition for executing the code block
    //s3 is executed every time after the code block is
    //executed
    for (int i = 0; i < 5; i++) { 
		std::cout << i << "\n";
	}
    return 0;
}
```
Break statements are helpful for stopping a loop, but the continue condition also has its uses. It breaks one iteration of the loop. Let's demonstrate this with a while loop:
```
#include <iostream>

int main(){
    int l = 0;
	while (l < 10) {
		std::cout << l << "\n";
		l++;
		if (l == 4) {
			continue;
		}
		else if (l == 7) {
			break;
		}
	}
    return 0;
}
```