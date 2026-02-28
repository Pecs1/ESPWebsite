<script>
  import { onMount } from 'svelte';
  
  // center of the EU
  const fallback = [49.843, 9.902];
  export let initialView = fallback;

  onMount(() => {
    console.log("Attempting geolocation...");

    if ("geolocation" in navigator) {
      navigator.geolocation.getCurrentPosition(
        (position) => {
          initialView = [position.coords.latitude, position.coords.longitude];
          console.log("Success:", initialView);
        },
        (error) => {
          console.warn(`OS/Hardware rejected geo: ${error.message}`);
          initialView = fallback; 
        },
        { timeout: 3000 }
      );
    } else {
      initialView = fallback;
    }
  });
</script>
