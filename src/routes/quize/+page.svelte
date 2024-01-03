<script>
  import { onMount } from 'svelte';
  import { getDatabase, ref, get } from 'firebase/database';

  let exams = [];

  onMount(async () => {
    const db = getDatabase();

    // Assuming 'exams' is the root node in your Firebase Realtime Database
    const examsRef = ref(db, 'exams');
    const examsSnapshot = await get(examsRef);

    exams = examsSnapshot.val();
  });
</script>

<main>
  {#if exams}
    {#each Object.entries(exams) as [id, { details, questions }]}
      <section>
        <h2>{details.name}</h2>
        <p>Duration: {details.duration} minutes</p>
        <p>Negative Marking: {details.negative ? 'Yes' : 'No'}</p>

        <ul>
          {#each Object.values(questions) as { text, options, correctAnswer }}
            <li>
              <p>{text}</p>
              <ul>
                {#each options as option}
                  <li>{option}</li>
                {/each}
              </ul>
              <p>Correct Answer: {correctAnswer}</p>
            </li>
          {/each}
        </ul>
      </section>
    {/each}
  {/if}
</main>

<style>
  section {
    margin-bottom: 20px;
  }
</style>
