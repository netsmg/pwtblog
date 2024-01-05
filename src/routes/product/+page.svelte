<script>
  import { onMount } from "svelte";
  import { collection, getDocs, query, orderBy } from "firebase/firestore";
  import { getFirestore } from 'firebase/firestore';

  let posts = [];
  const db = getFirestore();

  import Icon from '@iconify/svelte';

  const q = query(collection(db, 'posts'), orderBy('createdAt'));
  const querySnapshot = await getDocs(q);
  
  querySnapshot.forEach((doc) => {
    const post = { id: doc.id, ...doc.data() };
    posts.push(post);
  });
</script>

{#each posts as post (post.id)}
  <div on:click class="font-hind p-2 text-black dark:text-gray-400 variant-glass rounded">
    <div class="text-xl font-bold">{post.title}</div>
    <div class="text-md exp">{@html post.express}...</div>
    <div class="flex space-x-1 items-center">
      <img
        class="w-5 h-5 max-sm:w-3 max-sm:h-3 rounded-full"
        src={avatar}
        alt="Rounded avatar"
      />
      
      <div class="text-sm font-bold max-sm:text-[10px]">{post.author}</div>
      <Icon icon="bi:dot" />
      <div class="font-lato flex read gap-1 max-sm:text-[11px]">
        <Icon icon="ic:baseline-remove-red-eye" />{post.read}
        <Icon icon="bi:dot" />
        <Icon icon="cil:comment-bubble" /> {post.commentCount}
      </div>
    </div>
    <div class="flex flex-wrap gap-2 mt-2 items-center text-gray-500 font-poppins text-sm max-sm:text-[10px]">
      <div>{post.createdAt}</div>
      · {post.mins} min read ·
      {#each post.tags as tag, index}
        {#if index < 3}
          <code>{tag}</code>
        {/if}
      {/each}
    </div>
  </div>
  <hr />
{/each}

<style>
  .exp {
    font-family: 'Lato', Kalpurush;
  }
  .read {
    align-items: center;
  }
</style>
