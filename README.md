# ch18ListNode4
What will this code snippet display? Assume that ListNode has been defined with a constructor

Output:

87.6

18.1 Introduction to Linked Lists

Concept

Dynamically allocated data structures may be linked together in memory to form a chain.

 

A linked list is a series of connected nodes, where each node is a data structure. The nodes of a linked list are usually dynamically allocated, used, and then deleted, allowing the linked list to grow or shrink in size as the program runs. If new information needs to be added to a linked list, the program simply allocates another node and inserts it into the series. If a particular piece of information needs to be removed from the linked list, the program deletes the node containing that information.

Advantages of Linked Lists over Arrays and Vectors

Although linked lists are more complex to code and manage than arrays, they have some distinct advantages. First, a linked list can easily grow or shrink in size. In fact, the programmer doesn’t need to know how many nodes will be in the list. They are simply created in memory as they are needed.

One might argue that linked lists are not superior to vectors (found in the Standard Template Library), because they too can expand or shrink. The advantage that linked lists have over vectors, however, is the speed at which a node may be inserted into or deleted from the list. To insert a value into the middle of a vector requires that all the elements after the insertion point be moved one position toward the vector’s end, thus making room for the new value. Likewise, removing a value from a vector requires all the elements after the removal point to be moved one position toward the vector’s beginning. When a node is inserted into or deleted from a linked list, none of the other nodes have to be moved.

The Structure of Linked Lists

Each node in a linked list contains one or more members that hold data. (For example, the data stored in the node may be an inventory record; or it may be a customer information record consisting of the customer’s name, address, and telephone number.) In addition to the data, each node contains a successor pointer that points to the next node in the list. The makeup of a single node is illustrated in Figure 18-1.

 

Figure 18-1 A Node

 

The first node of a nonempty linked list is called the head of the list. To access the nodes in a linked list, you need to have a pointer to the head of the list. Beginning with the head, you can access the rest of the nodes in the list by following the successor pointers stored in each node. The successor pointer in the last node is set to nullptr to indicate the end of the list.

Because the pointer to the head of the list is used to locate the head of the list, we can think of it as representing the list head. The same pointer can also be used to locate the entire list by starting at the head and following the successor pointers, so it is also natural to think of it as representing the entire list. Figure 18-2 illustrates a linked list of three nodes, showing the pointer to the head, the three nodes of the list, and the nullptr value that signifies the end of the list.

Figure 18-2 Nodes Linked by Pointers

 

Figure 18-2 Nodes Linked by Pointers Full Alternative Text

Note

Figure 18-2 depicts the nodes in the linked list as being very close to each other, neatly arranged in a row. In reality, the nodes may be scattered around various parts of memory.

C++ Representation of Linked Lists

To represent linked lists in C++, we need to have a data type that represents a single node in the list. Looking at Figure 18-1, we see that it is natural to make this data type a structure that contains the data to be stored, together with a pointer to another node of the same type. Assuming that each node will store a single data item of type double, we can declare the following type to hold the node:

 

 

struct ListNode

{

   double value;

   ListNode *next;

};

Here ListNode is the type of a node to be stored in the list, the structure member value is the data portion of the node, and the structure member next, declared as a pointer to ListNode, is the successor pointer that points to the next node.

 

The ListNode structure has an interesting property: It contains a pointer to a data structure of the same type and thus can be said to be a type that contains a reference to itself. Such types are called self-referential data types, or self-referential data structures.

 

Having declared a data type to represent a node, we can define an initially empty linked list by defining a pointer to be used as the list head and initializing its value to nullptr:

 

 

ListNode *head = nullptr;

We can now create a linked list that consists of a single node storing 12.5 as follows:

 

 

head = new ListNode;      // allocate new node

head−>value = 12.5;       // store the value

head−>next = nullptr;     // signify end of list

Now let’s see how we can create a new node, store 13.5 in it, and make it the second node in the list. We can use a second pointer to point to a newly allocated node into which the 13.5 will be stored:

 

 

ListNode *secondPtr = new ListNode;

secondPtr−>value = 13.5;

secondPtr−>next = nullptr;   // second node is end of list

head−>next = secondPtr;      // first node points to second

Note that we have now made the second node the end of the list by setting its successor pointer, secondPtr−>next, to nullptr, and we have changed the successor pointer of the list head to point to the second node. Program 18-1 illustrates the creation of a simple linked list.

 

Program 18-1

 

1 // This program illustrates the creation

2 // of linked lists.

3 #include <iostream>

4 using namespace std;

5

 6 struct ListNode

7 {

8    double value;

9    ListNode *next;

10 };

11

12 int main()

13 {

14    ListNode *head = nullptr;

15

16    // Create first node with 12.5

17    head = new ListNode;       // Allocate new node

18    head−>value = 12.5;        // Store the value

19    head−>next = nullptr;      // Signify end of list

20

21    // Create second node with 13.5

22    ListNode *secondPtr = new ListNode;

23    secondPtr−>value = 13.5;

24    secondPtr−>next = nullptr; // Second node is end of list

25    head−>next = secondPtr;    // First node points to second

26

27    // Print the list

28    cout << "First item is " << head−>value << endl;

29    cout << "Second item is " << head−>next−>value << endl;  

30    return 0;

31 }

Program Output

 

First item is 12.5

Second item is 13.5

Using Constructors to Initialize Nodes

