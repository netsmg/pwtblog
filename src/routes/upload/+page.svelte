<script>
  import { getStorage, ref, uploadBytes } from 'firebase/storage';
  import { onMount } from 'svelte';

  let selectedFile;

  const storage = getStorage();

  const handleFileUpload = async () => {
    const storageRef = ref(storage, 'pwtblog/' + selectedFile.name);
    await uploadBytes(storageRef, selectedFile);
    console.log('File uploaded to Firebase Storage!');
    
    // Show an alert after successful submission
    window.alert('File uploaded successfully!');
  };

  
</script>

<input type="file" class="input" accept=".jpg, .jpeg, .png, .pdf, .mp4" on:change={handleFileUpload} />
