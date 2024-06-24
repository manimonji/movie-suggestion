<script>
    import SliderCircle from "./SliderCircle.svelte";
	let container;
	export let min;
	export let max;
	let nums = [min, max];
	$: nums = [clamp(min, nums[0], max), clamp(min, nums[1], max)];
	let reversed = false;
	$: numsSorted = [...nums].sort((a,b) => a - b)
	$: length = max - min;
	function clamp(min, num, max) {
		return num < min ? min : num > max ? max : num
	}
	function getPosition(min, num, length) {
		return (num - min) / length * 100
	}
	function leftToRightX(x, rect) {
		return rect.x + rect.width - x
	}
	function getNewNumber(start ,newPosition, rect) {
		let newNumber = start + Math.round(leftToRightX(newPosition, rect) / rect.width * length);
		return newNumber
	}
	function handleSlide1(e) {
		nums[0] = getNewNumber(min, e.detail.newPosition, container.getBoundingClientRect());
	}
	function handleSlide2(e) {
		nums[1] = getNewNumber(min, e.detail.newPosition, container.getBoundingClientRect());
	}
</script>
<div class="container" bind:this={container}>
	<div class="inputs">
			<div>
				از سال<input min={min} max={max} step="1" type="number" bind:value={nums[0]}>
			</div>
			<div>
				تا سال<input min={min} max={max} step="1" type="number" bind:value={nums[1]}>
			</div>
	</div>
	<div class="slider-container">
		<SliderCircle right={getPosition(min, nums[0], length)} on:slide={handleSlide1}></SliderCircle>
		<div class="choosen" style={`right: ${getPosition(min, numsSorted[0], length)}%; width: ${getPosition(min, numsSorted[1], length) - getPosition(min, numsSorted[0], length)}%;`}></div>
		<SliderCircle right={getPosition(min, nums[1], length)} on:slide={handleSlide2}></SliderCircle>
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