Recall that C++ structures can have constructors. It is often convenient to provide the structures that define the type for a list node with one or more constructors, to allow nodes to be initialized as soon as they are created. Recall also that just like regular functions, constructors can be defined with default parameters. It is very common to provide a default parameter of nullptr for the successor pointer of a node. Here is an alternative definition of the ListNode structure:

 

 

struct ListNode

{

   double value;

   ListNode *next;

   // Constructor

   ListNode(double value1, ListNode *next1 = nullptr)

   {

      value = value1;

      next = next1;

   }

};

With this declaration, a node can be created in two different ways:

 

by specifying just its value part and letting the successor pointer default to nullptr, or

 

by specifying both the value part and a pointer to the node that is to follow this one in the list

 

The first method is useful when we are creating a node to put at the end of a linked list, while the second method is useful when the newly created node is to be inserted at a place in the list where it will have a successor.

 

Using this new declaration of a node, we can create the previous list of 12.5 followed by 13.5 with much shorter code:

 

 

ListNode *secondPtr = new ListNode(13.5);

ListNode *head = new ListNode(12.5, secondPtr);

We can actually dispense with the second pointer and write the above code as:

 

 

ListNode *head = new ListNode(13.5);

head = new ListNode(12.5, head);

This code is equivalent to what precedes it because the assignment statement

 

 

head = new ListNode(12.5, head);

is evaluated from right to left: First the old value of head is used in the constructor, and then the address returned from the new operator is assigned to head, becoming its new value.

 

Building a List

Using the constructor version of ListNode, it is very easy to create a list by reading values from a file and adding each newly read value to the beginning of the list of values already accumulated. For example, using numberList for the list head, and numberFile for the input file object, the following code will read in numbers stored in a text file and arrange them in a list:

 

 

ListNode *numberList = nullptr;

double number;

while (numberFile >> number)

{

   // Create a node to hold this number

   numberList = new ListNode(number, numberList);

}

Traversing a List

The process of beginning at the head of a list and going through the entire list while doing some processing at each node is called traversing the list. For example, we would have to traverse a list if we needed to print the contents of every node in the list. To traverse a list, say one whose list head pointer is numberList, we take another pointer ptr and point it to the beginning of the list:

 

 

ListNode *ptr = numberList;

We can then process the node pointed to by ptr by working with the expression *ptr, or by using the structure pointer operator −>. For example, if we needed to print the value at the node, we could write the code

 

 

cout << ptr−>value;

Once the processing at the node is done, we move the pointer to the next node, if there is one, by writing

 

 

ptr = ptr−>next;

thereby replacing the pointer to a node by the pointer to the successor of the node. Thus to print an entire list, we can use code such as

 

 

ListNode *ptr = numberList;

while (ptr != nullptr)

{

   cout << ptr−>value << " ";   // Process node

   ptr = ptr−>next;             // Move to next node

}

Program 18-2 illustrates these techniques by reading a file of numbers, arranging the numbers in a linked list, and then traversing the list to print the numbers on the screen.

 

Program 18-2

 

1 // This program illustrates the building

2 // and traversal of a linked list.

3

4 #include <iostream>

5 #include <fstream>

6 using namespace std;

7

8 struct ListNode

9   {

10      double value;

11      ListNode *next;

12      // Constructor

13      ListNode(double value1, ListNode *next1 = nullptr)

14      {

15        value = value1;

16        next = next1;

17      }

18   };

19

20 int main()

21 {

22    double number;                    // Used to read the file

23    ListNode *numberList = nullptr;   // List of numbers

24

25    // Open the file

26    ifstream numberFile("numberFile.dat");

27    if (!numberFile)

28    {

29       cout << "Error in opening the file of numbers.";

30       exit(1);

31    }

32    // Read the file into a linked list

33    cout << "The contents of the file are: " << endl;

34    while (numberFile >> number)

35    {

36       cout << number << "  ";

37       // Create a node to hold this number

38       numberList = new ListNode(number, numberList);

39    }

40    // Traverse the list while printing

41    cout << endl << "The contents of the list are: " << endl;

42    ListNode *ptr = numberList;

43    while (ptr != nullptr)

44    {

45       cout << ptr−>value << "  "; // Process node

46       ptr = ptr−>next;            // Move to next node

47    }

48    return 0;

49 }

Program Output

 

 

The contents of the file are:

10  20  30  40

The contents of the list are:

40  30  20  10

 

Checkpoint

18.1 Describe the two parts of a node.

 

18.2 What is a list head?

 

18.3 What signifies the end of a linked list?

 

18.4 What is a self-referential data structure?

== We're Using GitHub Under Protest ==

This project is currently hosted on GitHub.  This is not ideal; GitHub is a
proprietary, trade-secret system that is not Free and Open Souce Software
(FOSS).  We are deeply concerned about using a proprietary system like GitHub
to develop our FOSS project. I have a [website](https://bellKevin.me) where the
project contributors are actively discussing how we can move away from GitHub
in the long term.  We urge you to read about the [Give up GitHub](https://GiveUpGitHub.org) campaign 
from [the Software Freedom Conservancy](https://sfconservancy.org) to understand some of the reasons why GitHub is not 
a good place to host FOSS projects.

If you are a contributor who personally has already quit using GitHub, please
email me at **bellKevin@pm.me** for how to send us contributions without
using GitHub directly.

Any use of this project's code by GitHub Copilot, past or present, is done
without our permission.  We do not consent to GitHub's use of this project's
code in Copilot.

![Logo of the GiveUpGitHub campaign](https://sfconservancy.org/img/GiveUpGitHub.png)
