<!--
    La historia de este rpg trata sobre el escape de una Nave imperial
    un grupo de tiranidos invadio la nave (bichos muy feos).
    Y tu eres un simple soldado imperial con algunas cosas a favor y muchas en contra.
 -->
<script lang="ts">
	import Starfield from './starfield.svelte';
	interface Opcion {
		texto: string;
		siguienteEscenaId: number;
		itemRequerido?: string;
	}
	interface Escena {
		id: number;
		titulo: string;
		descripcion: string;
		opciones: Opcion[];
		items?: string[];
	}
	//Vamos a modelar las escenas como si fueran un arbol binario
	const coleccionOpciones: Opcion[] = [
		{ texto: 'Le pegas un porrazo al reloj inteligente del traje', siguienteEscenaId: 1 },
		{ texto: 'Tocas con cuidado el posponer alarma y seguis durmiendo', siguienteEscenaId: 2 },
		{ texto: 'Sales de la habitacion sin preocupaciones', siguienteEscenaId: 3 },
		{ texto: 'Tomas un blaster enorme y sales de la habitacion ', siguienteEscenaId: 3 },
		{ texto: 'Disparas a matar', siguienteEscenaId: 5, itemRequerido: 'Blaster' },
		{
			texto: 'Te escondes sigilosamente esperando a ver como reacciona el bicho',
			siguienteEscenaId: 4
		}
	];
	const coleccionEscenas: Escena[] = [
		{
			id: 0,
			titulo: 'El ataque',
			descripcion:
				'Te encuentras desorientado, confudido. Todas las alarmas de tu traje estan activas.',
			opciones: [coleccionOpciones[0], coleccionOpciones[1]]
		},
		{
			id: 1,
			titulo: 'Evacuar',
			descripcion:
				'Lograste apagar las alarmas. Te enteras que la nave esta en alerta, las comunicaciones no responden. Tu visor muestra un mensaje de alerta, le piden a todos evacuar lo mas rapido posible',
			opciones: [coleccionOpciones[3], coleccionOpciones[2]],

			items: ['Blaster', 'Tarjeta de acceso']
		},
		{
			id: 2,
			titulo: '¿En serio?',
			descripcion:
				'Tras cerrar placidamente los ojos, al cabo de un rato sientes como comienzan a desgarrate las mandibulas de cientos de bichos. Pero oye, al menos dormiste un ratito mas, ¿no?',
			opciones: []
		},
		{
			id: 3,
			titulo: 'La cosa esta caliente',
			descripcion: 'Al salir de la habitacion te encuentras con un bicho',
			opciones: [coleccionOpciones[4], coleccionOpciones[5]]
		},
		{
			id: 4,
			titulo: 'Paciencia, madre de la supervivencia',
			descripcion:
				'Gracias a la gloria del emperador, usas tu cabeza para esconderte y esperar. Al cabo de unos minutos el bicho se larga del lugar',
			opciones: []
		},
		{
			id: 5,
			titulo: 'Al menos lo intentaste',
			descripcion:
				'Disparas con furia incansable el blaster, sin embargo, este se recalienta y cae de tus manos. El monstruo yace muerto en el suelo, agujereado como colador, pero escuchas el ruido de muchos pasos y patas acercandose',
			opciones: []
		}
	];

	let escenaActualID: number = $state(0);
	let escenaActual: Escena = $derived(
		coleccionEscenas.filter((h) => {
			return h.id === escenaActualID;
		})[0]
	);

	let stateGame: { inventario: string[]; espacio: number; itemTomado: boolean } = $state({
		inventario: [],
		espacio: 5,
		itemTomado: false
	});

	function anhadirItem(item: string) {
		stateGame.inventario = [...stateGame.inventario, item];
		stateGame.espacio -= 1;
		stateGame.itemTomado = true;
	}
</script>

<Starfield />
<div class="relative flex flex-col items-center min-h-screen text-white">
	<header class="m-2">
		<h1 class="text-center font-bold text-2xl">Across the Space - Text RPG (Alpha)</h1>
	</header>
	<main class="flex flex-col flex-1 rounded-2xl m-4 p-2 w-3xl text-center font-mono bg-gray-950">
		<div class="flex items-center">
			<h2 class="m-3 text-2xl flex-1">{escenaActual.titulo}</h2>
			{#if stateGame.inventario.length > 0}
				<details class="relative z-10 text-white">
					<summary>
						Inventario ({stateGame.inventario.length})
					</summary>
					<ul class="absolute bg-gray-950">
						{#each stateGame.inventario as item (item)}
							<li>{item}</li>
						{/each}
					</ul>
				</details>
			{/if}
		</div>

		<p class="m-3">{escenaActual.descripcion}</p>

		<div class="flex flex-col mt-9 text-left">
			{#each escenaActual.opciones as op, index (op)}
				<button
					onclick={() => (escenaActualID = op.siguienteEscenaId)}
					class="inline hover:bg-green-500 text-left ml-2 disabled:opacity-50"
					disabled={(op.itemRequerido && !stateGame.inventario.includes(op.itemRequerido)) ? true : false}
				>
					[{index}] {op.texto}
					{#if op.itemRequerido}
						<span class="inline">(item requerido: {op.itemRequerido})</span>
					{/if}
				</button>
				
			{/each}
		</div>

		{#if !stateGame.itemTomado && escenaActual.items}
			<div class="mt-14">
				<p class="m-6">items de la escena: {escenaActual.items}</p>
				<div class="grid grid-cols-3 gap-2">
					{#each escenaActual.items as item (item)}
						<button onclick={() => anhadirItem(item)} class="hover:bg-green-500">
							Añadir item: {item}
						</button>
					{/each}
				</div>
				<span>Espacio de inventario actual: {stateGame.espacio}</span>
			</div>
		{/if}
	</main>
</div>
