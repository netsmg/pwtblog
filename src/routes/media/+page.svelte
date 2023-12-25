<script>
  import { onMount } from 'svelte';
  import { getStorage, ref, listAll } from 'firebase/storage';

  let filesList = [];

  const storage = getStorage();
  const storageRef = ref(storage, 'pwtblog/');

  const generateDownloadLink = (file) => {
    // Generate the download link for the file
    return `https://firebasestorage.googleapis.com/v0/b/contact-database-9c47b.appspot.com/o/pwtblog/${encodeURIComponent(file)}`;
  };

  const getFilesList = async () => {
    const filesList = await listAll(storageRef);
    return filesList.items.map((item) => item.fullPath);
  };

  onMount(async () => {
    filesList = await getFilesList();
  });
</script>

<ul>
  {#each filesList as file}
    <li>
      <a href={generateDownloadLink(file)} download>
        {file}
      </a>
    </li>
  {/each}
</ul>
