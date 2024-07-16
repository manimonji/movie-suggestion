<script>
    import AreaChoice from "$lib/AreaChoice.svelte";

	import oldVideo from "$lib/videos/charlie.mp4"
	import oldTitle from "$lib/images/old-title.svg"
	import filledStar from "$lib/images/star-filled.svg"
	import outlinedStar from "$lib/images/star-outlined.svg"
    import { fade } from "svelte/transition";
    import Slider from "../lib/Slider.svelte";
    import Hero from "../lib/Hero.svelte";
    import OldFashioned from "../lib/OldFashioned.svelte";
    import Time from "../lib/Time.svelte";

	const stages = {
		landing: 'landing',
		inputs: 'inputs',
		favoriteMovies: 'favoriteMovies',
		suggestion: 'suggestion'
	}
	const stagesList = [stages.landing, stages.inputs, stages.favoriteMovies, stages.suggestion]
	let currentStageIndex = 0;
	$: if(currentStageIndex >= stagesList.length) {
		currentStageIndex = 0;
	}
	$: currentStage = stagesList[currentStageIndex]
	// let inputsVisible = false;
	function handleClick(e) {
		currentStageIndex++;
		console.log('index' + currentStageIndex);
	}
	let likesOldMovies = '';
	let dateArea = {min: 1980, max: 2024};
	let lengthArea = {min: 20, max: 80};
	// let generes = ["معمایی","مهیج"];
	// let ageRatings = ["+3", "+7", "+12", "+15", "+18"];
	// let choosenGenereIndex = 0;
	// // $: choosenGenere = generes[choosenGenereIndex];
	// let choosenAgeRating;
	let choice = '';
	let query = '';
	function fakeFetch(query ,stuff) {
		return new Promise((resolve, reject) => {
			setTimeout(() => {
				resolve(new Response(JSON.stringify(
					stuff
				)));
			}, 200)
		})
	}
	let query2 = '';
	$: results = fakeFetch(query, ['James cameron', 'Blah']).then(response => response.json());
	$: results2 = fakeFetch(query2, ['1','2','3','5','6','7','8']).then(response => response.json());
	let suggestions;
	let movieRatings = {};
