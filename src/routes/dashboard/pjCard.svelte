<script lang="ts">
	let { src, name, species }: { src: string; name: string; species: string } = $props();
</script>

<div
	class="flex flex-col items-centerp-1.5 rounded-2xl
			border-2 border-[#97ef3a] bg-[#13152b] shadow-md text-center"
>
	<img {src} alt={'image of ' + name} loading="lazy" class="rounded-2xl" />
	<p class="text-[#97ef3a] text-2xl">{name}</p>
	<p class="text-[#00e5ff]">{species}</p>
</div>
