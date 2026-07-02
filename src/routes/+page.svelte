<script lang="ts">
	import Button from '$lib/components/ui/button/button.svelte';
	let contador: number = $state(0);
	function incrementar() {
		contador++;
	}
</script>

<div class="min-h-screen w-full bg-blue-900 flex flex-col">
	<header class="flex rounded-2xl bg-violet-200 gap-3">
		<nav
			class="flex gap-4 w-full p-4 rounded-xl
		font-mono"
		>
			<details class="relative">
				<summary
					class="rounded-xl bg-violet-400 hover:bg-violet-300
				font-bold list-none cursor-pointer transition-colors p-1"
				>
					Proyects
				</summary>
				<ul
					class="absolute left-0 mt-2 w-56 rounded-xl p-2 z-50 
					shadow-lg bg-white border border-slate-200 
				  text-slate-900"
				>
					<li><a href="./contador" class="hover:bg-slate-100">Contador</a></li>
					<li><a href="./todo" class="hover:bg-slate-100">Todo</a></li>
					<li><a href="./dashboard" class="hover:bg-slate-100">Dashboard</a></li>
					<li><a href="./rpg" class="hover:bg-slate-100">RPG-Text</a></li>
				</ul>
			</details>
			<span>About</span>
			<span>My Proyects</span>
		</nav>
	</header>
	<main
		class="flex flex-col flex-1 justify-start
    justify-center min-h-screen
    bg-blue-900 p-4 gap-8"
	>
		<h1
			class="text-center text-5xl font-extrabold
		tracking-tight text-zinc-900 rounded-2xl p-4 bg-amber-50"
		>
			Bienvenido a mi playground de Svelte y SvelteKit
		</h1>
		<div class="flex justify-center gap-2">
			<section class="flex flex-col gap-4 max-w-sm w-full p-4">
				<article
					class="bg-white p-6 rounded-2xl shadow-md
			text-center border-4 border-blue-300"
				>
					<h2 class="text-2xl font-bold text-gray-800 bg-blue-100 py-2 px-4 rounded-xl mb-4">
						Mi primer componente Svelte
					</h2>
					<p class="text-lg text-gray-600 mb-6">
						Clicks detectados: <span class="font-mono font-bold text-blue-600">{contador}</span>
					</p>
					<button
						onclick={incrementar}
						class="px-6 py-3 bg-blue-600 text-white font-medium rounded-lg
					hover:bg-blue-700 transition-colors duration-200 active:scale-95"
					>
						Presioname!
					</button>
				</article>

				<div
					class="bg-white py-4 px-3 rounded-2xl
			text-center font-bold text-gray-700 shadow-md"
				>
					<h2>Segunda cajita</h2>
				</div>

				<div
					class="bg-amber-200 rounded-2xl h-16 flex
				items-center justify-center font-bold text-amber-900 shadow-md"
				>
					3ra cajita
					<Button
						variant="outline"
						class="rounded-2xl text-cyan-50
					bg-black hover:bg-blue-700 transition-colors duration-200">Soy un boton</Button
					>
				</div>
			</section>

			<div
				class="flex flex-col gap-4 bg-amber-50 p-6 rounded-2xl
			shadow-lg max-w-xs w-full min-h-50
			 justify-start text-blue-900
			border-4 border-green-300"
			>
				<h2 class="font-bold text-center">Hola! ahora desde la segunda columna</h2>

				<p>
					En este sitio aprendo sobre svelte, js/ts, css, layout y todo lo relacionado con
					desarrollo web moderno. Asi que espera cualquier locura y malgusto.
				</p>
			</div>
		</div>
	</main>
</div>
