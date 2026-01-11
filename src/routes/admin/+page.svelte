<script>
  import { onMount } from 'svelte';
  import { browser } from '$app/environment';
  
  onMount(() => {
    if (browser) {
      window.location.href = '/admin/index.html';
    }
  });
</script>

<p>Redirecting to admin...</p>
