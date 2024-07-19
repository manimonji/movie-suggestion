<script>
    import SearchBox from "$lib/SearchBox.svelte";

    let favouriteDirector = '';
</script>
<section class="w-full flex bg-stone-950 text-gray-50 p-10 items-center">
    <section class="flex-1">
        <h2 class="text-7xl">کارگردان مورد علاقه ات کیه؟</h2>
		<p class="text-slate-300 text-xl mt-5"></p>
    </section>
    <section class="flex-1 flex self-stretch items-stretch">
        <SearchBox let:results={results} let:resultsMap={resultsMap}>
            {#await results}
                <p>loading...</p>
            {:then results} 
                {#each [...(favouriteDirector == '' || resultsMap.has(favouriteDirector) ? [] : [favouriteDirector]), ...results] as directorName}
                    <button class={`py-2 px-3 rounded-md text-black shadow-[0_0.5rem_0_hsl(0deg,_0%,_60%)] focus:shadow-none hover:rounded-2xl transition-all ${favouriteDirector == directorName ? 'shadow-none translate-y-2 bg-blue-700 text-white' : 'bg-neutral-100'}`} on:click={() => favouriteDirector = directorName}>
                        {directorName}
                    </button>
                    {@debug favouriteDirector, directorName}
                {/each}
            {/await}
        </SearchBox>
    </section>
</section>