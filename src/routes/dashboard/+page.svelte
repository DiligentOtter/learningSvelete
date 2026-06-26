<script lang="ts">
	import { fade } from 'svelte/transition';

	const status: string[] = ['Alive', 'Dead', 'Unknown'];
	const species: string[] = ['Human', 'Alien', 'Robot', 'Mytholog'];
	const gender: string[] = ['Male', 'Female', 'Unknown'];
	let filtros = $state({ name: '', status: '', species: '', gender: '' });

	let personajesTodos: personaje[] = $state([]);
	let personajesLista: personaje[] = $state([]);
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
			personajesLista = todosPj;
			cargando = false;
		} catch (error) {
			console.log('Error al cargar personajes:', error);
		}
	}

	$effect(() => {
		cargar();
	});

	function filtrarPersonajes() {
		limite = 10;
		personajesLista = personajesTodos.filter((p) => {
			return (
				p.name.toLowerCase().includes(filtros.name.toLowerCase()) &&
				p.status.toLowerCase().includes(filtros.status.toLowerCase()) &&
				p.species.toLowerCase().includes(filtros.species.toLowerCase()) &&
				p.gender.toLowerCase().includes(filtros.gender.toLowerCase())
			);
		});
	}
</script>

<svelte:head>
	<link href="https://fonts.googleapis.com/css2?family=Bangers&display=swap" rel="stylesheet" />
</svelte:head>

<main class="flex flex-col h-screen gap-3 font-['Bangers'] bg-[#0b0c1a]">
	<div class="flex flex-col shrink-0 sticky top-0 text-white rounded-2xl">
		<span class="text-[#97ef3a] text-shadow-[#97ef3a] text-2xl ml-1.5"
			>Rick and Morty / <strong>Diligent Otter</strong></span
		>
		<input
			type="text"
			placeholder="Busca tu personaje favorito!"
			bind:value={filtros.name}
			oninput={filtrarPersonajes}
			class="p-1 ml-1.5 mr-1.5 rounded-4xl bg-gray-600 placeholder:text-gray-300 placeholder:opacity-80"
		/>
		<span class="text-gray-300 ml-1.5">{personajesLista.length + ' Personajes encontrados'}</span>
	</div>

	<div class="flex flex-1 gap-3 align-items overflow-y-auto">
		<div
			class="flex flex-col text-center max-h-72
			items-center rounded-2xl
			border-[#97ef3a] border-2
			text-white gap-2.5"
		>
			<span class="text-[#97ef3a] text-2xl -mb-2">Filtros</span>
			<span class="text-gray-500">Status</span>
			<select
				name="Status"
				bind:value={filtros.status}
				onchange={filtrarPersonajes}
				class="rounded-3xl w-28 max-h-8
				ml-1 mr-1 text-center bg-gray-600"
			>
				<option value="">Todos</option>
				{#each status as s (s)}
					<option value={s}>{s}</option>
				{/each}
			</select>

			<span class="text-gray-500">species</span>
			<select
				name="species"
				bind:value={filtros.species}
				onchange={filtrarPersonajes}
				class="rounded-3xl w-28 max-h-8
				ml-1 mr-1 text-center bg-gray-600"
			>
				<option value="">Todos</option>
				{#each species as t (t)}
					<option value={t}>{t}</option>
				{/each}
			</select>
			<span class="text-gray-500">Gender</span>
			<select
				name="Gender"
				bind:value={filtros.gender}
				onchange={filtrarPersonajes}
				class="rounded-3xl w-28 max-h-8
				ml-1 mr-1 text-center bg-gray-600"
			>
				<option value="">Todos</option>
				{#each gender as g (g)}
					<option value={g}>{g}</option>
				{/each}
			</select>
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
					<div
						class="flex flex-col items-center
				p-1.5 rounded-2xl
				border-2 border-[#97ef3a] bg-[#13152b]
				shadow-md"
					>
						<img src={p.image} alt={'image of ' + p.name} loading="lazy" class="rounded-2xl" />
						<p class="text-[#97ef3a] text-2xl">{p.name}</p>
						<p class="text-[#00e5ff]">{p.species}</p>
					</div>
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
