<script>
    export let firstLabel;
    export let secondLabel;

    export let on = true;
</script>
<div class="flex items-center text-xl" >
    {firstLabel}
    <button class="mx-3 h-8 w-14 p-1 bg-slate-700 rounded-full relative" on:click={() => on = !on}>
        <div class="rounded-full relative bg-white aspect-square h-full transition-all" class:right-0={on} class:translate-x-full={!on} class:right-full={!on}></div>
    </button>
    {secondLabel}
</div>