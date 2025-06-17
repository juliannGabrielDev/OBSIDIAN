---
aliases:
  - Clases 🚀
tags:
  - js
breadcrumb:
  - "[[indice-web|WEB 🔗📝]]"
  - "[[indice-javascript|⚡ Índice JavaScript]]"
Fecha: 2025-06-05
---
# Clases 🚀
Pensemos en ellas como si fueran **moldes** o **planos** para crear objetos. Antes se usaban funciones constructoras y prototipos, pero desde ES6, tenemos esta sintaxis que es mucho más clara y se parece más a otros lenguajes. <mark style="background: #FFF3A3A6;">En el fondo, sigue siendo lo mismo (prototipos), pero es más fácil de escribir y entender.</mark>

---
## 1. Definiendo una Clase 🧱
Para crear una clase, usamos la palabra clave **`class`** seguida del nombre que le queramos dar (por convención, empieza con mayúscula).
- Dentro de la clase, lo primero y más importante es el método **`constructor()`**.
- Este método es especial y se ejecuta **automáticamente** cada vez que creamos un nuevo objeto a partir de la clase.
- <mark style="background: #BBFABBA6;">Su trabajo principal es inicializar las propiedades del objeto.</mark>
```js
class Videojuego {
  constructor(titulo, genero, anio) {
    // 'this' se refiere al objeto que se creará
    this.titulo = titulo;
    this.genero = genero;
    this.anio = anio;
  }
}
```

---

## 2. Creando Instancias (Objetos) 🤖
Una vez que tenemos nuestro "molde" (la clase), podemos crear objetos reales. A esto se le llama **instanciar**. Para hacerlo, usamos la palabra clave **`new`**.
- Al usar **`new`**, JavaScript crea un objeto vacío `{}`.
- Luego, llama al **`constructor()`** de la clase, y `this` dentro del constructor apuntará a ese nuevo objeto.
- <mark style="background: #ADCCFFA6;">Le pasamos los argumentos que el constructor necesite.</mark>
```js
// Creamos dos instancias de la clase Videojuego
const juego1 = new Videojuego("The Legend of Zelda: Ocarina of Time", "Aventura", 1998);
const juego2 = new Videojuego("Stardew Valley", "Simulación", 2016);

console.log(juego1.titulo); // Imprime: "The Legend of Zelda: Ocarina of Time"
console.log(juego2.genero); // Imprime: "Simulación"
```

---
## 3. Métodos en las Clases 🔧

Los métodos no son más que **funciones** que pertenecen a la clase. Sirven para definir el comportamiento que tendrán los objetos creados a partir de ella.

- Se definen directamente dentro del cuerpo de la clase, después del constructor.
- <mark style="background: #FFB86CA6;">Pueden acceder a las propiedades del objeto usando `this`.</mark>

```js
class Videojuego {
  constructor(titulo, genero, anio) {
    this.titulo = titulo;
    this.genero = genero;
    this.anio = anio;
  }

  // Este es un método de la clase
  mostrarInfo() {
    return `${this.titulo}, un juego de ${this.genero} lanzado en ${this.anio}.`;
  }

  // Este es un método estático
  static creador() {
    return "Las clases son syntactic sugar sobre los prototipos.";
  }
}

const juego1 = new Videojuego("The Legend of Zelda: Ocarina of Time", "Aventura", 1998);

// Llamamos al método desde la instancia
console.log(juego1.mostrarInfo());

// Los métodos estáticos se llaman desde la clase, no la instancia
console.log(Videojuego.creador());
```

| **Tipo de Método**      | **Descripción**                                                       | **Ejemplo de llamada** |
| ----------------------- | --------------------------------------------------------------------- | ---------------------- |
| **De Instancia**        | Opera sobre una instancia específica de la clase. Usa `this`.         | `juego1.mostrarInfo()` |
| **Estático (`static`)** | Pertenece a la clase en sí, no a una instancia. No puede usar `this`. | `Videojuego.creador()` |

---

## 4. Herencia 🧬
La herencia nos permite crear una **clase hija** que hereda propiedades y métodos de una **clase padre**. ¡Es súper útil para no repetir código!
- Usamos la palabra clave **`extends`** para indicar de quién se hereda.
- Dentro del constructor de la clase hija, es **obligatorio** llamar a **`super()`**.
- <mark style="background: #D2B3FFA6;">`super()` llama al constructor de la clase padre</mark> y nos asegura que el objeto se inicialice correctamente.
```js
// Clase Padre
class Personaje {
  constructor(nombre, vida) {
    this.nombre = nombre;
    this.vida = vida;
  }

  atacar() {
    return `${this.nombre} realiza un ataque básico.`;
  }
}

// Clase Hija que hereda de Personaje
class Mago extends Personaje {
  constructor(nombre, vida, mana) {
    // 1. Llama al constructor del padre
    super(nombre, vida);
    // 2. Añade nuevas propiedades
    this.mana = mana;
  }

  // Nuevo método específico de Mago
  lanzarHechizo() {
    this.mana -= 10;
    return `${this.nombre} lanza una bola de fuego. Quedan ${this.mana} de maná.`;
  }

  // También podemos sobreescribir un método del padre
  atacar() {
    return `${this.nombre} ataca con su báculo mágico.`
  }
}

const gandalf = new Mago("Gandalf", 100, 150);
console.log(gandalf.nombre);        // "Gandalf" (heredado)
console.log(gandalf.atacar());      // "Gandalf ataca con su báculo mágico." (sobreescrito)
console.log(gandalf.lanzarHechizo()); // "Gandalf lanza una bola de fuego..." (propio de la hija)
```

---
## 5. Getters y Setters ✨
Son métodos especiales que nos permiten "obtener" (**get**) y "establecer" (**set**) el valor de una propiedad. Nos dan más control sobre cómo se accede o modifica una propiedad.
- **`get`**: Se usa para leer el valor de una propiedad. Se define como una función pero se llama como una propiedad.
- **`set`**: Se usa para modificar el valor de una propiedad. Recibe un argumento con el nuevo valor.
```js
class Usuario {
  constructor(nombre, apellido) {
    this.nombre = nombre;
    this.apellido = apellido;
  }

  // GETTER: une nombre y apellido
  get nombreCompleto() {
    return `${this.nombre} ${this.apellido}`;
  }

  // SETTER: modifica nombre y apellido a partir de un string
  set nombreCompleto(nuevoNombre) {
    const [nombre, apellido] = nuevoNombre.split(' ');
    this.nombre = nombre;
    this.apellido = apellido;
  }
}

const user = new Usuario("John", "Doe");

// Usamos el getter como si fuera una propiedad
console.log(user.nombreCompleto); // Imprime: "John Doe"

// Usamos el setter para modificar los datos
user.nombreCompleto = "Jane Smith";

console.log(user.nombre);         // Imprime: "Jane"
console.log(user.nombreCompleto); // Imprime: "Jane Smith"
```