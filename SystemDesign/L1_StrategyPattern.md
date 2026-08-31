STRATEGY PATTERN :

    The Strategy Pattern is a behavioral design pattern that lets you define multiple algorithms/behaviors and switch between them at runtime.
    Strategy Pattern defines a family of algorithms, encapsulates each algorithm into separate classes, and makes them interchangeable at runtime.
    It helps eliminate large conditional statements and follows the Open/Closed Principle.
    Strategy Pattern is a behavioral design pattern that defines a family of interchangeable algorithms, encapsulates each algorithm behind a common interface, and allows the client or context to select and change the algorithm at runtime. 
    It promotes composition over inheritance and helps avoid large conditional statements.



1. What is a family of algorithms?

        A family means a group of related ways to solve the same problem.

2. What does encapsulate each algorithm mean?

It means:

    Put each algorithm's logic inside its own separate class.



Issues withot startegy Pattern


Problem 1: Large if-else / switch statements ❌

    Every new behavior requires:

        else if (customerType.equals("VIP")) {
        // VIP logic
        }

Over time:

        Code becomes difficult to read
        Code becomes difficult to maintain
        Logic becomes messy


Problem 2: Violates Open/Closed Principle ❌

    Open/Closed Principle says:

        Open for extension, closed for modification.

Without Strategy:

Suppose you want to add:

    VIP Discount

You must modify:

    DiscountService

        Existing working code is changed.

        That can introduce bugs.                    



Problem 3: One class has too many responsibilities ❌

Without Strategy:

    DiscountService

knows how to calculate:

        Regular discounts
        Premium discounts
        Festival discounts
        VIP discounts

    So one class contains multiple independent algorithms.
    
    This violates the Single Responsibility Principle.                                                                          



Problem 4: Difficult to test ❌

    Suppose you want to test only:

        VIP discount logic

Without Strategy, you must go through:

    DiscountService.calculateDiscount("VIP", amount);

The class contains all other logic too.



Problem 5: Difficult to change behavior at runtime ❌

Imagine this situation:

    Normal Day → Regular Discount
    
    Festival Day → Festival Discount

Without Strategy, your main class must constantly decide:

    if (festival) {
    // Festival logic
    } else {
    // Regular logic
    }

    

Problem 6: Changes can affect unrelated logic ❌

Suppose this huge method exists:

        calculateDiscount()

You modify the VIP section.

⚠️ Possible accidental impact:

        Regular Discount
        Premium Discount
        Festival Discount

Because everything lives in the same method/class.




| Without Strategy Pattern                     | With Strategy Pattern                     |
| -------------------------------------------- | ----------------------------------------- |
| Large `if-else`                              | Separate strategy classes                 |
| Logic mixed together                         | Logic isolated                            |
| Modify existing class for new behavior       | Add a new strategy                        |
| Difficult to maintain as it grows            | Easier to maintain                        |
| Difficult to test individual algorithms      | Each strategy can be tested independently |
| Tight coupling                               | Looser coupling                           |
| Behavior selection mixed with implementation | Selection separated from implementation   |


Use it when:

    You have multiple ways of performing the same operation.
    Those algorithms are interchangeable.
    You have growing if-else / switch logic based on behavior.
    You need to change behavior at runtime.
    You want to isolate algorithms into independently testable classes.
    New algorithms are expected to be added frequently.


WITHOUT STRATERGY :


```java

class SortingContext {

    public void performSort(String sortingType, int[] array) {

        if (sortingType.equals("BUBBLE")) {

            System.out.println("Sorting using Bubble Sort");

            bubbleSort(array);

        } else if (sortingType.equals("MERGE")) {

            System.out.println("Sorting using Merge Sort");

            mergeSort(array);

        } else if (sortingType.equals("QUICK")) {

            System.out.println("Sorting using Quick Sort");

            quickSort(array);

        } else {
            throw new IllegalArgumentException("Invalid sorting type");
        }
    }


    private void bubbleSort(int[] array) {

        for (int i = 0; i < array.length - 1; i++) {

            for (int j = 0; j < array.length - i - 1; j++) {

                if (array[j] > array[j + 1]) {

                    int temp = array[j];
                    array[j] = array[j + 1];
                    array[j + 1] = temp;
                }
            }
        }
    }


    private void mergeSort(int[] array) {

        // Merge Sort implementation

        System.out.println("Merge Sort logic executed");
    }


    private void quickSort(int[] array) {

        // Quick Sort implementation

        System.out.println("Quick Sort logic executed");
    }
}


```





