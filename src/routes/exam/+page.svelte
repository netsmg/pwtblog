<script>
  import { onMount } from 'svelte';
  import { getDatabase, ref, onValue } from 'firebase/database';

  let exams = [];

  onMount(() => {
    // Initialize Firebase database
    const db = getDatabase();

    // Reference to 'jachai/exams/practice'
    const examsRef = ref(db, 'exams');

    // Fetch exams from Firebase
    onValue(examsRef, (snapshot) => {
      exams = snapshot.val();
    });
  });
</script>

<style>
  /* Add your styles here */
</style>

<main>
  {#if exams && exams.length > 0}
    {#each exams as exam (exam.key)}
      <div class="exam">
        <div class="logo">{exam.details.name[0]}</div>
        <div class="details">
          <div class="title">{exam.details.name}</div>
          <div class="others">
            প্রশ্নঃ {exam.questions.length} টি | সময়ঃ {exam.details.duration} মিনিট | স্কোরঃ {exam.questions.length} | নেগেটিভঃ {exam.details.negative}
          </div>
        </div>
      </div>
    {/each}
  {:else}
    <p>No exams available</p>
  {/if}
</main>
