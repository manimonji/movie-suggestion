<script>
    import AreaChoice from "$lib/AreaChoice.svelte";
	import TwoOptionChoice from "$lib/TwoOptionChoice.svelte";

	import oldVideo from "$lib/videos/charlie.mp4"
	import oldTitle from "$lib/images/old-title.svg"

	const stages = {
		landing: 'landing',
		inputs: 'inputs',
		favoriteMovies: 'favoriteMovies'
	}
	const stagesList = [stages.landing, stages.inputs]
	let currentStageIndex = 0;
	$: if(currentStageIndex >= stagesList.length) {
		currentStageIndex = 0;
	}
	$: currentStage = stagesList[currentStageIndex]
	// let inputsVisible = false;
	function handleClick(e) {
		currentStageIndex++;
	}
	let likesOldMovies = '';
	// let generes = ["معمایی","مهیج"];
	// let ageRatings = ["+3", "+7", "+12", "+15", "+18"];
	// let choosenGenereIndex = 0;
	// // $: choosenGenere = generes[choosenGenereIndex];
	// let choosenAgeRating;
</script>
{#if currentStage == stages.landing}
	<div class="hero">
		<div class="text">
			<h1 class="title">دنبال فیلم می گردی؟</h1>
			<h2 class="subtitle">برات پیدا می کنیم</h2>
			<button class="btn" on:click={handleClick}>نشونم بده</button>
		</div>
	</div>
{:else if currentStage == stages.inputs}
	<div class="inputs">
		<div class="likes-old-movies">
			<img class="image-title" src={oldTitle} alt="Do you like old movies?">
			<!-- <h1 class="title">قدیمی پسندی؟</h1> -->
			<video class="video" src={oldVideo} autoplay muted loop>
				<track kind="captions">
			</video>
			<div class="buttons">
				<button on:click={() => likesOldMovies = true} class:active={likesOldMovies}>آره</button>
				<button on:click={() => likesOldMovies = false} class:active={!likesOldMovies}>نه</button>
			</div>
		</div>
		<section class="date-and-length">
			<div class="date">
				<h2 class="title">تاریخ انتشار</h2>
				<AreaChoice min={1980} max={new Date().getFullYear()} unit="سال" mode="labled"></AreaChoice>
			</div>
			<div class="movie-length">
				<h2 class="title">طول فیلم</h2>
				<AreaChoice min={20} max={80} unit="دقیقه" mode="linear"></AreaChoice>
			</div>
			<button class="btn next">بعدی</button>
		</section>
		<section class="favourite-director">
			<h1 class="title" >کارگردان مورد علاقه ات کیه؟</h1>
			<input type="text" class="search-box" placeholder="کریستوفر نولان">
			<section class="results">
				<p class="hint">نتایج میان اینجا</p>
			</section>
		</section>
	</div>
	<!-- <div class="input">
		<div class="input-group">
			<h3 class="label">چه ژانری دوست داری؟</h3>
			<select name="" id="" class="genere" bind:value={choosenGenereIndex}>
				{#each generes as genere, i}
				<option value={i}>{genere}</option>
				{/each}
			</select>
		</div>
		<button on:click={() => currentStageIndex++} class="btn">بعدی</button>
	</div> -->
	<!-- {/if}
	{#if currentStage == stages.oldOrNew} -->
		<!-- <TwoOptionChoice first="قدیمی" second="جدید"></TwoOptionChoice> -->
{/if}
	<!-- {#if currentStage == stages.domesticOrForeign}
		<h1 class="domestic-or-foreign">طرفدار فیلم خارحی هستی یا ایرانی؟</h1>
		<div class="buttons">
			<button>ایرانی</button>
			<button>خارجی</button>
		</div>
	{/if} -->
<style>
	@font-face {
		font-family: sahel;
		src: url("$lib/fonts/sahel.woff2");
	}
	:global(body) {
		margin: 0;
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
	.hero .text {
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
	.inputs {
		box-sizing: border-box;
		display: grid;
		grid-template-columns: 1fr 1fr 1fr;
		/* gap: 30px; */

		height: 100vh;

		/* padding: 30px; */
	}
	.inputs .likes-old-movies {
		box-sizing: border-box;
		padding: 28px;

		position: relative;

		grid-row: 1 / 3;

		overflow: clip;
		
		background: linear-gradient(0deg, #000, transparent);
	}
	.inputs .likes-old-movies .image-title {
		width: 100%;

		position: relative;

		z-index: 1;
	}
	.inputs .likes-old-movies .buttons {
		width: 100%;
		display: flex;
		gap: 25px;
	}
	.inputs .likes-old-movies .buttons button {
		flex-grow: 1;

		padding: 25px 0;

		color: #fff;
		background-color: transparent;

		border: solid 3px currentColor;

		font-size: 28px;
		font-weight: 500;

		transition: background-color 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94);
	}
	.inputs .likes-old-movies .buttons button:hover {
		background-color: hsla(0deg, 100%, 100%, 0.25);
	}
	.inputs .likes-old-movies .buttons button.active {
		color: #1c0302;
		background-color: #d48817;
	}
	.inputs .likes-old-movies .title {
		margin: 0;

		line-height: 1.1;
		font-size: 84px;

		padding-bottom: 0.1em;

		position: relative;

		color: #fff;
		
		-moz-mask-image: url("$lib/images/type-dark.png");
		-webkit-mask-image: url("$lib/images/type-dark.png");
		mask-image: url("$lib/images/type-dark.png");
	}
	.inputs .likes-old-movies .video {
		position: absolute;
		top: calc(50% - 1px);
		left: 50%;
		height: 100%;

		transform: translate(-50%, -50%);

		z-index: -1;
	}
	.inputs .favourite-director {
		box-sizing: border-box;
		grid-row: 1 / 3;
		display: flex;
		flex-direction: column;

		padding: 32px;

		background-color: #000;
	}
	.inputs .favourite-director .title {
		color: #fff;
		
		margin: 0;

		font-size: 64px;
		line-height: 1.2;
	}
	.inputs .favourite-director .search-box {
		box-sizing: border-box;

		width: 100%;

		padding: 16px;
		margin-top: 32px;

		color: #fff;
		background-color: hsl(0deg 0% 20%);
		
		border: 1px solid hsla(0deg, 100%, 100%, 0.25);
		border-radius: 15px;

		font-size: 20px;

		z-index: 1;
	}
	.inputs .favourite-director .search-box:focus {
		border-color: hsl(220deg 100% 60%);
		outline: none;
	}
	.inputs .favourite-director .results {
		flex-grow: 1;

		padding-top: 15px;

		position: relative;
		top: -15px;

		background-color: hsl(0deg 0% 5%);

		border-radius: 0 0 15px 15px;
	}
	.inputs .favourite-director .results .hint {
		position: absolute;
		top: 50%;
		left: 50%;

		color: #fff;

		transform: translate(-50%, -50%);
	}
	.inputs .date-and-length {
		box-sizing: border-box;
		grid-row: 1 / 3;
		display: flex;
		flex-direction: column;
		gap: 48px;

		padding: 64px;
	}
	.inputs .date-and-length .title{
		margin: 0 0 8px 0;
	}
	/* .hero .input .input-group {
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

		-moz-appearance:none; 
		-webkit-appearance:none; 
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
	} */
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