open class Animal(val id: Int, val name: String) {
    override fun toString(): String {
        return "id: $id, name: $name"
    }
}

class Dog(id : Int, name: String, val furColor: Int): Animal(id, name) {
    override fun toString(): String {
        return "id: $id, name: $name, furColor: $furColor"
    }    
}

class Cat(id : Int, name: String, val eyeColor: Int): Animal(id, name) {
    override fun toString(): String {
        return "id: $id, name: $name, eyeColor: $eyeColor"
    }    
}

class Cage<T: Animal>(var animal: T, val size: Double) {
    override fun toString(): String {
        return "animal: $animal ;; size: $size"
    }     
}


fun main() {
    val animal1 = Animal(0, "Animal 1")
    val dog1 = Dog(1, "Dog1", 54454)
    val cat1 = Cat(2, "Cat1", 854587)
    
    
    //val cage1 = Cage<Dog>(animal1, 11.0) //not working
    //val cage1 = Cage<Animal>(dog1, 11.0)	//Works
    val cage1 = Cage<Dog>(dog1, 11.0)
    val cage2 = Cage<Cat>(cat1, 5.0)
    
   // val cage3 = Cage<String>("asd", 55.3)
    
    println(cage1)
    
}
