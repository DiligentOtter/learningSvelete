<script lang="ts">
	import Button from '$lib/components/ui/button/button.svelte';
	import Header from '$lib/components/ui/header/Header.svelte';
	let contador: number = $state(0);
	function incrementar() {
		contador++;
	}
</script>

<main class="flex flex-col items-start justify-center min-h-screen bg-blue-900 p-8 gap-8">
	<Header />
	<h1>Bienvenido a mi playground de Svelte y SvelteKit</h1>
	<div class="flex flex-row">
		<div class="flex flex-col gap-4 max-w-sm w-full p-4">
			<div class="bg-white p-6 rounded-2xl shadow-md text-center border-4 border-blue-300">
				<h1 class="text-2xl font-bold text-gray-800 bg-blue-100 py-2 px-4 rounded-xl mb-4">
					Mi primer componente Svelte
				</h1>
				<p class="text-lg text-gray-600 mb-6">
					Clicks detectados: <span class="font-mono font-bold text-blue-600">{contador}</span>
				</p>
				<button
					onclick={incrementar}
					class="px-6 py-3 bg-blue-600 text-white font-medium rounded-lg hover:bg-blue-700 transition-colors duration-200 active:scale-95"
				>
					Presioname!
				</button>
			</div>

			<div class="bg-white py-4 px-3 rounded-2xl text-center font-bold text-gray-700 shadow-md">
				<h1>Segunda cajita</h1>
			</div>

			<div
				class="bg-amber-200 rounded-2xl h-16 flex items-center justify-center font-bold text-amber-900 shadow-md"
			>
				3ra cajita
				<Button
					variant="outline"
					class="rounded-2xl text-cyan-50 bg-black hover:bg-blue-700 transition-colors duration-200"
					>Soy un boton</Button
				>
			</div>
		</div>

		<div
			class="bg-amber-50 p-6 rounded-2xl
			shadow-lg max-w-xs w-full min-h-50
			flex items-center justify-center
			font-bold text-blue-900 border-4 border-green-300"
		>
			<h1>Hola desde la segunda columna</h1>
		</div>
	</div>
</main>
