<script>
    import AreaChoice from "$lib/AreaChoice.svelte";
</script>
<section class="flex bg-stone-950 text-gray-50 p-10 items-center">
    <section class="flex-1">
        <h2 class="text-7xl">فیلمش چقدر طول بکشه؟</h2>
		<p class="text-slate-300 text-xl mt-5">فیلم های چارلی چاپلین، لورل و هاردی، آلفرد هیچکاک، پدر خوانده، خلاصه هرچی خفنه. این جور فیلم ها رو دوست داری؟</p>
    </section>
    <section class="flex-1">
        <AreaChoice min={20} max={80} mode="linear" unit="دقیقه"></AreaChoice>
    </section>
</section>