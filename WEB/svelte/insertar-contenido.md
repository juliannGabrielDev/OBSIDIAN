## Insertar Contenido

A veces necesitas renderizar HTML directamente en un componente.

En Svelte, esto se hace con la `{@html ...}`etiqueta especial:

```ts
<script>
	let string = `this string contains some <strong>HTML!!!</strong>`;
</script>

<p>{@html string}</p>
```

>[!WARNING] Advertencia
>Svelte no realiza ninguna limpieza de la expresión interna `{@html ...}` antes de insertarla en el DOM.

