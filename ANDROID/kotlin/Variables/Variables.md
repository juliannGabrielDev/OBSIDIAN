---
tags:
  - kotlin
  - variables
---
```kotlin
fun main() {
	val name : String = "Julian"

	println(name)
}
```

- `val`: Constante
- `var`: Puede cambiar su valor

## Tipos de datos

- `String`
- `Int` : hasta `2,147,483,647`
- `Long`: hasta `9,223,372,036,854,775,807`
	- `val i1 = 10L`
	- `val i2 : Long = 10`
	- `val i3 = 2147483648`
	- `val i4 = 2_147_483_648`
- `Boolean`
- `Float` : usar sufijo `F`
	- `val pi : Float = 3.14F`
- `Double` : proporciona el doble de espacio que `Float`
- `Short`: hasta `32,767`
- `Byte`: desde `-128` hasta `127`
- `Any`
- `Char`: usar `''`
- `List`
- `Array`