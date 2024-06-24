<script>
    import AreaChoice from "../lib/AreaChoice.svelte";
import TwoOptionChoice from "../lib/TwoOptionChoice.svelte";

	const stages = {
		landing: 'landing',
		generes: 'generes',
		oldOrNew: 'old-or-new',
		domesticOrForeign: 'domestic-or-foreign',
	}
	const stagesList = [stages.landing, stages.generes, stages.oldOrNew, stages.domesticOrForeign]
	let currentStageIndex = 0;
	$: if(currentStageIndex >= stagesList.length) {
		currentStageIndex = 0;
	}
	$: currentStage = stagesList[currentStageIndex]
	let inputsVisible = false;
	function handleClick(e) {
		currentStageIndex++;
	}
	let generes = ["معمایی","مهیج"];
	let ageRatings = ["+3", "+7", "+12", "+15", "+18"];
	let choosenGenereIndex = 0;
	// $: choosenGenere = generes[choosenGenereIndex];
	let choosenAgeRating;
</script>
<div class="hero">
	{#if currentStage == stages.landing}
		<div class="text">
			<h1 class="title">دنبال فیلم می گردی؟</h1>
			<h2 class="subtitle">برات پیدا می کنیم</h2>
			<button class="btn" on:click={handleClick}>نشونم بده</button>
		</div>
	{/if}
	{#if currentStage == stages.generes}
		<div class="input">
			<div class="input-group">
				<h3 class="label">چه ژانری دوست داری؟</h3>
				<select name="" id="" class="genere" bind:value={choosenGenereIndex}>
					{#each generes as genere, i}
					<option value={i}>{genere}</option>
					{/each}
				</select>
			</div>
			<button on:click={() => currentStageIndex++} class="btn">بعدی</button>
		</div>
		{/if}
		{#if currentStage == stages.oldOrNew}
			<AreaChoice min={1980} max={new Date().getFullYear()}></AreaChoice>
			<!-- <TwoOptionChoice first="قدیمی" second="جدید"></TwoOptionChoice> -->
		{/if}
		<!-- {#if currentStage == stages.domesticOrForeign}
			<h1 class="domestic-or-foreign">طرفدار فیلم خارحی هستی یا ایرانی؟</h1>
			<div class="buttons">
				<button>ایرانی</button>
				<button>خارجی</button>
			</div>
		{/if} -->
</div>
<style>
	@font-face {
		font-family: sahel;
		src: url("$lib/fonts/sahel.woff2");
	}
	:global(*){
		font-family: Sahel;
	}
	.hero {
		display: flex;
		justify-content: center;
		align-items: center;

		height: 100vh;
	}
	.hero .text, .hero .input {
		text-align: center;
	}
	.hero .text .title {
		margin-bottom: 0;
	}
	.hero .text .subtitle {
		margin: 5px 0 20px 0;

		font-size: 28px;
		font-weight: 300;
	}
	.hero .input .input-group {
		margin: 0 0 30px 0;
	}
	.hero .input .genere {
		box-sizing: border-box;

		width: 100%;

		position: relative;
		
		padding: 10px 20px;
		
		border: none;
		border-radius: 50px;

		font-size: 20px;

		-moz-appearance:none; /* Firefox */
		-webkit-appearance:none; /* Safari and Chrome */
		appearance:none;
	}
	.hero .input .genere::after {
		content: ">";
		position: absolute;
		left: 5px;
		top: 50%;
		transform: translate(-50%, -50%);
	}
	.hero .input .label {
		margin: 0 0 5px 0;

		font-size: 28px;
		font-weight: 400;
	}
	.btn {
		color: #fff;
		background-color: hsl(220deg 100% 40%);

		padding: 10px 20px;

		border: none;
		border-radius: 10px;

		font-size: 20px;
	}
	/* .domestic-or-foreign > .buttons {
		display: flex;
		align-items: space-between;
	} */
</style>