<!-- FileList.svelte -->
<script>
  import { onMount } from 'svelte';
  import { getStorage } from 'firebase/storage';

  const storage = getStorage();
  let files = [];
  let error = null;

  onMount(async () => {
    try {
      const listRef = storage.ref('/');
      const items = await listRef.listAll();

      files = items.items.map((item) => ({
        name: item.name,
        downloadUrl: item.getDownloadURL(),
      }));
    } catch (e) {
      console.error('Error fetching files:', e.message);
      error = 'Error fetching files. Please try again later.';
    }
  });
</script>

{#if error}
  <p>{error}</p>
{:else}
  <ul>
    {#each files as { name, downloadUrl }}
      <li>
        {name} - <a href={downloadUrl} download>Download</a>
      </li>
    {/each}
  </ul>
{/if}
