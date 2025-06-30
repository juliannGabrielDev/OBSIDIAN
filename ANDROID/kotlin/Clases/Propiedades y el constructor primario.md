---
tags:
  - kotlin
  - clases
---
## Modelar el mundo real

En programación, los programadores intentan a menudo modelar el mundo real. Para ello, se utilizan clases con propiedades que expresan lo que es importante en un sistema.

Supongamos que implementa un sistema para gestionar una escuela.

```kotlin
class Student

class Teacher

class Subject
```

## Especificar propiedades

En un sistema no necesita saberlo todo sobre cada persona, sólo que es relevante para el sistema.

```kotlin
class Subject(
	val name : String,
	val teacher : Teacher,
	val isObligatory : Boolean
)

class Teacher(
	val name : String,
	val surname : String,
	val birthday : String,
	val status : String
)

fun main() {
	val julianGabriel = Teacher(name = "Julian", surname = "Gabriel", birthday = "20.01.2005", status = "ACTIVE")
	val biology1 = Subject(name = "Biology 1", teacher = julianGabriel, isobligatory = true)
}
```

>[!IMPORTANT]
>Cada propiedad adicional en una clase es un coste, por eso debe intentar límitar el número de propiedades de sus clases al mínimo requerido por su sitema.

## Propiedad `id`

En los sitemas de la vida real, las clases suelen tener una propiedad llamada `id`.

```kotlin
class Student(
	val id : String,
	val name : String,
	val surname : String,
)
```

Es útil suponiendo que tiene dos usuarios con el mismo nombre y apellido, y que uno de ellos ha sacado una mala nota. Si asocia esta nota al nombre y apellido del alumno, podría asociarse a la persona incorrecta.

```kotlin
class Grade(
	var points : Int,
	val studentId : String,
	val topicId : String,
)
```

