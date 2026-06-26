<script lang="ts">
	let h: number = $state(0);
</script>

<main
	class="flex flex-row bg-white rounded-3xl
    border-4 border-blue-300"
>
	<div class="flex flex-row p-4">
		<div class="p-3">
			<button
				onclick={() => h++}
				class="rounded-3xl border-4
				border-red-300"
			>
				Options
			</button>
		</div>
		<div class="p-3">Home {h}</div>
	</div>

	<div class="flex flex-rows p-4">
		<div class="p-3">barra de busqueda</div>
	</div>

	<div class="flex flex-row p-4">
		<div class="p-3">Notificaciones</div>

		<div class="p-3">Usuario</div>
	</div>
</main>

<style>
</style>
