<script lang="ts">
	let numero: number = $state(0);
	let estadoMunicion: string = $derived(
		numero === 0 ? 'Sin municion' : numero === 10 ? 'Municion llena' : 'Cargando municion'
	);
	let bgcolor: string = $derived(
		numero === 0 ? 'bg-red-400' : numero === 10 ? 'bg-green-300' : 'bg-blue-400'
	);
</script>

<main
	class=" flex flex-col min-h-screen justify-center items-center p-4 gap-2 
	bg-amber-200 "
>
	<div
		class="flex items-center justify-center p-8 rounded-3xl
	    text-center max-w-3xl
		border-black border-2 {bgcolor}"
	>
		<button
			onclick={() => numero--}
			disabled={numero <= 0}
			class="h-max w-max p-2 shadow-2xl shadow-black
				max-w-fit bg-blue-300 rounded-xl
				disabled:opacity-45 hover:bg-blue-600"
		> - </button>

		<div class="p-5 font-bold text-2xl">{numero}</div>

		<button
			onclick={() => numero++}
			disabled={numero >= 10}
			class="h-max w-max p-2 rounded-xl
			shadow-2xl shadow-black
				bg-blue-300 disabled:opacity-45
			    hover:bg-blue-600"
			> + </button>
	</div>
	<div class="bg-white rounded-2xl p-3 max-w-fit text-2xl font-bold">
		{estadoMunicion}
	</div>
</main>
