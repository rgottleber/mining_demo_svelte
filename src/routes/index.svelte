<script>
	import { onMount } from 'svelte';
	import { ethers } from 'ethers';
	import Miner from '../contracts/Miner.json';
	import GetDwarf from '../components/GetDwarf.svelte';
	import SetDwarf from '../components/SetDwarf.svelte';
	$: account = null;
	$: chainID = null;
	let web3Props;
	const contractAddress = '0x0Bd6e788347a0dE65558fB50003F65235d91dE56';
	onMount(async () => {
		try {
			const { ethereum } = window;
			const provider = new ethers.providers.Web3Provider(ethereum);
			const signer = provider.getSigner();
			const minerContract = new ethers.Contract(contractAddress, Miner.abi, signer);
			web3Props = {
				minerContract
			};
			if (!ethereum) {
				alert('Get MetaMask!');
				return;
			}
			const accounts = await ethereum.request({ method: 'eth_accounts' });
			if (accounts.length !== 0) {
				account = accounts[0];
				chainID = await signer.getChainId();
			}
		} catch (error) {
			console.log(error);
		}
	});
	async function attachWallet() {
		const provider = new ethers.providers.Web3Provider(window.ethereum, 'any');
		// Prompt user for account connections
		await provider.send('eth_requestAccounts', []);
		const signer = provider.getSigner();
		account = await signer.getAddress();
		chainID = await signer.getChainId();
	}
</script>

<h1>⛏️ Who's Adding Blocks? ⛏️</h1>
<p>
	Contract can be found <a href={`https://rinkeby.etherscan.io/address/${contractAddress}`}
		>on Etherscan</a
	>
</p>

{#if !account}
	<p>This application is built on the Rinkeby Test Network.</p>
	<p>Please connect your wallet to Rinkeby Test Network.</p>
	<p>
		<button on:click={attachWallet}>Attach Wallet</button>
	</p>
{:else if chainID != 4}
	<h3>Please use the Rinkeby TestNet</h3>
	<p>Network Name: Rinkeby Testnet</p>
	<p>New RPC URL: https://rinkeby.infura.io/</p>
	<p>Chain ID: 4</p>
	<p>Currency Symbol: ETH</p>
	<p>Block Explorer URL: https://rinkeby.etherscan.io</p>
{:else}
	<SetDwarf {...web3Props} />
	<GetDwarf {...web3Props} />
{/if}

<style>
	h1,
	h3,
	p {
		text-align: center;
	}
</style>
