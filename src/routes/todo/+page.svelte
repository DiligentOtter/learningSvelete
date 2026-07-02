<script lang="ts">
	import { fly } from 'svelte/transition';
	import { bounceOut } from 'svelte/easing';

	let listaTasks: string[] = $state([]);
	let cantTareas: number = $derived(listaTasks.length);
	let tarea: string = $state('');

	let msg: string = $derived(
		cantTareas < 0
			? 'Ingresa una tarea'
			: cantTareas === 0
				? 'Con que comenzamos hoy?'
				: cantTareas > 5
					? 'Estas atareado hoy, eh'
					: 'Vamos con todo!'
	);
	function deleteTask(index: number) {
		listaTasks = listaTasks.filter((_, i) => i !== index);
		cantTareas--;
	}
	function nuevaTarea(newTask: string) {
		listaTasks = [...listaTasks, newTask];
		tarea = '';
		cantTareas++;
	}
</script>

<main class="flex flex-col items-center min-h-screen bg-blue-300">
	<div
		class="flex flex-row w-full max-w-4xl m-2
		items-center justify-center
		min-h-25 p-2 rounded-2xl bg-white"
	>
		<div class="w-22 hidden sm:block"></div>
		<div class="flex-1 text-center">
			<input
				type="text"
				placeholder={msg}
				bind:value={tarea}
				onkeydown={(e) => {
					if (e.key === 'Enter') nuevaTarea(tarea);
				}}
				class="rounded-2xl w-full"
			/>
		</div>
		<div
			class="flex justify-center items-center
				    ml-auto w-22 max-h-8"
		>
			<button
				onclick={() => (tarea === "" ? (cantTareas = -1) : nuevaTarea(tarea))}
				class="flex rounded-full h-8 w-8 pb-0.5
				justify-center text-center shadow-2xl shadow-black
				font-bold text-xl
				 bg-amber-500 hover:bg-amber-700"
			>
				+
			</button>
		</div>
	</div>

	<div class="flex flex-col gap-2 mt-4 w-full max-w-4xl">
		{#each listaTasks as t, i (t)}
			<div
				transition:fly={{ y: -50, duration: 500, easing: bounceOut }}
				class="p-4 bg-white rounded-xl shadow-sm flex items-center gap-3"
			>
				<input type="checkbox" class="rounded-full" onclick={() => deleteTask(i)} />
				<span class="text-gray-700">{t}</span>
			</div>
		{/each}
	</div>
</main>
