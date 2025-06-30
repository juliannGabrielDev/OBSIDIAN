---
tags:
  - kotlin
  - otros
---
```kotlin
data class Student(  
    val fullName : String,  
    var grade : Double,  
    val id : Int  
)  
  
val students = listOf<Student>(  
    Student("Juan", 140.0, 223),  
    Student("Mark", 120.0, 548),  
    Student("Natali", 150.0, 342),  
    Student("Sara", 130.0, 781)  
)  
  
fun getStudentById(id: Int) : Student {  
    return students.find { it.id == id }!!  
}  
  
fun searchInStudent(name : String) : Student? {  
    return students.find { it.fullName.uppercase() == name.uppercase() }  
}  
  
fun main() {  
    println("Please, Enter the ID of the student")  
    val id : Int = readln().toInt()  
    val student = getStudentById(id)  
    println(student)  
    println("Please, Enter the student's name")  
    val name = readln()  
    println(searchInStudent(name) ?: "The student is not found")  
}
```