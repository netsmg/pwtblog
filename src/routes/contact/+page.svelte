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

    // Clear the form after submission
    formData = {
      name: "",
      email: "",
      message: "",
    };

    // Alert after submission
    alert("Form submit successful!");

    console.log("Form submitted to Firebase Realtime Database!");
  };
onMount(()=>{
        document.title = "Contact";
    })
</script>

<style>
  /* Add your Tailwind styles here */
</style>
<div class="text-2xl font-hind font-bold">Contact Form</div>
<div class="variant-glass p-5 rounded-md flex flex-col gap-2">
    <form on:submit={handleSubmit}>
  <label for="name">Name:</label>
  <input type="text" class="input" id="name" bind:value={formData.name} />

  <label for="email">Email:</label>
  <input type="email" id="email" class="input" bind:value={formData.email} />

  <label for="message">Message:</label>
  <textarea class="input" id="message" rows="4" bind:value={formData.message}></textarea>

  <button type="submit" class="mt-5 bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">Submit</button>
</form>

  
</div>


