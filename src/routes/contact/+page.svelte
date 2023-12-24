<script>
  import { onMount } from 'svelte';
  import { addDoc, Timestamp } from 'firebase/firestore';
  import { fstore } from '../../firebase';

  let name = "";
  let email = "";
  let message = "";

  const submitContactForm = async () => {
    try {
      const docRef = await addDoc(collection(fstore, "contactForms"), {
        name,
        email,
        message,
        createdAt: Timestamp.fromDate(new Date())
      });
      console.log('Contact form submitted with ID: ', docRef.id);
      // You can add additional logic or redirection after successful submission
    } catch (err) {
      console.error('Error submitting contact form:', err);
    }
  };
</script>

<div class="text-2xl font-hind font-bold">Contact Form</div>
<div class="variant-glass p-5 rounded-md flex flex-col gap-2">
  <input class="input" type="text" placeholder="Your Name" bind:value={name} />
  <input class="input" type="email" placeholder="Your Email" bind:value={email} />
  <textarea class="input" placeholder="Your Message" bind:value={message}></textarea>
</div>

<button on:click={submitContactForm} class="mt-5 bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">Submit</button>
