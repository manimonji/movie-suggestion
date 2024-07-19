<script>
    import AreaChoice from "$lib/AreaChoice.svelte";
</script>
<section class="w-full flex gap-16 bg-stone-950 text-gray-50 p-10 items-center">
    <section class="flex-1">
        <AreaChoice min={1980} max={2023} mode="labeled" unit="سال" inputClassNames="bg-black w-min">
            <div slot="circle" class="w-4 h-4 bg-white rounded-lg border-2 border-neutral-500"></div>
        </AreaChoice>
    </section>
    <section class="flex-1">
        <h2 class="text-7xl">فیلمش برای چه دوره ای باشه؟</h2>
        <p class="text-slate-300 text-xl mt-5">اگه برات مهم نیست، اسکرول کن</p>
    </section>
</section>