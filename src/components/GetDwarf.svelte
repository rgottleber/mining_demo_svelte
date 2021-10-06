<script>
	export let minerContract;
	$: currentDwarf = {};
	const getDwarf = async () => {
		const currentDwarfData = await minerContract.getCurrentDwarf();
		currentDwarf = {
			dwarf: currentDwarfData,
			image: `/${currentDwarfData}.png`
		};
		minerContract.on('NewDwarf', (dwarf) => {
			currentDwarf = {
				dwarf: dwarf,
				image: `/${dwarf}.png`
			};
		});

		return currentDwarf;
	};
	getDwarf();
</script>

<img src={currentDwarf.image} alt={currentDwarf.name} />

<style>
	img {
		display: block;
		margin-left: auto;
		margin-right: auto;
		width: 25%;
	}
</style>
