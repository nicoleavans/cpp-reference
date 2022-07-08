# c++ basics

## inputs, outputs, and strings
At the very simplest, we output text:
```c++
#include <iostream>

int main(){
    std::cout << "Hello World!\n";
    return 0;
}
```
We can also take in text from the user:
```c++
#include <iostream>

int main(){
    int a;
    std::cout << "Type a number: ";
    std::cin >> a;
    return 0;
}
```
This, however, doesn't allow for any spaces. For that, we need getline(). We'll also use a string to store the input:
```c++
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
```c++
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
```c++
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
```c++
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
```c++
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
```c++
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
```c++
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
```c++
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
```c++
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
```c++
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
```c++
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
```c++
#include <string>

int main(){
    std::string colors[4] = { "Red", "Blue", "Yellow", "Green" };
	int numSeq[5] = { 10, 20, 30, 40, 50 };
    
    return 0;
}
```
Looping through an array is easy with a for loop:
```c++
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
```c++
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
```c++
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
```c++
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
```c++
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
```c++
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
```c++
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
## references and pointers
References and pointers give you the ability to manipulate data in the computers memory which can reduce code and improve performance, a main distinction between c++ and other languages, like java.

A reference variable is a reference to an existing variable created with the & operator:
```c++
#include <iostream>
#include <string>

int main()
{
    std::string food = "Macaroni";
    std::string& meal = food; //putting & in front of a variable denotes address of
    std::cout << food << "\n"; //should output macaroni
    std::cout << meal << "\n"; //should output macaroni

    return 0;
}
```
The & operator can also be used to get the memory address of a variable. A pointer is a variable that stores the memory address as its value:
```c++
#include <iostream>
#include <string>

int main()
{
    std::string food = "Macaroni";
    std::string* ptr = &food; //created with * operator, stores address
    std::cout << food << "\n"; //output value
    std::cout << &food << "\n"; //outputs mem address 0x78fde0
    std::cout << ptr << "\n"; //output mem address w/ pointer

    return 0;
}
```
Pointers can also be used to get and change the value of the variable:
```c++
#include <iostream>
#include <string>

int main()
{
    std::string food = "Macaroni";
    std::string* ptr = &food; //created with * operator, stores address
    std::cout << *ptr << "\n"; // dereference : output value from pointer
    *ptr = "Hamburger"; //change the value of the pointer
    std::cout << *ptr << "\n"; //output new value of pointer
    std::cout << food << "\n"; //output new value of food variable

    return 0;
}
```
## function basics
Basic functions follow take this format:
```c++
#include <iostream>

void myFunction() { //declaration
	std::cout << "I just got executed!\n"; //definition
}

int main()
{
    myFunction(); //can be called multiple times
    return 0;
}
```
Functions also frequently function by taking parameters. A function can take one or more parameters, and those parameters may also have a default value. Frequently, function declarations are made above the main class, with the definitions following after. Here are a few examples:
```c++
#include <iostream>

void myCountry(std::string country = "Sweden"); //default param is part of declaration
int mySum(int x, int y);
void swapNums(int& x, int& y); //you can pass a reference to a function
void arrayFunction(int myNums[5]);

int main()
{
    myCountry(); //prints Sweden
    myCountry("USA"); //prints USA
    std::cout << mySum(2, 3) << "\n"; //should return 5
    int z = mySum(4, 6); // functions can create variables
    std::cout << z << "\n";
    int num1 = 10;
    int num2 = 20;
    std::cout << "Before swap: " << num1 << " " << num2 << "\n";
    swapNums(num1, num2);
    std::cout << "After swap: " << num1 << " " << num2 << "\n";
    int myNums[5] = { 1,2,3,4,5 };
    arrayFunction(myNums);
    int int1 = 2;
    int int2 = 7;
    return 0;
}

void myCountry(std::string country) { //default param defined above
	std::cout << country << '\n';
}
//if you want it to return a value, data type can be used instead of void
int mySum(int x, int y) {
	return x + y; //use the return keyword inside the function
}
void swapNums(int& x, int& y) { //useful for changing the value of arguments
	int z = x;
	x = y;
	y = z;
}
void arrayFunction(int myNums[5]) {
	for (int i = 0; i < 5; i++) {
		std::cout << myNums[i] << "\n";
	}
}
```
Function overloading involves multiple functions that the same name with different parameters. Overloaded functions such as sum could be overloaded for a lot of types. Typically, the functions have the same body, but they don't have to.
```c++
#include <iostream>

int sum(int x, int y);
float sum(float x, float y);
double sum(double x, double y);

int main()
{
    int int1 = 3;
    int int2 = 2;
    std::cout << int1 << " + " << int2 << " = " << sum(int1, int2) << "\n";
    float float1 = 3.14;
    float float2 = 2.97;
    std::cout << float1 << " + " << float2 << " = " << sum(float1, float2);
    std::cout << "\n";
    double double1 = 3.78954245335;
    double double2 = 5.12555488632;
    std::cout << double1 << " + " << double2 << " = " << sum(double1, double2);
    return 0;
}

int sum(int x, int y) {
	return x + y;
}
float sum(float x, float y) {
	return x + y;
}
double sum(double x, double y) {
	return x + y;
}
```
### recursive functions
Recursive functions are useful for tasks such as sorting elements or factorials. They are essentially functions that call themselves:
```c++
#include <iostream>

int sum(int k);

int main()
{
    int result = sum(5);
    std::cout << result;
    return 0;
}

//when called, adds k to the sum of all numbers smaller than k, returns result
int sum(int k) {
	if (k > 0) {
		return k + sum(k - 1);
	}
	else {
		return 0;
	}
}
```
### efficiency considerations
Calling a function with parameters taken by value causes copies of the values to be made. this is relatively inexpensive for fundamental types such as int, but if the parameter is of a large compound type, it may result on certain overhead. for example, consider:
```c++
string concatenate(string a, string b) {
	return a + b;
}
```
This function takes two strings (by value) and returns the result of concatenating them. by passing the arguments by value, the function forces a and b to be copies of the arguments passed to the function when it is called. and if these are long strings, it may mean copying large quantities of data just for the function called. This copy can be avoided if both parameters are references:
```c++
string eConcatenate(string& a, string& b) {
	return a + b;
}
```
Arguments by reference do not require a copy, and is more efficient. However, functions with reference parameters are generally perceived as functions that modify the arguments passed. The solution is for the function to guarantee that its reference parameters are not going to be modified by this function, by qualifying them as constant.
```c++
string iConcatenate(const string& a, const string& b) {
	return a + b;
}
```
In some cases, const references may be less efficient! Specifically good for larger parameter types.
## templates
C++ has the ability to define functions with generic types, known as function templates. defining a function template follows the same syntax as a regular function, except that it is preceded by the template keyword and a series of template parameters enclosed in <> :
```c++
#include <iostream>

template <class T>
T sum(T a, T b) {
	T result; //T is also used to declare a local variable of that generic type
	result = a + b;
	return result;
}

int main()
{
    int i = 5, j = 6, k;
    double f = 2.0, g = 0.5, h;
    k = sum<int>(i, j);
    h = sum<double>(f, g);
    std::cout << k << "\n" << h << "\n";
    //since result in the original function is also defined with generic type T
    //the compiler is capable of deducing the data type
    k = sum(i, j);
    h = sum(f, g);
    std::cout << k << "\n" << h << "\n";
    return 0;
}
```
## classes, objects, and constructors
The basic structure of a class is demonstrated here:
```c++
#include <iostream>

class MyClass { //the class
public: //access specifier, public means accessible outside the class
	void myMethod() { //method/function defined in class
		std::cout << "Hello World!\n";
	}
	int myNum; //attribute (int variable)
	std::string myString; //attribute (string variable)
};

int main()
{
    MyClass myObj; //create an object of MyClass
    //access attributes and set values
    myObj.myNum = 10;
    myObj.myString = "some text";
    std::cout << myObj.myNum << "\n" << myObj.myString << "\n"; //print values
    myObj.myMethod();
    return 0;
}
```
Constructors are special methods that are automatically called when the class object is created:
```c++
#include <iostream>

class MyClass {
public:
	MyClass() { //constructor
		std::cout << "Hello World! (again)\n";
	} //constructor has the same name as the class, has no return value
};

int main()
{
    MyClass myObj; //calls the constructor
    return 0;
}
```
Constructors can also take parameters:
```c++
#include <iostream>

class Car {
public:
	std::string brand;
	std::string model;
	int year;
	Car(std::string x, std::string y, int z){ 
		brand = x;
		model = y;
		year = z;
	}
};

int main()
{
    Car car1("Chevrolet", "Corvette", 2022);
	std::cout << car1.brand << " " << car1.model << " " << car1.year << "\n";
    return 0;
}
```
Constructors can also be defined outside of the class:
```c++
#include <iostream>

class Car {
public:
	std::string brand;
	std::string model;
	int year;
	Car(std::string x, std::string y, int z); //must be defined
};

Car::Car(std::string x, std::string y, int z) {
	brand = x;
	model = y;
	year = z;
}

int main()
{
    Car car1("Chevrolet", "Corvette", 2022);
	std::cout << car1.brand << " " << car1.model << " " << car1.year << "\n";
    return 0;
}
```
## encapsulation, inheritance, and polymorphism
Encapsulation makes sure that sensitive data is hidden from users. This typically necessitates functions to access that data:
```c++
#include <iostream>

class Employee {
private:
	int salary;
public:
	//getters and setters needed to access private variables
	void setSalary(int s) { //setter
		salary = s;
	}
	int getSalary() {
		return salary;
	}
};

int main()
{
    Employee e1;
	e1.setSalary(50000);
	std::cout << e1.getSalary() << '\n';
    return 0;
}
```
Inheritance is useful for code reusablity, like attributes and methods.
```c++
#include <iostream>

class MyClass {
public:
	void myFunction() {
		std::cout << "Some content in parent class. \n";
	}
};
//derived class (child)
class MyChild : public MyClass {};
//derived class (grandchild)
class MyGrandChild : public MyChild {};

int main()
{
    MyGrandChild myObj;
    myObj.myFunction();
    return 0;
}
```
Classes can also be derived from more than one base class:
```c++
#include <iostream>

class MyClass {
public:
	void myFunction() {
		std::cout << "Some content in parent class. \n";
	}
};

class MyOtherClass {
public:
	void myOtherFunction() {
		std::cout << "Some content in another class. \n";
	}
};

//derived class
class MyChildClass : public MyClass, public MyOtherClass {};

int main()
{
    MyChildClass myObj2;
    myObj2.myFunction();
    myObj2.myOtherFunction();
    return 0;
}
```
The access specifier protected allows access in inherited class:
```c++
#include <iostream>

class ProtectedEmployee {
protected:
	int salary;
};
//derived class
class Programmer : public ProtectedEmployee {
public:
	int bonus;
	void setSalary(int s) {
		salary = s;
	}
	int getSalary() {
		return salary;
	}
};

int main()
{
    Programmer myEmp;
    myEmp.setSalary(70000);
    myEmp.bonus = 15000;
    std::cout << "Salary: " << myEmp.getSalary() << "\n";
    std::cout << "Bonus: " << myEmp.bonus << "\n";
    return 0;
}
```
Polymorphism occurs when many classes are related by inheritance:
```c++
#include <iostream>

class Animal {
public:
	void animalSound() {
		std::cout << "The animal makes a sound \n";
	}
};
//derived class
class Pig : public Animal {
public:
	void animalSound() { //overrides parent class method
		std::cout << "The pig says: oink oink \n";
	}
};
class Dog : public Animal {
public:
	void animalSound() { //overrides parent class method
		std::cout << "The dog says: woof woof \n";
	}
};

int main()
{
    Animal myAnimal;
    Pig myPig;
    Dog myDog;
    
    myAnimal.animalSound();
    myPig.animalSound();
    myDog.animalSound();
    return 0;
}
```
## files
Working with files directly requires the iostream and fstream libraries. There are three classes in the fstream library:
- ofstream - creates and writes to files
- ifstream - reads from files
- fstream - a combination of ofstream and ifstream: creates, reads, and writes to files

