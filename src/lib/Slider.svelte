<script>
	export let data;
	let currentItemIndex = 0;
</script>

<section class="slider">
	<button class="previous-item" on:click={() => currentItemIndex--} class:hidden={currentItemIndex == 0}>&lt;</button>
	{#each data as item, i}
		<article class="item" class:hidden={currentItemIndex != i}>
			<slot {item}></slot>
		</article>
	{/each}
	<button class="next-item" on:click={() => currentItemIndex++} class:hidden={currentItemIndex + 1 == data.length}>&gt;</button>
</section>

<style>
	.slider {
		display: flex;
	}
	.item.hidden {
		display: none;
	}
	button.hidden {
		visibility: hidden;
		pointer-events: none;
	}
</style>