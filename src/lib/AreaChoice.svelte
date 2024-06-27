<script>
    import { writable } from "svelte/store";
	import SliderCircle from "./SliderCircle.svelte";

	Number.prototype.clamp = function(min, max) {
		  return Math.min(Math.max(this, min), max);
	};
	function createMinMax({ min, max }) {
		let { subscribe, set, update } = writable({ min, max });
		return {
			subscribe,
			setMin: (v) => update((obj) => ({min: v.clamp(min, obj.max), max: obj.max})),
			setMax: (v) => update((obj) => ({min: obj.min, max: v.clamp(obj.min, max)})),
		}
	}
	let container;
	export let min;
	export let max;
	let minMax = createMinMax({ min, max });
	let nums = [min, max];
	$: {
		let oldNums = [...nums];
		let num1 = oldNums[0].clamp(min, oldNums[1]);
		let num2 = oldNums[1].clamp(oldNums[0], max);
		console.log(num1, num2);
		nums = [num1, num2];
	}
	// $: if (changedCircle == 'min' && nums[0] > nums[1]) {
	// 	nums[0] = nums[1];
	// }
	// $: if (changedCircle == 'max' && nums[0] > nums[1]) {
	// 	nums[1] = nums[0];
	// }
	// let reversed = false;
	$: numsSorted = [...nums].sort((a,b) => a - b)
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
	// function handleMinInputChange(e) {
	// 	newNumber = parseInt(e.target.value);
	// 	nums[0] = newNumber > nums[1] ? nums[1] : newNumber;
	// }
	// function handleMaxInputChange(e) {
	// 	newNumber = parseInt(e.target.value);
	// 	nums[1] = newNumber < nums[0] ? nums[0] : newNumber;
	// }
</script>
<div class="container" bind:this={container}>
	<div class="inputs">
			<div>
				از سال<input min={min} max={max} step="1" on:change={(e) => minMax.setMin(parseInt(e.target.value))} bind:value={$minMax.min}>
			</div>
			<div>
				تا سال<input min={min} max={max} step="1" on:change={(e) => minMax.setMax(parseInt(e.target.value))} bind:value={$minMax.max}>
			</div>
	</div>
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
		display: flex;
		flex-direction: column;
		gap: 10px;

		margin: 15px 0;
	}
	.inputs input{
		width: 6ch;

		padding: 0 10px;

		margin-right: 10px;

		border: 1px solid hsl(0deg 0% 70%);
		border-radius: 10px;

		font-size: 32px;
	}
</style>