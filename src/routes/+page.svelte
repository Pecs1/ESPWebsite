<script>
  import { onMount } from 'svelte';

  // 1. This is the "Cargo Container" for your data
  let rawData = "0,0,0,0,0,0,0:0:0,0"; // Default state
  
  // 2. Reactive variables (they update automatically when rawData changes)
  $: parts = rawData.split(',');
  $: latitude = parts[0];
  $: longitude = parts[1];
  $: speed = parts[2];
  $: altitude = parts[3];
  $: usedSats = parts[4];
  $: accuracy = parts[5]
  $: time = parts[6];
  $: isActive = parts[7] === "1";

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
  <div class="nav-bar">
    <div class="main-title">
      <h1>Telemetry Dashboard</h1>
      <span class="disclaimer">Data is fetched automatically every minute</span>
    </div>
    <div class="status">{isActive ? "Active" : "Inactive"}
     <!-- <p class="pulse-dot"></p> -->
    </div>
  </div>
  <div class=data-container>
    <div class="data-map">
      <!-- Map or additional data visualization can go here -->
    </div>
    <div class="general-data">
      <div class="data-display">
        <h1><strong><centre>Telemetry Data</centre></strong></h1>
        <p><strong>Latitude:</strong> {latitude}</p>
        <p><strong>Longitude:</strong> {longitude}</p>
        <p><strong>Altitude:</strong> {altitude} m</p>
        <p><strong>Speed:</strong> {speed} m/s</p>
        <p><strong>Accuracy:</strong> {accuracy}</p>
        <p><strong>Used Satellites:</strong> {usedSats}</p>
        <p><strong>Time:</strong> {time}</p>
      </div>
      <div class="data-logs">
        <h1><strong><centre>Telemetry log</centre></strong></h1>
        <!-- some .js thingy to store the logs -->
        <p>ur ur ur</p>
      </div>
    </div>
  </div>
</div>

<style>
  :global(body) {
    margin: 0;
    background-color: #020617;
    color: #f8fafc;
    font-family: 'Segoe UI', Roboto, sans-serif;
    overflow: hidden;
    cursor: default;
    user-select: none; 
  }

  .dashboard-root {
    display: flex;
    flex-direction: column;
    height: 100vh;
    padding: 1rem;
    box-sizing: border-box;
  }
  
  .nav-bar {
    background-color: #1e293b;
    padding: 1rem;
    border-radius: 0.5rem;
    margin-bottom: 1rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .main-title {
    background-color: #242f40;
    color: white;
    border: none;
    font-size: 1.2rem;
    padding: 0.5rem 1rem;
    border-radius: 0.5rem;
    cursor: pointer;
    position: relative;
    display:inline-flex;
    align-items: center;
  }
  
  .main-title:hover {
    background-color: #3b4d68;
  }

  .main-title:hover .disclaimer {
    max-width: 400px;
    opacity: 1;
    margin-left: 10px;
  }

  .disclaimer {
    max-width: 0;
    opacity: 0;
    font-size: 1rem;
    overflow: hidden;
    white-space: nowrap;
    transition: all 0.4s ease;
    margin-left: 0;
    color: aliceblue;
  }

  .data-container {
    display: grid;
    grid-template-columns: 2fr 0.75fr;
    gap: 1.25rem;
    flex-grow: 1;
    margin-top: 1rem;
  }

  .general-data {
    display:grid;
    grid-template-rows: 0.5fr 1fr;
  }

  .data-display {
    background-color: #2d3a4b;
    border-radius: 0.5rem;
    padding: 2rem 0.75rem;
    width: 100%;
    height: 100%;
  }

  .data-display h1 {
    margin-top: 0;
    margin-bottom: 1rem;
    color: #f8fafc;
    font-size: 20px;
    text-align: center;
  }

  .data-display p {
    margin: 0.25rem 0;
    font-size: 1.1rem;
  }

  .status {
    background-color: #2d3a4b;
    color: white;
    border: none;
    padding: 0.5rem 1rem;
    border-radius: 0.5rem;
  }

  .data-map {
    background-color: #2d3a4b;
    border-radius: 0.5rem;
    max-width: 100%;
    max-height: 100%;
  }

  .data-logs {
    background-color: #2d3a4b;
    border-radius: 0.5rem;
    padding: 2rem 0.75rem;
    height: 96.5%;
    width: 100%;
    margin-top: 3.5%
  }

  .data-logs h1 {
    margin-top: 0;
    margin-bottom: 1rem;
    color: #f8fafc;
    font-size: 20px;
    text-align: center;
  }

  .data-logs p {
    margin: 0.25rem 0;
    font-size: 1.1rem;
  }

</style>
