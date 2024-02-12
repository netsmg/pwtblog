<script>
   
  import { onMount, onDestroy } from 'svelte';
  import { writable } from 'svelte/store';
  import { doc, getDoc, setDoc } from 'firebase/firestore';
  import { fauth } from "../../firebase";

  let user = null; // Initialize user variable
  let editMode = false; // Initialize edit mode variable
  let bio = writable(''); // Initialize bio variable as a writable store

  // Function to fetch user data from Firebase Authentication and Firestore
  async function fetchUserData() {
    const currentUser = fauth().currentUser; // Get current user
    if (currentUser) {
      // If user is logged in, set user data
      user = {
        displayName: currentUser.displayName,
        email: currentUser.email,
        photoURL: currentUser.photoURL,
        // Add more user data as needed
      };
      
      // Fetch user's bio from Firestore
      const userDoc = doc(db, 'users', currentUser.uid);
      const userSnapshot = await getDoc(userDoc);
      if (userSnapshot.exists()) {
        bio.set(userSnapshot.data().bio);
      }
    }
  }

  // Fetch user data when component mounts
  onMount(async () => {
    await fetchUserData();
  });

  // Log out function
  function logout() {
    fauth().signOut(); // Sign out current user
  }

  // Function to save user data to Firestore
  async function saveUserData() {
    const currentUser = fauth().currentUser; // Get current user
    if (currentUser) {
      // Update user's bio in Firestore
      const userDoc = doc(db, 'users', currentUser.uid);
      await setDoc(userDoc, { bio: $bio });
    }
  }

  // Clean up function when component is destroyed
  onDestroy(() => {
    user = null; // Reset user data
  });
</script>

{#if user}
  <div>
    <h1>Welcome, {user.displayName || user.email}!</h1>
    {#if user.photoURL}
      <img src={user.photoURL} alt="User Profile Picture" />
    {/if}
    <p>Email: {user.email}</p>
    <!-- Add more user information here -->
    {#if editMode}
      <!-- Edit mode -->
      <textarea bind:value={$bio}></textarea>
      <button on:click={saveUserData}>Save</button>
      <button on:click={() => editMode = false}>Cancel</button>
    {:else}
      <!-- View mode -->
      <p>Bio: {$bio}</p>
      <button on:click={() => editMode = true}>Edit Bio</button>
      <button on:click={logout}>Log Out</button>
    {/if}
  </div>
{:else}
  <p>Loading...</p>
{/if}
