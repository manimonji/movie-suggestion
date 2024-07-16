<script>
    import { createEventDispatcher, onMount } from "svelte";

	export let right =  0;

	let firstX;

	let isDragging = false;

	function handleMouseDown(e) {
		isDragging = true;
		firstX = e.clientX;
		console.log(firstX);
	}

	const dispatch = createEventDispatcher();
	function handleMouseMove(e) {
		if (isDragging) {
			dispatch('slide', {
				newPosition: e.clientX
			});
		}
	}
	function handleMouseUp() {
		isDragging = false;
	}
	onMount(() => {
		document.body.addEventListener('mouseup', handleMouseUp);
		document.body.addEventListener('mousemove', handleMouseMove)
	});
</script>
<button class="circle" style={`right: ${right}%`} on:mousedown={handleMouseDown} class:dragged={isDragging}></button>
<style>
	.circle {
		display: block;

		--slider-circle-radius: 15px;
		width: var(--slider-circle-radius);
		height: var(--slider-circle-radius);

		padding: 0;

		position: absolute;
		top: 0;

		background-color: #fff;

		border: 3px solid #000;
		border-radius: 100%;

		transform: translate(50%, -25%);

		z-index: 2;
	}
	.circle.dragged {
		z-index: 3;
	}
</style>