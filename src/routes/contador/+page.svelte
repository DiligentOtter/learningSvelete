<script lang="ts">
	let numero: number = $state(0);
	let estadoMunicion: string = $derived(
		numero === 0 ? 'Sin municion' : numero === 10 ? 'Municion llena' : 'Cargando municion'
	);
	let bgcolor: string = $derived(
		numero === 0 ? 'bg-red-400' : numero === 10 ? 'bg-green-300' : 'bg-blue-400'
	);
</script>

<main class="bg-amber-200 min-h-screen
        flex flex-col 
        justify-center items-center p-4">
	<div
		class="flex flex-row
		justify-center p-8 rounded-3xl
	    text-center max-w-3xl
		border-black border-4 {bgcolor}"
	>
		<div class="p-4">
			<button
				onclick={() => numero--}
				disabled={numero <= 0}
				class="p-2 border-b-black border-2
				max-w-fit bg-blue-300 rounded-xl
				disabled:opacity-45 hover:bg-blue-600">-</button
			>
		</div>
		<div class="p-5 font-bold text-2xl font-mono">{numero}</div>
		<div class="p-4">
			<button
				onclick={() => numero++}
				disabled={numero >= 10}
				class="max-w-fit border-2
				border-b-black bg-blue-300
				rounded-xl p-2 disabled:opacity-45
			    hover:bg-blue-600">+</button
			>
		</div>
	</div>
	<div class="bg-white rounded-2xl p-3 max-w-fit text-2xl font-bold">
		{estadoMunicion}
	</div>
</main>
