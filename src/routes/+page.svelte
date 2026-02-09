<script>
  import { onMount } from 'svelte';

  // 1. This is the "Cargo Container" for your data
  let rawData = "0,0,0,0,0,0:0:0,0"; // Default state
  
  // 2. Reactive variables (they update automatically when rawData changes)
  $: parts = rawData.split(',');
  $: lat = parts[0];
  $: lon = parts[1];
  $: speed = parts[2];
  $: alt = parts[3];
  $: usedSats = parts[4];
  $: time = parts[5];
  $: isActive = parts[6] === "1";

  // 3. The "Fetcher" - we will point this at your PHP file soon
  async function fetchTelemetry() {
    // For now, we just log to show it's working
    console.log("Fetching new data...");
    // rawData = await fetch('your-api-url').then(r => r.text());
  }

  onMount(() => {
    const interval = setInterval(fetchTelemetry, 60000); // Every minute
    return () => clearInterval(interval);
  });
</script>

<div class="dashboard-root">
  <div class="navBar">
    <h1>Telemetry Dashboard</h1>
    <div class="status-container">
      <p class="pulse-dot">{isActive ? "Active" : "Inactive"}</p>
        <span class="disclaimer">Data is fetched automatically every minute</span>
    </div>
  </div>
  <div class=data-container>
    <div class="data-map">
      <!-- Map or additional data visualization can go here -->
    </div>
    <div class="general-data">
      <div class="data-display">
        <p><strong>Latitude:</strong> {lat}</p>
        <p><strong>Longitude:</strong> {lon}</p>
        <p><strong>Speed:</strong> {speed} m/s</p>
        <p><strong>Altitude:</strong> {alt} m</p>
        <p><strong>Used Satellites:</strong> {usedSats}</p>
        <p><strong>Time:</strong> {time}</p>
      </div>
      <div class="data-logs">
        <!-- Logs or historical data can go here -->
      </div>
    </div>
  </div>
</div>

<style>
  :global(body) {
    margin: 0;
    background-color: #020617; /* Slate-950 */
    color: #f8fafc;
    font-family: 'Segoe UI', Roboto, sans-serif;
    overflow: hidden;
  }

  .dashboard-root {
    display: flex;
    flex-direction: column;
    height: 100vh;
    padding: 1rem;
    box-sizing: border-box;
  }
  
  .navBar {
    background-color: #1e293b; /* Slate-800 */
    padding: 1rem;
    border-radius: 0.5rem;
    margin-bottom: 1rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .navBar h1 {
    margin: 0;
    font-size: 1.5rem;
  }
  
  .data-container {
    display: grid;
    grid-template-columns: 2fr 1fr; /* 2/3 width for speed */
    grid-template-rows: 1fr 1fr;
    gap: 1rem;
    flex-grow: 1;
    margin-top: 1rem;
  }

  .data-display {
    background-color: #334155; /* Slate-700 */
    padding: 1rem;
    border-radius: 0.5rem;
    display: grid;
    gap: 0.5rem;
    justify-content: start;
  }

  .status-container {
    background-color: #394a63;
    color: white;
    border: none;
    padding: 0.5rem 1rem;
    border-radius: 0.5rem;
    cursor: pointer;
    position: relative;
    display:inline-flex;
    align-items: center;
  }
  .status-container:hover {
    background-color: #415c85;
  }

  .status-container:hover .disclaimer {
    max-width: 400px;
    opacity: 1;
    margin-left: 10px;
  }

  .disclaimer {
    max-width: 0;
    opacity: 0;
    overflow: hidden;
    white-space: nowrap;
    transition: all 0.4s ease;
    font-size: 14px;
    margin-left: 0;
  }
</style>
