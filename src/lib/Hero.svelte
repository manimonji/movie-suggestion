<script>	
	import { createEventDispatcher, onMount } from "svelte";
	import { cubicOut } from "svelte/easing";
    import Machine from "./Machine.svelte";

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

<article class="hero flex py-10 px-14 gap-10 bg-stone-950 items-center">
	{#if ready}
		<section class="text flex-1 py-20" in:slide>
			<h1 class="title text-white text-7xl font-semibold">
				پیشنهاد فیلم با <strong class="bg-none text-blue-400">هوش مصنوعی</strong>
			</h1>
			<p class="text-slate-300 text-xl mt-5">فقط باید به چند تا سوال جواب بدی!</p>
			<button class="py-5 px-6 text-blue-950 rounded-lg text-lg font-semibold bg-blue-50 mt-6" on:click={() => dispatch('ctaClick', {})}>پیشنهاد فیلم</button>
		</section>
	{/if}
	<section class="flex-1 box-border px-28">
		<Machine></Machine>
	</section>
</article>