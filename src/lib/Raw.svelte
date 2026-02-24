<script lang="ts">
	import { onMount } from 'svelte';

	export let latitude;
	export let longitude;
	export let speed;
	export let altitude;
	export let usedSats;
	export let accuracy;
	export let time;
	export let isActive;

	// 1. This is the "Cargo Container" for your data
	let rawData = '0,0,0,0,0,0,0:0:0,0'; // Default state

	// 2. Reactive variables (they update automatically when rawData changes)
	$: parts = rawData.split(',');
	$: latitude = parts[0];
	$: longitude = parts[1];
	$: speed = parts[2];
	$: altitude = parts[3];
	$: usedSats = parts[4];
	$: accuracy = parts[5];
	$: time = parts[6];
	$: isActive = parts[7] === '1';

	// 3. The "Fetcher" - we will point this at your PHP file soon
	async function fetchTelemetry() {
		// For now, we just log to show it's working
		console.log('Fetching new data...');
		// rawData = await fetch('your-api-url').then(r => r.text());
	}

	onMount(() => {
		const interval = setInterval(fetchTelemetry, 60000); // Every minute
		return () => clearInterval(interval);
	});
</script>
