<script>
    import AreaChoice from "$lib/AreaChoice.svelte";
</script>
<section class="w-full flex bg-stone-950 text-gray-50 p-10 items-center">
    <section class="flex-1">
        <h2 class="text-7xl">فیلمش چقدر طول بکشه؟</h2>
        <p class="text-slate-300 text-xl mt-5">اگه برات مهم نیست، اسکرول کن</p>
    </section>
    <section class="flex-1">
        <AreaChoice min={20} max={80} mode="linear" unit="دقیقه" inputClassNames="bg-black w-min">
            <div slot="circle" class="w-4 h-4 bg-white rounded-lg border-2 border-neutral-500"></div>
        </AreaChoice>
    </section>
</section>