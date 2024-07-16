<script>
	import charlie from '$lib/images/charlie_chaplin.jpg'
    import { onMount } from 'svelte';
    import Switch from '$lib/Switch.svelte';
	
	function clamp(min, val, max) {
		return Math.max(min, Math.min(val, max))
	}
	
	let scrollY = 0;
	let section;

	let mounted = false;
	onMount(() => mounted = true);
	let animationProgress;
	$: if (mounted) {
		animationProgress = scrollY / section.offsetTop;
		console.log(animationProgress);
	}

	let on = true;
</script>
<svelte:window bind:scrollY></svelte:window>
<section class="flex bg-stone-950 text-gray-50 p-10 items-center" bind:this={section}>
	<section class="flex-1 relative">
		<div class="p-16">
			<img src={charlie} alt="Charlie Chaplin"
			class="rounded-full"
			style:translate={`${clamp(0, 1 - animationProgress, 1) * 100}% 0`}
			style:rotate={`${clamp(0, 1 - animationProgress, 1) * 360}deg`}
			>
		</div>
	</section>
	<section class="flex-1">
		<h2 class="text-7xl">فیلم های قدیمی دوست داری؟</h2>
		<p class="text-slate-300 text-xl mt-5">فیلم های چارلی چاپلین، لورل و هاردی، آلفرد هیچکاک، پدر خوانده، خلاصه هرچی خفنه. این جور فیلم ها رو دوست داری؟</p>
		<div class="flex mt-5">
			<Switch firstLabel="آره" secondLabel="نه" bind:on></Switch>
		</div>
	</section>
</section>