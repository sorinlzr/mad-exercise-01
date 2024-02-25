# MAD - Exercise 01
## Tasks
* Watch the Kotlin Crashcourse Video in Moodle or complete the tutorials **[Introduction to programming in Kotlin](https://developer.android.com/courses/pathways/android-basics-compose-unit-1-pathway-1)** and **[Kotlin fundamentals](https://developer.android.com/courses/pathways/android-basics-compose-unit-2-pathway-1
)**.
* Answer the questions inside this Readme.md file and push it to your repository.
* Submit your coding solution of the Number Guessing Game inside the repository.
* Submit the link to your repository in Moodle.

## Questions
### Describe how Kotlin handles null safety. What are nullable types and non-null types in Kotlin? (0,5 points)

In kotlin, the type system is designed to eliminate the danger of null references, often referred to as [The Billion Dollar Mistake](https://en.wikipedia.org/wiki/Null_pointer#History). 
This is achieved by distinguishing between nullable and non-nullable types. 
Therefore types are non-nullable by default, which means you cannot assign null to them

for example, a regular variable of type String cannot hold null:

```kotlin
var a: String = "abc"       // regular initialization means non-nullable by default
a = null                    // this would cause a compilation error
```

to allow nulls, you can declare a variable as a nullable string by appending a `?` to its type:
```kotlin
var b: String? = "abc"  // can be set to null
b = null                // ok
print(b)
```

if you want to access a property on a nullable reference, 
you need to handle the null condition explicitly to avoid compilation error:

```kotlin
val l = a.length                            // ok
val l = b.length                            // error: variable 'b' can be null
val l = if (b != null) b.length else -1     
```

### What are lambda expressions and higher order functions in Kotlin? Why would you store a function inside a variable? (0,5 points)

lambda expressions in kotlin are essentially anonymous functions that you can treat as values, 
they are defined using curly braces `{}` and the arrow operator `->`
```Kotlin
val sum = { x: Int, y: Int -> x + y }               // lambada expression
```

A higher-order function in kotlin is a function that takes functions as parameters or returns a function
```Kotlin
fun applyTwice(f: (Int) -> Int, x: Int): Int {
    return f(f(x))
}

val result = applyTwice({ x -> x * 2 }, 10)
println(result)             // prints 40
```

You can store a function inside a variable to pass it as a parameter to another function:
```kotlin
val ascendingComparator: (Int, Int) -> Int = { a, b -> a - b }
val descendingComparator: (Int, Int) -> Int = { a, b -> b - a }

fun sortWithComparator(list: List<Int>, comparator: (Int, Int) -> Int): List<Int> {
    // sort the list using the provided comparator function
}

val sortedAscending = sortWithComparator(listOf(3, 1, 2), ascendingComparator)
val sortedDescending = sortWithComparator(listOf(3, 1, 2), descendingComparator)
```

or to return it from another function:
```kotlin
fun getOperation(operationType: String): (Int, Int) -> Int {
    return when (operationType) {
        "add" -> { a, b -> a + b }
        "subtract" -> { a, b -> a - b }
        else -> throw IllegalArgumentException("unsupported operation")
    }
}

val addFunction = getOperation("add")
val subtractFunction = getOperation("subtract")

val result1 = addFunction(3, 2)         // prints 5
val result2 = subtractFunction(3, 2)    // prints 1
```

Storing functions inside variables is also common when working with callback-based APIs. 
For example when registering event listeners in android development you often store callback functions inside variables to be invoked when the event occurs:
```kotlin
val clickListener: (View) -> Unit = { view ->
    // handle click 
}

button.setOnClickListener(clickListener)
```

### Provide a solution for the following number guessing game inside `App.kt`. (3 points)

## Number Guessing Game in Kotlin
The game is a simple number guessing game. The task is to generate a random, max 9-digit, number. The number must **not contain repeating digits**. Valid digits are 1-9.
Ask the user to guess the max 9-digit number. The game is finished when the user guesses the correct digits in the correct order.
In each round, the user gets feedback about the number of correct digits and the number of correct digits in the correct position.
The output should be in the format "n:m", where "n" is the number of digits guessed correctly regardless of their position, 
and "m" is the number of digits guessed correctly at their correct position. Here are some examples:

This example shows the game flow with 4-digits to guess (the default argument)

Generated number: 8576
-	User input: 1234, Output: 0:0
-	User input: 5678, Output: 4:1
-	User input: 5555, Output: 1:1
-	User input: 3586, Output: 3:2
-	User input: 8576, Output: 4:4 -> user wins

Take a look into the provided code structure in `src/main/kotlin/App.kt`. Implement the following methods (lambda expressions):
- _playNumberGame(digitsToGuess: Int = 4)_ (1 point): The main game loop that handles user input and game state. Make use of _generateRandomNonRepeatingNumber_ and _checkUserInputAgainstGeneratedNumber_ here. This function also utilizes a default argument 
- _generateRandomNonRepeatingNumber_ (1 point): A lambda expression that generates a random number with non-repeating digits of a specified length.
- _checkUserInputAgainstGeneratedNumber_ (1 point): A lambda expression that compares the user's input against the generated number and provides feedback.

``CompareResult.kt`` This class is a data structure which helps with bundling the result of the comparison of the user input and the generated number. Look at the toSting() and use it in your output.

Run the project with `./gradlew run` and test your implementation with the provided tests in `src/test/kotlin/AppTest.kt` with `./gradlew test`.

# Project Structure
The project is structured into two main Kotlin files:

**App.kt**: Contains the main game logic and functions.

**AppTest.kt**: Contains unit tests for the various functions in App.kt.

