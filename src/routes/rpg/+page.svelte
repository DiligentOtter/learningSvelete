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
		{ texto: 'Tomas un blaster enorme y sales de la habitacion ', siguienteEscenaId: 4 },
		{ texto: 'Disparas a matar', siguienteEscenaId: 5 },
		{
			texto: 'Te escondes sigilosamente esperando a ver como reacciona el bicho',
			siguienteEscenaId: 6
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
			opciones: [coleccionOpciones[3], coleccionOpciones[4]],

			items: ['Blaster']
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
			opciones: [coleccionOpciones[5], coleccionOpciones[6]]
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
</script>
<Starfield />
<div class="relative flex flex-col items-center min-h-screen text-white">
	<header class="m-2">
		<h1 class="text-center font-bold text-2xl">Across the Space - Text RPG (Alpha)</h1>
	</header>
	<main class="flex flex-col flex-1 rounded-2xl m-4 p-2 w-3xl  text-center bg-gray-950">
		<div class="flex flex-col font-mono">
			<h2 class="m-3">{escenaActual.titulo}</h2>
			<p class="m-3">{escenaActual.descripcion}</p>
			{#each escenaActual.opciones as op, index (op)}
				<button onclick={() => (escenaActualID = op.siguienteEscenaId)} class="hover:bg-green-500"
					>[{index}] {op.texto}</button
				>
				{#if op.itemRequerido}
					<p>item requerido: {op.itemRequerido}</p>
				{/if}
			{/each}
			{#if escenaActual.items}
				<p class="m-6">items de la escena: {escenaActual.items}</p>
			{/if}
		</div>
	</main>
</div>
