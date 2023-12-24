<script>
  import { onMount } from 'svelte';
  import { getFirestore, collection, getDocs } from 'firebase/firestore';
  

  let questions = [];
  let currentQuestion = 0;
  let userScore = 0;

  const loadQuestions = async () => {
    const db = getFirestore();
    const questionsCollection = collection(db, 'questions');
    const snapshot = await getDocs(questionsCollection);
    
    questions = snapshot.docs.map(doc => doc.data());
  };

  onMount(loadQuestions);

  const answerSelected = (selectedOption) => {
    if (selectedOption === questions[currentQuestion].correctAnswer) {
      userScore++;
    }

    if (currentQuestion < questions.length - 1) {
      currentQuestion++;
    } else {
      navigate('/results', { state: { userScore } });
    }
  };
</script>

<main class="p-4">
  {#if questions.length > 0}
    <h2 class="text-2xl font-bold mb-4">Question {currentQuestion + 1}</h2>
    <p class="mb-4">{questions[currentQuestion].text}</p>

    {#each questions[currentQuestion].options as option (option)}
      <button
        class="bg-blue-500 text-white px-4 py-2 rounded mb-2"
        on:click={() => answerSelected(option)}>
        {option}
      </button>
    {/each}
  {:else}
    <p>Loading questions...</p>
  {/if}
</main>
