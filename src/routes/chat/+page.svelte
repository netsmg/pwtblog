<script>
  import { onMount } from 'svelte';
  import firebase from 'firebase/app';
  import 'firebase/firestore';
  

  const db = firebase.firestore();
  const messages = [];

  // Fetch messages from Firestore
  const fetchMessages = async () => {
    const snapshot = await db.collection('messages').get();
    snapshot.forEach((doc) => {
      messages.push(doc.data());
    });
  };

  onMount(() => {
    fetchMessages();

    // Set up Firestore listener for real-time updates
    const unsubscribe = db.collection('messages').onSnapshot((snapshot) => {
      snapshot.docChanges().forEach((change) => {
        if (change.type === 'added') {
          messages.push(change.doc.data());
        }
      });
    });

    return unsubscribe; // Cleanup listener on component destroy
  });

  // Function to send a new message
  const sendMessage = (text) => {
    db.collection('messages').add({
      text,
      timestamp: firebase.firestore.FieldValue.serverTimestamp(),
    });
  };
</script>

<main>
  {#each messages as message (message.id)}
    <div>{message.text}</div>
  {/each}

  <textarea bind:value={newMessage} />
  <button on:click={() => sendMessage(newMessage)}>Send</button>
</main>
