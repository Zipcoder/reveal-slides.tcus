#Intro to Algorithms


-
##What is an algorithm?

An algorithm is a set of instructions for accomplishing a task.

Every piece of code could be called an algorithm

Some can speed up your code or some solve interesting problems

-
-

##Different types of algorithms

Searching

Graph algorithms to calculate the shortest route to your destination

To write an AI that plays chess

	f(x) = x × 2
		What is f(5)?



-
-
#Big O notation



-
-
##Big O notation 
	
Big O notation is a mathematical notation that describes the limiting behavior of a function when the argument tends towards a particular value or infinity.

For our purposes, Big O notation is used to classify algorithms according to how their running time or space requirements grow as the input size grows.


-
-
##Big O notation (continued)

Big O notation characterizes functions according to their growth rates: different functions with the same growth rate may be represented using the same O notation.

The letter O is used because the growth rate of a function is also referred to as the order of the function.

A description of a function in terms of big O notation usually only provides an upper bound on the growth rate of the function. 


-
-
##Big O notation (continued)
	

In mathematics, the 'logarithm' is the inverse function to exponentiation.

In other words, the logarithm of a given number x is the exponent to which another fixed number, the base x, must be raised, to produce that number x.

<span class="texhtml">1000 = 10 × 10 × 10 = 10<sup>3</sup></span>

"logarithm to base 10" of 1000 is 3


The logarithm of x to base b is denoted as
    <span class="texhtml">log<sub><i>b</i></sub> (<i>x</i>)</span>

Hence, <span class="texhtml">log<sub><i>10</i></sub> (<i>1000</i>) = 3</span>


-
-
##Big O notation (continued)
	
<img src="img/BigOgraph.jpeg">

-
-
##Big O notation (continued)
	
<img src="img/BigONotationSummary.png">


-
-
#Linear search	

-
-
##Linear search
	
...stuff

-
-
##Linear search
Java implementation for linear search x in arr[].
If x is present  then return its  location.  otherwise, return -1

    class LinearSearch
    {
        {
            for (int i = 0; i < n; i++)
            {
                if (arr[i] == x)
                    return i;
            }
            return -1;
        }
    } 

The time complexity of above algorithm is O(n).

-
-

##Linear search(continued)

Linear search is rarely used practically because other search algorithms, (such as the binary search algorithm and hash tables) allow significantly faster searching in comparison.

-
-
##Binary Search

-
-
##Binary Search
Suppose you’re searching for a person in the phone book (what an old-fashioned sentence!). Their name starts with K.

Would you start at the beginning or would you jump to the middle?<br>
    *Facebook login: <br>
        *Search username database.<br>
        *Your name is karlmegeddon<br>
    *How would you solve these search problems? binary search

-
-
##Binary Search (continued)
Binary search is an algorithm; its input is a sorted list of elements (I’ll explain later why it needs to be sorted).

If an element you’re looking for is in that list, binary search returns the position where it’s located.

Otherwise, binary search returns null.

Ex: Looking for companies in a phone book with binary search 

-
-
##Binary Search (continued)
Here’s an example of how binary search works.

I’m thinking of a number between 1 and 100:

-You have to try to guess my number in the fewest tries possible.

-With every guess, I’ll tell you if your guess is too low, too high, or correct.

-Suppose you start guessing like this: 1, 2, 3, 4 .... Here’s how it would go.

-
-
##Binary Search (continued)

A bad approach to number guessing

-That technique is simple search 

-With each guess, you’re eliminating only one number.

-If my number was 99, it could take you 99 guesses to get there!

-
-
##Binary Search (continued)
A better way to search

With binary search, you guess the middle number and eliminate half the remaining numbers every time:

    100 items -> 50 -> 25 -> 13 -> 7 -> 4 -> 2 -> 1
    Eliminate half the numbers everytime
        
Imagine a dictionary with 240,000 words:

        Simple Search = ? steps
        Binary search = ? steps
        
            
            

-
-
##Binary Search (continued)
Java implementation of recursive Binary Search

    class BinarySearch {
        int binarySearch(int arr[], int l, int r, int x) {
            if (r>=l) {
                int mid = l + (r - l)/2;
                if (arr[mid] == x)
                   return mid;
    
                if (arr[mid] > x)
                   return binarySearch(arr, l, mid-1, x);
     
                return binarySearch(arr, mid+1, r, x);
            }
            return -1;
        }
     
        public static void main(String args[]) {
            BinarySearch ob = new BinarySearch();
            int arr[] = {2,3,4,10,40};
            int n = arr.length;
            int x = 10;
            int result = ob.binarySearch(arr,0,n-1,x);
            if (result == -1)
                System.out.println("Element not present");
            else
                System.out.println("Element found at index " + result);
        }
    }
-
-
# To be continued...