To create and open a text file:
```c++
#include <iostream>
#include <fstream>

int main()
{
    std::ofstream MyFile("filename.txt");
	//write to the file
	MyFile << "Hello World!"; // << insertion operator used to write to file
	MyFile.close(); //closing the file is good practice and can clean up mem space
	return 0;
}
```
To read a file, use either ifstream or fstream and the name of the file:
```c++
#include <iostream>
#include <fstream>

int main()
{
    //create a text string, used to output to the text file
	std::string myText;
	//read from text file
	std::fstream MyReadFile("filename.txt");
	//use a whlie loop together with the getline() function to read line by line and print contents
	while (getline(MyReadFile, myText)) {
		std::cout << myText << '\n'; //output the text from the file
	}
	MyReadFile.close(); //close the file
	return 0;
}
```
To read in information from a file and convert it to a string, a loop can be used:
```c++
#include <iostream>
#include <fstream>

int main()
{
    std::string myText;
	//read from text file
	std::fstream MyReadFile("filename.txt");
	//use a whlie loop together with the getline() function to read 
	//line by line and print contents
	while (getline(MyReadFile, myText)) {
		std::cout << myText << "\n"; //output the text from the file
	}
	MyReadFile.close(); //close the file
}
```
## exception handling
Exception handling in c++ consists of try, throw, and catch.
- Try defines code to be tested for errors while being executed.
- Throw keyword throws an exception when a problem is found, allowing custom errors
- Catch statement allows code to be executed if an error occurs.

```c++
#include <iostream>

int main()
{
    try {
		int age = 15;
		//TEST int age = 19;
		if (age >= 18) { //block of code to try
			std::cout << "Access granted - you are old enough.\n";
		}
		else {
			throw (age); //throw an exception when a problem arises
		}
	}
	catch (int myNum) {
		std::cout << "Access denied - You must be at least 18 years old. \n";
		std::cout << "Age is: " << myNum << "\n";
	}   
}
```
