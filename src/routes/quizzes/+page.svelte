
<script>
  import { onMount } from 'svelte';
  import { getDatabase, ref, query, get } from 'firebase/database';

  export let topicID;
  export let id;
  export let noq;
  export let title;
  export let type;

  let imageLoaded = false;

  let loading = true;
  let error = false;
  let quiz = [];

  const db = getDatabase();
  const quizRef = ref(db, `quizzes/${topicID}/questions`);
  const quizQuery = query(quizRef);

  onMount(async () => {
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

  function getImageLink() {
    return type === 'video'
      ? `http://img.youtube.com/vi/${id}/maxresdefault.jpg`
      : `https://stpl.vercel.app/${id}.webp`;
  }

  function getTypeClass() {
    return type === 'quiz' ? 'flex h-28 flex-col justify-between gap-1' : 'h-14';
  }
</script>



<div class="mx-auto mb-32 flex min-h-screen w-[90%] animate-reveal flex-col items-center">
  <h1 class="page-heading">Attempt Quizzes</h1>

  {#if quiz.length > 0}
    <div class="mx-auto grid h-full w-full grid-cols-quizzes justify-items-center gap-7">
      {#each quiz as topic (topic.topicID)}
        {#if topic.noq > 0}
          <a href={`/quiz/${topic.topicID}`} target="_blank">
            <div class="card max-w-lg cursor-pointer rounded-lg border-2 transition-all duration-300 hover:border-primary" title={title}>
              <div class="card relative aspect-video w-full overflow-hidden rounded-lg p-0">
                <img alt={title} class="h-auto w-full animate-reveal object-cover" loading="lazy" src={getImageLink()} on:load={() => (imageLoaded = true)} />
                {#if imageLoaded}
                  <img alt="" class="h-full w-full object-cover" src={placeholder} />
                {/if}
              </div>
              <div class={getTypeClass()}>
                <p class="mt-1 line-clamp-2 overflow-hidden text-center font-semibold uppercase tracking-wide text-black dark:text-slate-300 sm:text-lg">
                  {title}
                </p>
                {#if type === 'quiz'}
                  <div class="flex justify-between rounded-lg border-2 border-black/10 px-3 py-1 text-sm font-medium text-black drop-shadow-md dark:border-white/10 dark:text-slate-300 sm:text-base">
                    <p>{noq}x Questions</p>
                    <p>{noq * 10} Points</p>
                  </div>
                {/if}
                {#if type === 'popularQuiz'}
                  <!-- Modify as needed for the data you want to display -->
                {/if}
              </div>
            </div>
          </a>
        {:else}
          <div class="w-full">
            <div class="card max-w-lg cursor-pointer rounded-lg border-2 transition-all duration-300 hover:border-primary" title={title}>
              <!-- Adjust the content based on your needs -->
            </div>
          </div>
        {/if}
      {/each}
    </div>
  {:else}
    <p>No data found!</p>
  {/if}

  <div class="flex items-center justify-center text-center text-xl">
    {#if !loading && quiz.length === 0}
      <p>No data found!</p>
    {/if}
    {#if error}
      <p>There was an error! Perhaps you are not a registered user; only registered users can get access. To get access, fill up the contact-form.</p>
    {/if}
    {#if loading}
      <p>Loading ...</p>
    {/if}
  </div>
</div>


