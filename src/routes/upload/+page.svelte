<script>
  import { getStorage, ref, uploadBytes } from 'firebase/storage';
  import { onMount } from 'svelte';

  let selectedFile;

  const storage = getStorage();

  const handleFileChange = (event) => {
    // Update selectedFile when the input value changes
    selectedFile = event.target.files[0];
    console.log('Selected file:', selectedFile);
  };

  const handleFileUpload = async () => {
    if (selectedFile) {
      const storageRef = ref(storage, 'pwtblog/' + selectedFile.name);
      await uploadBytes(storageRef, selectedFile);
      console.log('File uploaded to Firebase Storage!');
      
      // Show an alert after successful submission
      window.alert('File uploaded successfully!');
    } else {
      // Handle the case when no file is selected
      console.error('No file selected');
    }
  };

  
</script>

<input type="file" accept=".jpg, .jpeg, .png, .pdf, .mp4" on:change={handleFileChange} />
<button on:click={handleFileUpload}>Upload</button>
