script>
  import { onMount } from 'svelte';
  import { getStorage, ref, listAll, getDownloadURL } from 'firebase/storage';
  const storage = getStorage();
  
  let files = [];

  const fetchFiles = async () => {
    const storageRef = ref(storage, '/'); // Reference to the root of your Storage bucket

    try {
      const listResult = await listAll(storageRef);
      files = await Promise.all(listResult.items.map(async (item) => {
        const downloadURL = await getDownloadURL(item);
        return { name: item.name, downloadURL };
      }));
    } catch (error) {
      console.error('Error fetching files:', error);
    }
  };

  onMount(() => {
    fetchFiles();
  });
</script>

<h1>Your Firebase Storage Files</h1>

<ul>
  {#each files as file}
    <li>
      {file.name} - <a href={file.downloadURL} download={file.name}>Download</a>
    </li>
  {/each}
</ul>
