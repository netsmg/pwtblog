<script>
  import { getFirestore, collection, addDoc } from 'firebase/firestore';
  import { navigate } from 'svelte-routing';

  let newQuestions = [
    {
      text: '',
      options: [],
      correctAnswer: '',
    },
  ];

  const addQuestion = async () => {
    const db = getFirestore();
    const questionsCollection = collection(db, 'questions');

    try {
      for (const question of newQuestions) {
        const docRef = await addDoc(questionsCollection, question);
        console.log('Question added with ID: ', docRef.id);
      }
      navigate('/');
    } catch (error) {
      console.error('Error adding question: ', error);
    }
  };

  const addNewQuestion = () => {
    newQuestions = [...newQuestions, { text: '', options: [], correctAnswer: '' }];
  };
</script>

<main class="p-4">
  <h2 class="text-2xl font-bold mb-4">Add Quiz Questions</h2>

  <form>
    {#each newQuestions as question, i (question.text)}
      <div class="mb-4">
        <label for={`questionText${i}`} class="block text-sm font-medium text-gray-700">Question Text</label>
        <textarea
          id={`questionText${i}`}
          rows="3"
          class="mt-1 p-2 border rounded-md w-full"
          bind:value={question.text}
        ></textarea>
      </div>

      <div class="mb-4">
        <label for={`options${i}`} class="block text-sm font-medium text-gray-700">Options (comma-separated)</label>
        <input
          id={`options${i}`}
          type="text"
          class="mt-1 p-2 border rounded-md w-full"
          bind:value={question.options}
        />
      </div>

      <div class="mb-4">
        <label for={`correctAnswer${i}`} class="block text-sm font-medium text-gray-700">Correct Answer</label>
        <input
          id={`correctAnswer${i}`}
          type="text"
          class="mt-1 p-2 border rounded-md w-full"
          bind:value={question.correctAnswer}
        />
      </div>
    {/each}

    <button
      type="button"
      class="bg-green-500 text-white px-4 py-2 rounded"
      on:click={addNewQuestion}>
      Add Another Question
    </button>

    <button
      type="button"
      class="bg-green-500 text-white px-4 py-2 rounded mt-4"
      on:click={addQuestion}>
      Add Questions
    </button>
  </form>
</main>
