<!-- starfield.svelte -->
<script lang="ts">
	import { onMount } from 'svelte';

	let canvas: HTMLCanvasElement;
	let animationId: number;

	interface Star {
		x: number;
		y: number;
		size: number;
		opacity: number;
		speed: number;
	}

	function crearEstrellas(w: number, h: number, cantidad: number): Star[] {
		return Array.from({ length: cantidad }, () => ({
			x: Math.random() * w,
			y: Math.random() * h,
			size: Math.random() * 2 + 0.3,
			opacity: Math.random() * 0.7 + 0.3,
			speed: Math.random() * 0.8 + 0.1
		}));
	}

	function dibujar(ctx: CanvasRenderingContext2D, estrellas: Star[], w: number, h: number) {
		ctx.clearRect(0, 0, w, h);

		for (const s of estrellas) {
			ctx.globalAlpha = s.opacity;
			ctx.fillStyle = '#fff';
			ctx.beginPath();
			ctx.arc(s.x, s.y, s.size, 0, Math.PI * 2);
			ctx.fill();

			s.y += s.speed;
			if (s.y > h) {
				s.y = 0;
				s.x = Math.random() * w;
			}
		}
	}

	function loop(ctx: CanvasRenderingContext2D, estrellas: Star[], w: number, h: number) {
		dibujar(ctx, estrellas, w, h);
		animationId = requestAnimationFrame(() => loop(ctx, estrellas, w, h));
	}

	onMount(() => {
		const ctx = canvas.getContext('2d')!;
		const w = (canvas.width = window.innerWidth);
		const h = (canvas.height = window.innerHeight);
		const estrellas = crearEstrellas(w, h, 400);

		loop(ctx, estrellas, w, h);

		const onResize = () => {
			canvas.width = window.innerWidth;
			canvas.height = window.innerHeight;
		};
		window.addEventListener('resize', onResize);

		return () => {
			cancelAnimationFrame(animationId);
			window.removeEventListener('resize', onResize);
		};
	});
</script>

<canvas
	bind:this={canvas}
	class="fixed inset-0 pointer-events-none bg-black"
	style="width: 100vw; height: 100vh;"
></canvas>

<style>
	canvas {
		display: block;
	}
</style>
