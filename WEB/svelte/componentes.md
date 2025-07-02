# Componentes

En Svelte, una aplicación se compone de uno o más _componentes_ . Un componente es un bloque de código reutilizable e independiente que encapsula HTML, CSS y JavaScript, todos ellos relacionados entre sí, y se escriben en un `.svelte`archivo.

## Añadiendo datos

Un componente que solo renderiza marcado estático no es muy interesante. Añadamos algunos datos.

```ts
<script lang="ts">
	let name = 'Svelte';
</script>

<h1>Hello {name.toUpperCase()}!</h1>

<style>
	/* Write your CSS here */
</style>
```


## Atributos de taquigrafía

No es raro tener un atributo cuyo nombre y valor coincidan, como `src={src}`. Svelte nos ofrece una abreviatura práctica para estos casos:

```ts
<script>
	let src = '/tutorial/image.gif';
	let name = 'Rick Astley';
</script>

<img {src} alt="A {name} dances." />
```
