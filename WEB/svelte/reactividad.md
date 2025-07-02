## Reactividad

Con las runas de Svelte tenemos un poderoso sistema de _reactividad_ para mantener el DOM sincronizado con el estado de su aplicación, por ejemplo, en respuesta a un evento.

```ts
<script>
	let count = $state(0);

	function increment() {
		count++;
	}
</script>

<button onclick={increment}>
	Clicked {count}
	{count === 1 ? 'time' : 'times'}
</button>
```

## Reactividad profunda

Como vimos en el ejercicio anterior, el estado reacciona a _las reasignaciones_ . Pero también reacciona a _las mutaciones_ ; a esto lo llamamos _reactividad profunda_ .

```ts

```