<script>
    import { writable } from "svelte/store";
	import SliderCircle from "./SliderCircle.svelte";
    import { createEventDispatcher } from "svelte";

	Number.prototype.clamp = function(min, max) {
		  return Math.min(Math.max(this, min), max);
	};
	function createMinMax({ min, max }) {
		let { subscribe, set, update } = writable({ min, max });
		return {
			subscribe,
			setMin: (v) => update((obj) => isNaN(v) ? {min: min, max: obj.max} : ({min: v.clamp(min, obj.max), max: obj.max})),
			setMax: (v) => update((obj) => isNaN(v) ? {min: obj.min, max: max} : ({min: obj.min, max: v.clamp(obj.min, max)})),
		}
	}
	let container;
	export let unit;
	export let min;
	export let max;
	export let mode;
	let minMax = createMinMax({ min, max });
	$: length = max - min;
	function getPosition(min, num, length) {
		return (num - min) / length * 100
	}
	function leftToRightX(x, rect) {
		return rect.x + rect.width - x
	}
	function getNewNumber(start ,newPosition, rect) {
		return start + Math.round(leftToRightX(newPosition, rect) / rect.width * length)
	}
	function handleSlide1(e) {
		let newNumber = getNewNumber(min, e.detail.newPosition, container.getBoundingClientRect());
		minMax.setMin(newNumber);
	}
	function handleSlide2(e) {
		let newNumber = getNewNumber(min, e.detail.newPosition, container.getBoundingClientRect());
		minMax.setMax(newNumber);
	}
	const dispatch = createEventDispatcher();
	$: dispatch('change', $minMax) 
</script>
<div class="container" bind:this={container}>
	{#if mode == "labled"}
		<div class="inputs labled">
				<div>
					از {unit}<input min={min} max={max} step="1" on:change={(e) => minMax.setMin(parseInt(e.target.value))} value={$minMax.min}>
				</div>
				<div>
					تا {unit}<input min={min} max={max} step="1" on:change={(e) => minMax.setMax(parseInt(e.target.value))} value={$minMax.max}>
				</div>
		</div>
	{:else if mode == "linear"}
		<div class="inputs linear">
			از <input min={min} max={max} step="1" on:change={(e) => minMax.setMin(parseInt(e.target.value))} value={$minMax.min}>
			تا <input min={min} max={max} step="1" on:change={(e) => minMax.setMax(parseInt(e.target.value))} value={$minMax.max}>
			{unit}
		</div>
	{/if}
	<div class="slider-container">
		<SliderCircle right={getPosition(min, $minMax.min, length)} on:slide={handleSlide1}></SliderCircle>
		<div class="choosen" style={`right: ${getPosition(min, $minMax.min, length)}%; width: ${getPosition(min, $minMax.max, length) - getPosition(min, $minMax.min, length)}%;`}></div>
		<SliderCircle right={getPosition(min, $minMax.max, length)} on:slide={handleSlide2}></SliderCircle>
	</div>
</div>
<style>
	.slider-container {
		--bg-bar-height: 7.5px; 

		height: var(--bg-bar-height);
		width: 100%;
		
		position: relative;

		background-color: hsl(0deg 0% 90%);

		border-radius: calc(var(--bg-bar-height) / 2);
	}
	.slider-container > .choosen {
		height: var(--bg-bar-height);

		position: absolute;
		top: 0;

		background-color: blue;
	}
	.inputs {
		margin-bottom: 15px;
	}
	.inputs.labled {
		display: grid;
		gap: 15px;

		grid-template-columns: 1fr 1fr;

	}
	.inputs.linear {
		display: flex;
		align-items: center;
		gap: 8px;

		font-size: 20px;
	}
	.inputs input{
		box-sizing: border-box;

		padding: 0 10px;

		width: 100%;

		border: 1px solid hsl(0deg 0% 70%);
		border-radius: 10px;

		font-size: 32px;
	}
</style>