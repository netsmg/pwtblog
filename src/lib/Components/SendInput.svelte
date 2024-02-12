<script>
    import {createEventDispatcher} from "svelte"
    import {goto} from '$app/navigation';
    import Icon from "@iconify/svelte";
    import { userStore } from "../../stores/userStore";
    export let commentText="";
    const dispatch = createEventDispatcher();
    const submit = () => {
        dispatch('submit');
    }


</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<div  class="input-group relative input-group-divider grid-cols-[auto_1fr_auto] rounded-container-token">
    {#if $userStore.loggedIn == false}
    <div on:click={()=>goto('/auth')} class="font-poppins absolute h-full w-full variant-glass-error text-gray-100 font-bold flex">Sign In to comment</div>
    {/if}
    
    <button class="input-group-shim">
        {#if $userStore.photoURL}
        <img class="w-[30px] h-[30px] max-sm:w-[20px] max-sm:h-[20px] rounded-full" src={$userStore.photoURL} alt="Rounded avatar">
        {:else}
        +
        {/if}
    </button>
	<textarea
        bind:value={commentText}
		class="bg-transparent border-0 ring-0 commentInput"
		name="prompt"
		id="prompt"
		placeholder="Write a comment..."
		rows="1"
	/>
	<button on:click={submit} class="variant-soft-primary text-[25px]"><Icon icon="ri:send-plane-fill" /></button>
</div>
					

<style>
    .commentInput {
        font-family: 'Lato', Kalpurush;
    }
    /* Style for the input group */
.input-group {
  display: grid;
  grid-template-columns: auto 1fr auto;
  align-items: center;
  gap: 10px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

/* Style for the button */
.input-group button {
  padding: 5px;
  border: none;
  background-color: transparent;
  cursor: pointer;
}

/* Style for the textarea */
.input-group textarea {
  flex: 1;
  padding: 10px;
  border: none;
  background-color: transparent;
  font-family: 'Lato', Kalpurush;
  resize: none;
  outline: none;
}

/* Style for the sign-in message */
.input-group .sign-in-message {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(255, 0, 0, 0.5);
  color: #fff;
  font-weight: bold;
}
</style>
