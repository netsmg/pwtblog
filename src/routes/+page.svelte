<script>
  import { onDestroy, onMount } from "svelte";
  import PostList from "../lib/Components/PostList.svelte";
  import { common } from "../stores/postStore";
  import PostSkeleton from "../lib/Components/PostSkeleton.svelte";
  import Landing from "../lib/Components/Landing.svelte";
import Search from "../lib/Components/Search.svelte";
  import venus from '../images/venus.svg';
  // import bottom from '../images/alien_cute.png';
  import SectionHead from "../lib/Components/SectionHead.svelte";
  import {
    collection,
    query,
    orderBy,
    startAt,
    endAt,
    startAfter,
    endBefore,
    limit,
    onSnapshot,
    updateDoc,
    doc,
    increment,

    getDocs

  } from "firebase/firestore";
  import { fstore } from "../firebase";



  let posts = [];
  let lastVisible;
  let lim = 20;
  let postCount = 0;

  const makeData = (docS) => {
    docS.forEach(doc=>{
      posts.push({
          ...doc.data(),
          mins: $common.getReadTime(doc.data().content),
          createdAt: $common.getDate(doc.data().createdAt),
          id: doc.id
        });
    })

      posts = posts;
}

  const  nextData = async(lastVisible, lim) => {
      posts = [];
    const q = query(
      collection(fstore, "posts"),
      orderBy("createdAt", "desc"),
      startAfter(lastVisible),
      limit(lim)
    );

    postCount++;

    const docS = await getDocs(q);
    lastVisible = docS.docs[docS.docs.length-1];
    makeData(docS);

      setTimeout(()=>{
        MathJax.typeset();
      }, 500);
  }

  const  prevData = async(lastVisible, lim) => {
      posts = [];
    const q = query(
      collection(fstore, "posts"),
      orderBy("createdAt", "desc"),
      endAt(lastVisible),
      limit(lim)
    );

    postCount--;

    const docS = await getDocs(q);
    lastVisible = docS.docs[docS.docs.length-1];
    makeData(docS);

      setTimeout(()=>{
        MathJax.typeset();
      }, 500);
  }



  onMount(async ()=>{
    document.title = "PWTBLOG | Home";
    document.description = "An authentic blog to share your code";
    posts = [];
    const q = query(
      collection(fstore, "posts"),
      orderBy("createdAt", "desc"),
      limit(lim)
    );

    const docS = await getDocs(q);
    lastVisible = docS.docs[docS.docs.length-1];
    makeData(docS);

      setTimeout(()=>{
        MathJax.typeset();
      }, 500);
  });

  const readHandle = async(id, read) => {
    const postRef = doc(fstore, "posts", id);
    try{
      const res = await updateDoc(postRef, {
        read: increment(1)
      })
      // console.log(res);
    } catch(err){
      console.log(err);
    }
  }

  const next = () => {
      if(posts.length>=10){
        nextData(lastVisible, lim);
      }
  }

  const prev = () => {
      prevData(lastVisible, lim);
  }
 
</script>

<Landing/>
<Search/>

<div class="absolute z-[-1] lg:left-[0] lg:top-[10px] top-[50px] md:top-10 md:left-[250px]">
  <img class="lg:animate-pulse max-sm:max-h-[300px] lg:h-[350px] md:h-[200px]" src={venus} alt=""/>
</div>


{#if posts.length > 0}
<SectionHead title="Most Recent"/>
  {#each posts as post, index}
    <a href="/blog/{post.id}">
      <PostList
      on:click={()=> readHandle(post.id, post.read)}
        read={post.read}
        commentCount = {post.comment}
        author={post.author}
        title={post.title}
        express={post.express}
        tags={post.tags}
        createdAt={post.createdAt}
        mins={post.mins}
        avatar="https://codebuckblog.vercel.app/_next/image?url=%2F_next%2Fstatic%2Fmedia%2FLogo.e0b9e0a0.png&w=128&q=75"
      />
    </a>
  {/each}
  <div class="flex items-center justify-center gap-5">
    {#if postCount!=0}
    <button on:click={()=> prev()} class="btn variant-filled-primary">Previous</button>
    {/if}

    {#if posts.length>9}
    <button on:click={()=> next()} class="btn variant-filled-primary">Next</button>
    {/if}
  </div>
{:else}
  <PostSkeleton />
  <PostSkeleton />
  <PostSkeleton />
  <PostSkeleton />
{/if}
