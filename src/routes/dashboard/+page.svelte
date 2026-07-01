<script lang="ts">
	import { fade } from 'svelte/transition';
	import PjCard from './pjCard.svelte';
	import Header from './header.svelte';

	const status: string[] = ['Alive', 'Dead', 'Unknown'];
	const species: string[] = ['Human', 'Alien', 'Robot', 'Mytholog'];
	const gender: string[] = ['Male', 'Female', 'Unknown'];
	const atributes: { key: keyof typeof filtros; label: string; options: string[] }[] = [
		{ key: 'status', label: 'Status', options: status },
		{ key: 'species', label: 'Species', options: species },
		{ key: 'gender', label: 'Gender', options: gender }
	];

	let filtros = $state({ name: '', status: '', species: '', gender: '' });

	let personajesTodos: personaje[] = $state([]);
	let cargando: boolean = $state(true);
	let limite: number = $state(10);

	interface apiResponse {
		info: { count: number; pages: number; next: string; prev: string };
		results: personaje[];
	}
	interface personaje {
		id: number;
		name: string;
		status: string;
		species: string;
		type: string;
		gender: string;
		image: string;
	}

	async function datosCrudosApi(url: string): Promise<apiResponse> {
		const response = await fetch(url);
		if (!response.ok) {
			throw new Error('Error al cargar personajes');
		}
		return (await response.json()) as apiResponse;
	}

	async function cargar() {
		try {
			cargando = true;
			const primera: apiResponse = await datosCrudosApi(
				'https://rickandmortyapi.com/api/character'
			);
			const totalPaginas: number = primera.info.pages;
			const promesas: Promise<apiResponse>[] = [];

			for (let i = 2; i <= totalPaginas; i++) {
				promesas.push(datosCrudosApi(`https://rickandmortyapi.com/api/character?page=${i}`));
			}
			const todosPj = [
				...primera.results,
				...(await Promise.all(promesas)).flatMap((r) => r.results)
			];
			personajesTodos = todosPj;
			cargando = false;
		} catch (error) {
			console.log('Error al cargar personajes:', error);
		}
	}

	$effect(() => {
		cargar();
	});

	limite = 10;
	let personajesLista = $derived(
		personajesTodos.filter((p) => {
			return (
				p.name.toLowerCase().includes(filtros.name.toLowerCase()) &&
				p.status.toLowerCase().includes(filtros.status.toLowerCase()) &&
				p.species.toLowerCase().includes(filtros.species.toLowerCase()) &&
				p.gender.toLowerCase().includes(filtros.gender.toLowerCase())
			);
		})
	);

	$effect(() => {
		void personajesLista;
		limite = 10;
	});
</script>

<svelte:head>
	<link href="https://fonts.googleapis.com/css2?family=Bangers&display=swap" rel="stylesheet" />
</svelte:head>

<main class="flex flex-col h-screen gap-3 font-['Bangers'] bg-[#0b0c1a]">
	<div class="flex flex-col shrink-0 sticky top-0 text-white rounded-2xl">
		<Header nroPjs={personajesLista.length} bind:name={filtros.name} />
	</div>

	<div class="flex flex-1 gap-3 align-items overflow-y-auto">
		<div
			class="flex flex-col text-center max-h-72 items-center rounded-2xl
			border-[#97ef3a] border-2
			text-white gap-2.5"
		>
			<span class="text-[#97ef3a] text-2xl -mb-2">Filtros</span>
			{#each atributes as atribute (atribute.key)}
				<span class="text-gray-500">{atribute.label}</span>
				<select
					name={atribute.key}
					bind:value={filtros[atribute.key]}
					class="rounded-3xl w-28 max-h-8
						ml-1 mr-1 text-center bg-gray-600"
				>
					<option value="">Todos</option>
					{#each atribute.options as option (option)}
						<option value={option}>{option}</option>
					{/each}
				</select>
			{/each}
		</div>

		{#if cargando}
			<p class="text-[#97ef3a] text-center text-2xl col-span-full" transition:fade>
				Abriendo el portal...
			</p>
		{:else}
			<div
				class="flex-1 grid grid-cols-1 md:grid-cols-3 lg:grid-cols-4
                    mb-2 mr-2 gap-1.5 rounded-2xl"
			>
				{#each personajesLista.slice(0, limite) as p (p.id)}
					<PjCard src={p.image} name={p.name} species={p.species} />
				{/each}

				{#if limite < personajesLista.length}
					<button
						onclick={() => (limite = limite + 10)}
						class="col-span-full mt-2 bg-[#97ef3a] text-black
                            rounded-2xl p-2 hover:bg-green-700
                            hover:text-white transition-colors active:scale-95"
					>
						Cargar más
					</button>
				{/if}
			</div>
		{/if}
	</div>
</main>
