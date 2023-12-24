<script>
  import { onMount } from "svelte";
  import { getDatabase, ref, push } from "firebase/database";

  let formData = {
    name: "",
    email: "",
    message: "",
  };

  const handleSubmit = async () => {
    const db = getDatabase();
    const contactsRef = ref(db, "contacts");

    // Push the form data to the "contacts" collection
    await push(contactsRef, formData);

    // Optional: Clear the form after submission
    formData = {
      name: "",
      email: "",
      message: "",
    };

    console.log("Form submitted to Firebase Realtime Database!");
  };
</script>

<style>
  /* Add your Tailwind styles here */
</style>

<form on:submit={handleSubmit}>
  <label for="name">Name:</label>
  <input type="text" id="name" bind:value={formData.name} />

  <label for="email">Email:</label>
  <input type="email" id="email" bind:value={formData.email} />

  <label for="message">Message:</label>
  <textarea id="message" rows="4" bind:value={formData.message}></textarea>

  <button type="submit">Submit</button>
</form>
