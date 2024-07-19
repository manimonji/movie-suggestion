<script>
    import { createEventDispatcher } from "svelte";

    // export let url = '';
    let query = '';
    function fakeFetch(_,stuff) {
		return new Promise((resolve, reject) => {
			setTimeout(() => {
				resolve(new Response(JSON.stringify(
					stuff
				)));
			}, 200)
		})
	}
    const dispatch = createEventDispatcher();

    $: results = fakeFetch(query, ['James cameron', 'Blah']).then(response => response.json());
    let resultsMap;
    $: (async () => resultsMap = new Map(await results.then(arr => arr.map(v => [v, 0]))))();
    // fetch(url).then(response => response.json())
    // dispatch('querySent', { response: fakeFetch(query, ['James cameron', 'Blah']).then(response => response.json()) })
</script>
<section class="flex flex-col w-full">
    <input class="bg-neutral-900 p-2 rounded-md" placeholder="جستجو..." bind:value={query} >
    <section class="flex flex-grow p-5 gap-4 bg-neutral-800 items-start">
    <slot {results} {resultsMap}></slot>
    </section>
</section>