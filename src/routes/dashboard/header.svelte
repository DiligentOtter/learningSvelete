<script lang="ts">
    let {nroPjs, name=$bindable()} = $props();
</script>

<span class="text-[#97ef3a] text-shadow-[#97ef3a] text-2xl ml-1.5"
	>Rick and Morty / <strong>Diligent Otter</strong></span
>
<input
	type="text"
	placeholder="Busca tu personaje favorito!"
	bind:value={name}
	class="p-1 ml-1.5 mr-1.5 rounded-4xl bg-gray-600 placeholder:text-gray-300 placeholder:opacity-80"
/>
<span class="text-gray-300 ml-1.5">{nroPjs + ' Personajes encontrados'}</span>
