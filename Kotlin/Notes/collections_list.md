## Collections

- ```List```: cannot be modified after you created it.
- ```MutableList```: can be modified after you created it, meaning you can add, remove or update its elements.

## List

```Kotlin
val numbers = listOf(1, 3, 2, 6, 5, 4)
println("List: $numbers")
println("Size: ${numbers.size}")

// Access elements of the list
println("First element: ${numbers[0]}")
println("Second element: ${numbers[1]}")
println("Last index: ${numbers.size - 1}")
println("Last element: ${numbers[numbers.size - 1]}")
println("First: ${numbers.first()}")
println("Last: ${numbers.last()}")

// Use the contains() method
println("Contains 4? ${numbers.contains(4)}")
println("Contains 7? ${numbers.contains(7)}")
println("in 5? ${5 in numbers}")
println("Index of 3? ${numbers.indexOf(3)}")
```

Output:
```
List: [1, 3, 2, 6, 5, 4]
Size: 6
First element: 1
Second element: 3
Last index: 5
Last element: 4
First: 1
Last: 4
Contains 4? true
Contains 7? false
in 3? true
Index of 3? 1
```
#
## MutableList

```Kotlin
val entrees = mutableListOf<String>()
//mutableListOf(1, 2, 3, 23, 46, 93)
println("Entrees: $entrees")

// Add individual items using add()
println("Add noodles: ${entrees.add("noodles")}")
println("Entrees: $entrees")
println("Add spaghetti: ${entrees.add("spaghetti")}")
println("Entrees: $entrees")

// Add a list of items using addAll()
val moreItems = listOf("ravioli", "lasagna", "fettuccine")
println("Add list: ${entrees.addAll(moreItems)}")
println("Entrees: $entrees")

// Remove an item using remove()
println("Remove spaghetti: ${entrees.remove("spaghetti")}")
println("Entrees: $entrees")
println("Remove item that doesn't exist: ${entrees.remove("rice")}")
println("Entrees: $entrees")

// Remove an item using removeAt() with an index
println("Remove first element: ${entrees.removeAt(0)}")
println("Entrees: $entrees")

// Clear out the list
entrees.clear()
println("Entrees: $entrees")

// Check if the list is empty
println("Empty? ${entrees.isEmpty()}")  


val names = listOf("Jessica", "Henry", "Alicia", "Jose")
for (name in names) {
    println("$name - Number of characters: ${name.length}")
}  


val numbers1 = mutableListOf(1, 2, 3, 23, 46, 93)
val numbers2 = listOf(1, 1, 1, 45)
println("${numbers1.contains(2)}")				//Prints: TRUE
println("${numbers1.containsAll(numbers2)}")	//Prints: FALSE
```

```
Entrees: []
Add noodles: true
Entrees: [noodles]
Add spaghetti: true
Entrees: [noodles, spaghetti]
Add list: true
Entrees: [noodles, spaghetti, ravioli, lasagna, fettuccine]
Remove spaghetti: true
Entrees: [noodles, ravioli, lasagna, fettuccine]
Remove item that doesn't exist: false
Entrees: [noodles, ravioli, lasagna, fettuccine]
Remove first element: noodles
Entrees: [ravioli, lasagna, fettuccine]
Entrees: []
Empty? true
Jessica - Number of characters: 7
Henry - Number of characters: 5
Alicia - Number of characters: 6
Jose - Number of characters: 4
```
#
## 1.Predicate

### 1.Usage

```Kotlin
val numbers = listOf(1, 2, 3, 4, 5, 6)
var filteredNumbers = numbers.filter { v -> v < 5}
println("filteredNumbers: $filteredNumbers")
//Because the above statement have one parameter, we can write as below:
filteredNumbers = numbers.filter { it < 5}

var mappedNumbers = numbers.map { num -> num * num }
println("mappedNumbers: $mappedNumbers")

var mappedFilteredNumbers = numbers.filter { v < 5}.map { num * num }
println("mappedFilteredNumbers: $mappedFilteredNumbers")
```

```
filteredNumbers: [1, 2, 3, 4]
mappedNumbers: [1, 4, 9, 16, 25, 36]
mappedFilteredNumbers: [1, 4, 9, 16]
```

### 2.Usage

```Kotlin
data class Person(val age: Int, val name: String)

fun main() {
	var people = listOf<Person>(Person(20, "1.Person"), 
                                Person(46, "2.Person"), 
                                Person(23, "3.Person"), 
                                Person(44, "4.Person"), 
                                Person(56, "5.Person"))
    
    var namesPeople = people.filter { it.age < 40 }.map { it.name }
    println(namesPeople)
}
```

```
[1.Person, 3.Person]
```
#
## 2.Predicate

```Kotlin
fun main() {
    val numbers = listOf(1, 2, 3, 23, 46, 93)
    val check1 = numbers.all { it > 10 }	//Are all elements satisfy the Predicate? Output: FALSE
    val check2 = numbers.any { it > 10 }	//Does any of these elements satisfy the Predicate? Output: TRUE
    val count = numbers.count { it > 10 }	//Number of elements that satisfy the Predicate? Output: 3
    val num = numbers.find { it > 10 }		//Returns the first element that matches the Predicate. Output : 23
    val num2 = numbers.find { it > 100 }		//Returns the first element that matches the Predicate. Output : Null
}
```
#
### 3. Iterator
```Kotlin
fun main() {
    val theList = listOf("one", "two", "three", "four")
    
    val itr = theList.listIterator() 
    while (itr.hasNext()) {
        println(itr.next())
    }

    for ( temp in theList ) {
        println(person)
    }
    theList.forEach { println(temp) }
}
```
```
one
two
three
four
```

#
### 4. List Addition oR Subtraction
``` Kotlin
fun main() {
    val firstList = listOf("one", "two", "three")
    val secondList = listOf("four", "five", "six")
    val resultList = firstList + secondList

    val resultList2 = firstList - secondList
    
    println(resultList)
    println(resultList2)
}
```
```
[one, two, three, four, five, six]
[two, three]
```

#
### 5. Slicing a List
``` Kotlin
fun main() {
    val theList = listOf("one", "two", "three", "four", "five")
    val resultList = theList.slice(2..4)
    
    println(resultList)
}
```
```
[three, four, five]
```
#
### 6. Removing null a List
``` Kotlin
fun main() {
    val theList = listOf("one", "two", null, "four", "five")
    val resultList = theList.filterNotNull()
    
    println(resultList)
}
```
```
[one, two, four, five]
```
#
### 7. Grouping List Elements
``` Kotlin
fun main() {
    val theList: List<Int> = listOf(10, 12, 30, 31, 40, 9, -3, 0)
    val resultList: Map<Int,List<Int>> = theList.groupBy{ it % 3}
}
```
```
{1=[10, 31, 40], 0=[12, 30, 9, -3, 0]}
```

#
### 8. Chunking List Elements
``` Kotlin
fun main() {
    val theList: List<Int> = listOf(10, 12, 30, 31, 40, 9, -3, 0)
    val resultList: List<List<Int>> = theList.chunked(3)
    println(resultList)
}
```
```
[[10, 12, 30], [31, 40, 9], [-3, 0]]
```


