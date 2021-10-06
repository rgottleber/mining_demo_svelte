<script>
	export let minerContract;
	$: mining = false;
	let dwarves = [
		{ id: 0, text: `Bashful` },
		{ id: 1, text: `Doc` },
		{ id: 2, text: `Dopey` },
		{ id: 3, text: `Grumpy` },
		{ id: 4, text: `Happy` },
		{ id: 5, text: `Sleepy` },
		{ id: 6, text: `Sneezy` }
	];
	let selected;
	async function handleSubmit() {
		const txn = await minerContract.changeDwarf(selected.id);
		mining = true;
		await txn.wait();
		mining = false;
	}
</script>

<form on:submit|preventDefault={handleSubmit}>
	<select bind:value={selected}>
		{#each dwarves as dwarf}
			<option value={dwarf}>
				{dwarf.text}
			</option>
		{/each}
	</select>
	<button type="submit"> Submit </button>
</form>

{#if mining}
	<div id="background" />
	<div id="modal">
		<p>⚒️ mining ⚒️</p>
		<img src="/mining.gif" alt="mining" />
	</div>
	<audio autoplay loop>
		<source src="/mining.mp3" type="audio/mpeg" />
	</audio>
{/if}

<style>
	p {
		text-align: center;
		font-size: 2em;
	}
	form {
		display: flex;
		flex-direction: column;
		align-items: center;
	}
	select {
		width: 30%;
		padding: 10px;
		border-radius: 5px;
		border: 1px solid #ccc;
		font-size: 1.2em;
		margin: 10px;
	}
	button {
		width: 30%;
		padding: 10px;
		border-radius: 5px;
		border: 1px solid #ccc;
		font-size: 1.2em;
		margin: 10px;
	}
	#background {
		position: fixed;
		z-index: 1;
		top: 0;
		left: 0;
		width: 100vw;
		height: 100vh;
	}

	#modal {
		position: fixed;
		z-index: 2;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		background: #fff;
		filter: drop-shadow(0 0 20px #333);
	}
</style>