</script>
{#if currentStage == stages.landing}
	<Hero on:ctaClick={() => currentStageIndex++}></Hero>
	<OldFashioned></OldFashioned>
	<Time></Time>
{:else if currentStage == stages.inputs}
	<!-- <article class="inputs">
		<section class="likes-old-movies section">
			<img class="image-title" src={oldTitle} alt="Do you like old movies?">
			<video class="video" src={oldVideo} autoplay muted loop>
				<track kind="captions">
			</video>
			<div class="buttons">
				<button on:click={() => likesOldMovies = true} class:active={likesOldMovies}>آره</button>
				<button on:click={() => likesOldMovies = false} class:active={!likesOldMovies}>نه</button>
			</div>
		</section>
		<section class="date-and-length section">
			<div class="date">
				<h2 class="title">تاریخ انتشار</h2>
				<AreaChoice min={1980} max={new Date().getFullYear()} unit="سال" mode="labled" on:change={(e) => dateArea = e.detail}></AreaChoice>
			</div>
			<div class="movie-length">
				<h2 class="title">طول فیلم</h2>
				<AreaChoice min={20} max={80} unit="دقیقه" mode="linear" on:change={(e) => lengthArea = e.detail}></AreaChoice>
			</div>
			<button class="btn next" on:click={() => currentStageIndex++}>بعدی</button>
		</section>
		<section class="favourite-director section">
			<h1 class="title" >کارگردان مورد علاقه ات کیه؟</h1>
			<p class="choice">
				{#if choice == ''}
					انتخاب کن!
				{:else}
					{choice}
				{/if}
			</p>
			<input type="text" class="search-box" placeholder="Cristopher Nolan" bind:value={query}>
			<section class="results">
				{#await results}
					<span class="loader" in:fade></span>
				{:then results} 
					{#each results as result}
						<button class="result" on:click={() => choice = result}>{result}</button>
					{/each}
				{/await}
				{#if query = ''}
					<p class="hint">نتایج میان اینجا</p>
				{/if}
			</section>
		</section>
	</article> -->
{:else if currentStage == stages.favoriteMovies}
	<article class="favorite-movie">
		<div class="background"></div>
		<section class="search-help">
			<h2>امتیاز دهی</h2>
			<p>به 6 تا 10 فیلم امتیاز دهید.</p>
		</section>
		<section class="search">
			<input type="text" class="search-box" bind:value={query2}>
			<section class="results" tabindex="-1">
				{#await results2}
					<span class="loader" in:fade></span>	
				{:then results}
					{#each results as result}
						<article class="result">
							<h4 class="name">{result}</h4>
							<section class="buttons">
								{#each Array(5) as _, i}
									<button class="star-btn" on:click={()=> movieRatings[result] = i + 1} class:filled={(i + 1) <= (movieRatings[result] || 0)}>
										<img src={outlinedStar} alt="A star" class="star">
										<img src={filledStar} alt="A star" class="star filled">
									</button>
								{/each}
							</section>
						</article>
					{/each}
				{/await}
			</section>
		</section>
		<section class="ratings-help">
			<h2>امتیاز های داده شده</h2>
			<p>اینجا می تونید امتیاز هایی که دادید رو ببینید.</p>
		</section>
		<section class="current-ratings">
			<!-- <section class="movies"> -->
				{#each Object.keys(movieRatings) as movieName}
					<article class="movie">
						<h4 class="name">{movieName}</h4>
						<section class="buttons">
							{#each Array(5) as _, i}
								<button class="star-btn" on:click={()=> movieRatings[movieName] = i + 1} class:filled={(i + 1) <= (movieRatings[movieName] || 0)}>
									<img src={outlinedStar} alt="A star" class="star">
									<img src={filledStar} alt="A star" class="star filled">
								</button>
							{/each}
						</section>
					</article>
				{/each}
			<!-- </section> -->
		</section>
		<section class="buttons">
			{#if Object.keys(movieRatings).length >= 6 && Object.keys(movieRatings).length <= 10}
				<button class="btn" on:click={() => {
					currentStageIndex++;
					suggestions = fakeFetch('TODO QUERY', ['Movie 1', 'Movie 2', 'Movie 3', 'Movie 4']).then(response => response.json());
				}}>پیشنهاد فیلم</button>
			{/if}
		</section>
	</article>
{:else if currentStage = stages.suggestion}
	<article class="suggestion">
		<h3>پیشنهاد ما</h3>
		{#await suggestions}
			<span class="loader" in:fade></span>	
		{:then movies} 
			<!-- {#each movies as movieName}
				<article class="movie">
					<h2 class="name">{movieName}</h2>
				</article>
			{/each}-->
			<Slider data={movies} let:item={movieName}>
				<article class="movie">
					<h2 class="name">{movieName}</h2>
				</article>
			</Slider>
		{/await}
	</article>
{/if}
<style>
	@font-face {
		font-family: sahel;
		src: url("$lib/fonts/sahel.woff2");
	}
	@font-face {
		font-family: vazirmatn;
		src: url("$lib/fonts/vazirmatn.woff2");
	}
	:global(body) {
		margin: 0;
	}
	:global(*){
		font-family: vazirmatn;
	}
	/* .hero {
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
	} */
	.inputs {
		box-sizing: border-box;
		display: grid;
		grid-template-columns: 1fr 1fr 1fr;
		/* gap: 30px; */

		height: 100vh;
		max-height: 100vh;

		/* padding: 30px; */
	}
	.inputs .section {
		max-height: 100vh;
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
	.inputs .favourite-director .choice {
		color: #fff;
		text-align: center;

		font-size: 20px;
	}
	/* .inputs .favourite-director */
	.search-box {
		box-sizing: border-box;

		width: 100%;

		padding: 16px;

		color: #fff;
		
		border: 1px solid hsla(0deg, 100%, 100%, 0.25);
		border-radius: 15px;

		font-size: 20px;

		direction: ltr;

		z-index: 1;
	}
	.inputs .favourite-director .search-box {
		background-color: hsl(0deg 0% 20%);
	} 
	/* .inputs .favourite-director  */
	.search-box:focus {
		border-color: hsl(220deg 100% 60%);
		outline: none;
	}
	.inputs .favourite-director .results {
		flex-grow: 1;
		display: flex;
		flex-wrap: wrap;
		align-items: baseline;
		gap: 16px;

		max-height: fit-content;

		overflow-y: scroll;

		--padding: 16px;
		padding: calc(15px + var(--padding)) var(--padding) var(--padding) var(--padding);

		position: relative;
		top: -15px;

		background-color: hsl(0deg 0% 5%);

		border-radius: 0 0 15px 15px;
	}
	.inputs .favourite-director .results .loader {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
	}
	.inputs .favourite-director .results .hint {
		box-sizing: border-box;
		padding: 16px;

		position: absolute;
		top: 50%;
		left: 50%;

		color: #fff;

		transform: translate(-50%, -50%);
	}
	.inputs .favourite-director .results .result {
		padding: 16px;

		color: #fff;

		text-align: left;
		
		background-color: hsl(0deg 0% 10%);

		border: none;
		border-radius: 5px;
	}
	.inputs .favourite-director .results .result:hover {
		background-color: hsl(0deg 0% 15%);
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
	.favorite-movie {
		display: grid;
		grid-template-columns: 300px 1fr;
		grid-auto-rows: fit-content;
		align-items: start;

		box-sizing: border-box;
		padding: 32px;

		/* background-color: hsl(0deg 0% 95%); */
	}
	.favorite-movie .search {
		direction: ltr;
		position: relative;
	}
	.favorite-movie .search-box {
		background-color: hsl(0deg 0% 90%);
		border-color: hsl(0deg 0% 50%);
		color: #000;

		position: relative;

		z-index: 2;
	}
	.favorite-movie .results, .favorite-movie .current-ratings {
		flex-wrap: wrap;
		gap: 15px;
	}
	.current-ratings {
		display: flex;
	}
	.favorite-movie .results {
		display: none;
		background-color: hsl(0deg 0% 92.5%);

		--padding: 15px;
		padding: calc(15px + var(--padding)) var(--padding) var(--padding) var(--padding);
	
		position: absolute;
		bottom: 15px;

		border-radius: 15px;
		transform: translateY(100%);

		z-index: 1;
	}
	.favorite-movie .results .result, .favorite-movie .current-ratings .movie{
		padding: 15px;

		background-color: #fff;
		
		border-radius: 10px;
	}
	.favorite-movie .results .result .name, .favorite-movie .current-ratings .movie .name{
		margin: 0;

		text-align: center;
	}
	.favorite-movie .results .result .buttons, .favorite-movie .current-ratings .movie .buttons {
		display: flex;
	}
	.favorite-movie .results .result .star-btn, .favorite-movie .current-ratings .movie .star-btn{
		display: block;

		padding: 0;
		margin: 0;

		position: relative;

		background: none;
		border: none;
	}
	.favorite-movie .results .result .star-btn.filled .star, .favorite-movie .current-ratings .movie .star-btn.filled .star {
		opacity: 0;
	}
	.favorite-movie .results .result .star-btn.filled .star.filled, .favorite-movie .current-ratings .movie .star-btn.filled .star.filled {
		opacity: 1;
	}
	.favorite-movie .results .result .star-btn:hover .star.filled, .favorite-movie .current-ratings .movie .star-btn:hover .star.filled {
		opacity: 0.2;
	}
	.favorite-movie .results .result .star-btn .star, .favorite-movie .current-ratings .movie .star-btn .star {
		display: block;
	}
	.favorite-movie .results .result .star-btn .star.filled,  .favorite-movie .current-ratings .movie .star-btn .star.filled {
		position: absolute;
		top: 0;
		left: 0;

		opacity: 0;
	}
	.favorite-movie .search:focus-within .results,
	.favorite-movie .search:hover .results {
		display: flex;
	}
	.favorite-movie > .buttons {
		display: flex;
		justify-content: center;
		grid-column: span 2;

		margin-top: 30px;
	}
	.suggestion {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;

		height: 100vh;
	}
	.suggestion .movie .name {
		margin: 0;
	}
	.loader {
		color: #fff;
		font-size: 10px;
		width: 1em;
		height: 1em;
		border-radius: 50%;
		position: relative;
		text-indent: -9999em;
		animation: mulShdSpin 1.3s infinite linear;
		transform: translateZ(0);
	}
	.background {
		width: 100%;
		height: 100%;

		position: absolute;
		top: 0;
		left: 0;

		background-color: hsl(0deg 0% 97.5%);

		z-index: -1;
	}
	@keyframes mulShdSpin {
		0%,
		100% {
			box-shadow: 0 -3em 0 0.2em, 
			2em -2em 0 0em, 3em 0 0 -1em, 
			2em 2em 0 -1em, 0 3em 0 -1em, 
			-2em 2em 0 -1em, -3em 0 0 -1em, 
			-2em -2em 0 0;
		}
		12.5% {
			box-shadow: 0 -3em 0 0, 2em -2em 0 0.2em, 
			3em 0 0 0, 2em 2em 0 -1em, 0 3em 0 -1em, 
			-2em 2em 0 -1em, -3em 0 0 -1em, 
			-2em -2em 0 -1em;
		}
		25% {
			box-shadow: 0 -3em 0 -0.5em, 
			2em -2em 0 0, 3em 0 0 0.2em, 
			2em 2em 0 0, 0 3em 0 -1em, 
			-2em 2em 0 -1em, -3em 0 0 -1em, 
			-2em -2em 0 -1em;
		}
		37.5% {
			box-shadow: 0 -3em 0 -1em, 2em -2em 0 -1em,
			3em 0em 0 0, 2em 2em 0 0.2em, 0 3em 0 0em, 
			-2em 2em 0 -1em, -3em 0em 0 -1em, -2em -2em 0 -1em;
		}
		50% {
			box-shadow: 0 -3em 0 -1em, 2em -2em 0 -1em,
			3em 0 0 -1em, 2em 2em 0 0em, 0 3em 0 0.2em, 
			-2em 2em 0 0, -3em 0em 0 -1em, -2em -2em 0 -1em;
		}
		62.5% {
			box-shadow: 0 -3em 0 -1em, 2em -2em 0 -1em,
			3em 0 0 -1em, 2em 2em 0 -1em, 0 3em 0 0, 
			-2em 2em 0 0.2em, -3em 0 0 0, -2em -2em 0 -1em;
		}
		75% {
			box-shadow: 0em -3em 0 -1em, 2em -2em 0 -1em, 
			3em 0em 0 -1em, 2em 2em 0 -1em, 0 3em 0 -1em, 
			-2em 2em 0 0, -3em 0em 0 0.2em, -2em -2em 0 0;
		}
		87.5% {
			box-shadow: 0em -3em 0 0, 2em -2em 0 -1em, 
			3em 0 0 -1em, 2em 2em 0 -1em, 0 3em 0 -1em, 
			-2em 2em 0 0, -3em 0em 0 0, -2em -2em 0 0.2em;
		}
	}
</style>