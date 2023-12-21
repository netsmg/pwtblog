<script>
    import { onDestroy, onMount } from "svelte";
    import PostList from "../../lib/Components/PostList.svelte";
    import { postStore } from "../../stores/postStore";
    import PostSkeleton from "../../lib/Components/PostSkeleton.svelte";
    import Landing from "../../lib/Components/Landing.svelte";
    import venus from '../../images/venus.svg';
    import bottom from '../../images/alien_cute.png';
    import SectionHead from "../../lib/Components/SectionHead.svelte";
    let posts = [];
    let unsubscribe = postStore.subscribe(async (data) => {
      posts = data;
    });
  
    onDestroy(unsubscribe);
  </script>
  
  <Landing/>
  
  <div class="absolute z-[-1] lg:left-[0] lg:top-[10px] top-[50px] md:top-10 md:left-[250px]">
    <img class="lg:animate-pulse max-sm:max-h-[300px] lg:h-[350px] md:h-[200px]" src={venus} alt=""/>
  </div>
  
  
  {#if posts.length > 0}
  <SectionHead title="All Posts"/>
    {#each posts as post, index}
      <a href="/blog/{index}">
        <PostList
          author={post.author}
          title={post.title}
          express={post.express}
          tags={post.tags}
          createdAt={post.createdAt}
          mins={post.mins}
          avatar="https://codebuckblog.vercel.app/_next/image?url=%2F_next%2Fstatic%2Fmedia%2FLogo.e0b9e0a0.png&w=384&q=75"
        />
      </a>
    {/each}
  {:else}
    <PostSkeleton />
    <PostSkeleton />
    <PostSkeleton />
    <PostSkeleton />
  {/if}
  