WITH STRATEEGY

```java

class SortingContext {
private SortingStrategy sortingStrategy;

    public SortingContext(SortingStrategy sortingStrategy) {
        this.sortingStrategy = sortingStrategy;
    }

    public void setSortingStrategy(SortingStrategy sortingStrategy) {
        this.sortingStrategy = sortingStrategy;
    }

    public void performSort(int[] array) {
        sortingStrategy.sort(array);
    }
}

// SortingStrategy.java
interface SortingStrategy {
void sort(int[] array);
}

// BubbleSortStrategy.java
class BubbleSortStrategy implements SortingStrategy {
@Override
public void sort(int[] array) {
// Implement Bubble Sort algorithm
System.out.println("Sorting using Bubble Sort");
// Actual Bubble Sort Logic here
}
}

// MergeSortStrategy.java
class MergeSortStrategy implements SortingStrategy {
@Override
public void sort(int[] array) {
// Implement Merge Sort algorithm
System.out.println("Sorting using Merge Sort");
// Actual Merge Sort Logic here
}
}

// QuickSortStrategy.java
class QuickSortStrategy implements SortingStrategy {
@Override
public void sort(int[] array) {
// Implement Quick Sort algorithm
System.out.println("Sorting using Quick Sort");
// Actual Quick Sort Logic here
}
}

// Client.java
public class Client {
public static void main(String[] args) {
        // Create SortingContext with BubbleSortStrategy
        SortingContext sortingContext = new SortingContext(new BubbleSortStrategy());
        int[] array1 = {5, 2, 9, 1, 5};
        sortingContext.performSort(array1); // Output: Sorting using Bubble Sort

        // Change strategy to MergeSortStrategy
        sortingContext.setSortingStrategy(new MergeSortStrategy());
        int[] array2 = {8, 3, 7, 4, 2};
        sortingContext.performSort(array2); // Output: Sorting using Merge Sort

        // Change strategy to QuickSortStrategy
        sortingContext.setSortingStrategy(new QuickSortStrategy());
        int[] array3 = {6, 1, 3, 9, 5};
        sortingContext.performSort(array3); // Output: Sorting using Quick Sort
    }
}


```


ACTORS:


1. Context

       Acts as an intermediary between the client and the strategy, delegating tasks to the selected strategy.
        Holds a reference to a strategy object and uses it to perform operations.
        Allows switching strategies without changing its own code.

   2. Strategy Interface

             Defines a common interface that all concrete strategies must implement.
          Ensures consistency so all strategies are interchangeable.
          Promotes flexibility by decoupling context from implementations.
 3. Concrete Strategies
    
                   Provide specific implementations of the strategy interface with different algorithms or behaviors.
    
             Encapsulate the actual logic of each algorithm.
             Can be selected and replaced based on requirements.

   4. Client

            Responsible for selecting and configuring the appropriate strategy for the context.

          Decides which strategy to use based on the problem.
          Passes the chosen strategy to the context for execution.



Without Strategy Pattern ❌

The if-else does two jobs:

    Decides which algorithm to use
    Contains/controls the algorithm implementation

With Strategy Pattern ✅

    You may still use if-else to select the strategy:


But can we remove the selection if-else too?

Yes! In real applications, we often use a Map.

        Map<String, SortingStrategy> strategies = new HashMap<>();
        
        strategies.put("BUBBLE", new BubbleSortStrategy());
        strategies.put("MERGE", new MergeSortStrategy());
        strategies.put("QUICK", new QuickSortStrategy());
        
        SortingStrategy strategy = strategies.get(type);
        
        strategy.sort(array);

Now there is no large if-else.