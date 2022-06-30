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
## arrays
Arrays are useful for containing objects you need to access by reference:
```
#include <string>

int main(){
    std::string colors[4] = { "Red", "Blue", "Yellow", "Green" };
	int numSeq[5] = { 10, 20, 30, 40, 50 };
    
    return 0;
}
```
Looping through an array is easy with a for loop:
```
#include <iostream>
#include <string>

int main(){
    std::string colors[4] = { "Red", "Blue", "Yellow", "Green" };
	//i value must be the size of the array
	for (int i = 0; i < 4; i++) {
		std::cout << colors[i] << "\n";
	}
    
    return 0;
}
```
To get the number of values in an array:
```
#include <iostream>

int main(){
    int numSeq[5] = { 10, 20, 30, 40, 50 };
    //sizeof() returns 20, the number of bytes contained in the array
	//there are 5 ints, of 4 bytes each
	std::cout << sizeof(numSeq) << "\n";
	//to get the # of values in an array divide size of array by size of data type
	int getArrayLength = sizeof(numSeq) / sizeof(int);
	std::cout << getArrayLength << "\n";
	return 0;
}
```
Multi-dimensional arrays are also possible:
```
#include <iostream>
#include <string>

int main(){
    //type array2D[x][y] x = # of arrays y = # of elements in arrays
	std::string letters[2][4] = {
		{"A", "B", "C", "D"},
		{"E", "F", "G", "H"}
	};
	std::cout << letters[0][2] << "\n"; //should output C
	return 0;
}
```
Another example, with a demonstration of looping through the array:
```
#include <iostream>

int main(){
    //type array3D[x][y][z] x = # of top level arrays 
	//y = # of arrays within top level arrays
	//z = number of elements within each array
	int numbers[2][2][2] = {
		{
			{1,2},
			{3,4}
		},
		{
			{5,6},
			{7,8}
		}
	};
	//to loop through a multi-dimensional array 1 loop is needed per array dimension
	for (int i = 0; i < 2; i++) {
		for (int j = 0; j < 2; j++) {
			for (int k = 0; k < 2; k++) {
				std::cout << numbers[i][j][k] << "\n";
			}
		}
	}
	return 0;
}
```
Multi-dimensional arrays are also useful for representing grids, like in the classic example Battleship:
```
#include <iostream>

int main(){
    //multidimensional arrays are good for representing grids
	//battleship example
	bool ships[4][4] = {
		{0, 1, 1, 0},
		{0, 0, 0, 0},
		{0, 0, 1, 0},
		{0, 0, 1, 0}
	};
	int hits = 0;
	int numTurns = 0;
	while (hits < 4) { //less than 4 bc there are 4 ships
		int row, column;
		std::cout << "Selecting Coordinates\n";
		std::cout << "Choose a row number between 0 and 3: ";
		std::cin >> row;
		std::cout << "Chose a column number between 0 and 3: ";
		std::cin >> column;
		//check if ship exists
		if (ships[row][column]) {
			ships[row][column] = 0; //if hit, remove it
			hits++; // increase hits counter
			std::cout << "Hit!\n";
			std::cout << (4 - hits) << " left.\n"; //# of ships - current hits 
		}
		else {
			std::cout << "Miss.\n";
		}
		numTurns++; //count how many turns taken
	}
	std::cout << "Victory!\n" << "You won in " << numTurns << " turns.";
	
	return 0;
}
```
## structures
Structures group several related variables into one place. Unlike an array, it can contain different data types:
```
#include <iostream>
#include <string>

int main(){
    struct { //declaration
		int myNum; //member
		std::string myString;
	} myStructure; //structure variable, multiple can be made with , separating
	//to access members of a structure use the dot (.) syntax
	myStructure.myNum = 1; //assign values
	myStructure.myString = "Hello World!";
	std::cout << myStructure.myNum << "\n"; //print
	std::cout << myStructure.myString << "\n";
	
	return 0;
}
```
Named structures can function as data types:
```
#include <iostream>
#include <string>

int main(){
    struct myBook{
		std::string title;
		std::string author;
		int year;
	};
	
	myBook myBook1;
	myBook myBook2;

	myBook1.title = "Pride & Prejudice";
	myBook1.author = "Jane Austen";
	myBook1.year = 1813;

	myBook2.title = "The Idiot";
	myBook2.author = "Fyodor Dostoevsky";
	myBook2.year = 1869;

	std::cout << myBook1.title << '\t' << myBook1.author << "\t\t" << myBook1.year << '\n';
	std::cout << myBook2.title << "\t\t" << myBook2.author << '\t' << myBook2.year << '\n';

	
	return 0;
}
```
