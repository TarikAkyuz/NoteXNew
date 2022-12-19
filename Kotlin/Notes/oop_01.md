## Simple Class

``` Kotlin
class Lamp {

    // property (data member)
    private var isOn: Boolean = false

    // member function
    fun turnOn() {
        isOn = true
    }

    // member function
    fun turnOff() {
        isOn = false
    }

    fun displayLightStatus() {
        if (isOn == true)
            println("lamp is on.")
        else
            println("lamp is off.")
    }
}

fun main(args: Array<String>) {

    val lamp = Lamp()
    lamp.displayLightStatus()
}
```
```
lamp is off.
```

#
## Constructors
### Primary Constructor

``` Kotlin
class Person(val firstName: String, var age: Int) {

}
//OR
class Person(fName: String, personAge: Int) {
    val firstName: String
    var age: Int

    // initializer block
    init {
        firstName = fName.capitalize()
        age = personAge
    }
}
// OR
class Person(fName: String, personAge: Int) {
    val firstName = fName.capitalize()
    var age = personAge

    // initializer block
    init {
    }
}

fun main(args: Array<String>) {

    val person1 = Person("Joe", 25)

    println("First Name = ${person1.firstName}")
    println("Age = ${person1.age}")
}
```
```
First Name = Joe
Age = 25
```
#
### Secondary Constructor

``` Kotlin
class Person(_firstName: String = "UNKNOWN", _age: Int = 0) {
    val firstName: String
    var age: Int
    
    
    init {
        firstName = _firstName.capitalize()
        age = _age        
    }
    
    //You have to call primary constructor in this case with this keyword   
    constructor(_firstName: String): this(_firstName, 0) {
        
    }
    
    override fun toString(): String {
        return "Name: ${firstName}, Age: ${age}"
    }
}
//OR
class Person {
    val firstName: String
    var age: Int
    
    init {
        println("Init calls")
    }
    
    constructor(_firstName: String) {
        firstName = _firstName.capitalize()
        age = 0
    }
    
    override fun toString(): String {
        return "Name: ${firstName}, Age: ${age}"
    }
}

fun main() {
    val person1 = Person("joe")
    println(person1)
    
}
```
```
Init calls
Name: Joe, Age: 0
```
