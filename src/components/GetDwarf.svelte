<script>
	export let minerContract;
	$: currentDwarf = {};
	const getDwarf = async () => {
		// Call the getCurrentDwarf function in the miner contract
		// Get the current dwarf's data
		// Update the page on event emit
		// Return the current dwarf
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
