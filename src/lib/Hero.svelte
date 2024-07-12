<script>
    import { createEventDispatcher, onMount } from "svelte";
	import { cubicOut } from "svelte/easing";

	const dispatch = createEventDispatcher();

	function slide(node, { delay = 0, duration = 400 }) {
		return {
			delay,
			duration,
			css: (t) => `translate: ${1000 - (t * 1000)}px`,
			easing: cubicOut
		};
	}
	let ready = false;
	onMount(() => ready = true);
</script>

<article class="hero flex py-10 px-14 gap-10 bg-stone-950 min-h-screen items-center">
	{#if ready}
		<section class="text flex-1 py-20" in:slide>
			<h1 class="title text-white text-8xl font-semibold" >
				پیشنهاد فیلم با <strong class="bg-none text-yellow-300">هوش مصنوعی</strong>
			</h1>
			<button class="py-5 px-6 text-yellow-200 rounded-lg text-lg font-semibold bg-yellow-950 mt-8" on:click={() => dispatch('ctaClick', {})}>نشونم بده</button>
		</section>
	{/if}
	<section class="flex-1">
		img
	</section>
</article>