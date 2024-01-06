<!-- AddProject.svelte -->
<script>
  import { getFirestore, collection, addDoc } from 'firebase/firestore';
  import { onMount } from 'svelte';

  const db = getFirestore();

  let projectName = '';
  let projectTags = '';
  let projectImage = '';
  let projectDescription = '';

  const addProject = async () => {
    if (projectName && projectDescription) {
      const projectData = {
        name: projectName,
        description: projectDescription,
        image: projectImage,
        tags: projectTags.split(',').map(tag => tag.trim()) // assuming tags are comma-separated
      };

      try {
        await addDoc(collection(db, 'projects'), projectData);
        console.log('Project added successfully!');
      } catch (error) {
        console.error('Error adding project:', error);
      }
    } else {
      console.warn('Please fill out all fields.');
    }
  };
</script>

<main class="add-project">
  <h2>Add a New Project</h2>
  <form>
    <label for="projectName">Project Name:</label>
    <input type="text" id="projectName" class="input border rounded-md w-full" bind:value={projectName} />

    <label for="projectTags">Project Tags:</label>
    <input type="text" id="projectTags" class="input border rounded-md w-full" bind:value={projectTags} />

    <label for="projectImage">Project Image:</label>
    <input type="text" id="projectImage" class="input border rounded-md w-full" bind:value={projectImage} />

    <label for="projectDescription">Project Description:</label>
    <textarea id="projectDescription" class="input border rounded-md w-full" bind:value={projectDescription}></textarea>

    <!-- Add other form fields as needed -->

    <button type="button" class="mt-5 bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded" on:click={addProject}>Add Project</button>
  </form>
</main>
