<script>
  import { onMount } from 'svelte';
  import { getDatabase, ref, query, get } from 'firebase/database';
  
  export let topicID;

  let loading = true;
  let error = false;
  let quiz = [];

  onMount(async () => {
    const db = getDatabase();
    const quizRef = ref(db, `quizzes/${topicID}/questions`);
    const quizQuery = query(quizRef);

    try {
      const snapshot = await get(quizQuery);
      loading = false;

      if (snapshot.exists()) {
        quiz = Object.values(snapshot.val());
      }
    } catch (err) {
      loading = false;
      error = true;
    }
  });
</script>

<div class="mx-auto mb-32 flex min-h-screen w-[90%] animate-reveal flex-col items-center">
  <h1 class="page-heading">Attempt Quizzes</h1>

  {quiz.length > 0 && (
    <div class="mx-auto grid h-full w-full grid-cols-quizzes justify-items-center gap-7">
      {#each quiz as topic (topic.topicID)}
        {#if topic.noq > 0}
          <a href={`/quiz/${topic.topicID}`} target="_blank">
            <Thumbnail {id=topic.topicID} {noq=topic.noq} {title=topic.title} {type="quiz"} />
          </a>
        {:else}
          <div class="w-full">
            <Thumbnail {id=topic.topicID} {noq=topic.noq} {title=topic.title} {type="quiz"} />
          </div>
        {/if}
      {/each}
    </div>
  )}

  <div class="flex items-center justify-center text-center text-xl">
    {!loading && quiz.length === 0 && <>No data found!</>}
    {error && <>There was an error! Perhaps you are not a registered user; only registered users can get access. To get access, fill up the contact-form.</>}
    {loading && <>Loading ...</>}
  </div>
</div>

<!-- Footer component goes here -->
