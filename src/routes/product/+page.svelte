<script>
  import { onMount } from "svelte";
  import { collection, getDocs, query, orderBy } from "firebase/firestore";
  import { getFirestore } from 'firebase/firestore';

  let products = [];
  const db = getFirestore();

  let searchTerm = '';
  let searchResults = [];

  const searchPosts = async () => {
    searchResults = [];

    const q = query(
      collection(db, "posts"),
      orderBy("createdAt", "desc")
    );

    const querySnapshot = await getDocs(q);

    querySnapshot.forEach((doc) => {
      const post = {
        ...doc.data(),
        id: doc.id
      };

      const lowerCaseSearchTerm = searchTerm.toLowerCase();
      const postMatchesSearch =
        post.title.toLowerCase().includes(lowerCaseSearchTerm) ||
        post.content.toLowerCase().includes(lowerCaseSearchTerm) ||
        post.tags.some((tag) => tag.toLowerCase().includes(lowerCaseSearchTerm));

      if (searchTerm === '' || postMatchesSearch) {
        searchResults.push(post);
      }
    });

    // Open the modal when search results are available
    if (searchResults.length > 0) {
      openModal();
    }
  };

  let modalOpen = false;

  const openModal = () => {
    modalOpen = true;
  };

  const closeModal = () => {
    modalOpen = false;
  };

  onMount(() => {
    document.title = "PWTBLOG | Search";
  });
</script>

<main>
  <h1>Search Page</h1>
  
  <input bind:value={searchTerm} placeholder="Search posts" on:input={searchPosts} />
  <!-- No need for a separate search button -->

  {#if searchResults.length > 0}
    <button on:click={openModal} class="btn variant-filled-primary">Open Results</button>
  {/if}

  {#if modalOpen}
    <div class="modal">
      <div class="modal-content">
        <span on:click={closeModal} class="close">&times;</span>
        
        <h2>Search Results</h2>
        
        {#each searchResults as result, index}
          <div>
            <p><a href="/blog/{result.id}">{result.title}</a></p>
            <!-- Display other post details as needed -->
          </div>
        {/each}
      </div>
    </div>
  {/if}
</main>

<style>
  /* Modal styles */
  .modal {
    display: block;
    position: fixed;
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgba(0, 0, 0, 0.4);
    padding-top: 60px;
  }

  .modal-content {
    background-color: #fefefe;
    margin: 5% auto;
    padding: 20px;
    border: 1px solid #888;
    width: 80%;
  }

  .close {
    color: #aaa;
    float: right;
    font-size: 28px;
    font-weight: bold;
  }

  .close:hover,
  .close:focus {
    color: black;
    text-decoration: none;
    cursor: pointer;
  }
</style>
