<script>
  import { onMount } from 'svelte';
  import { getDatabase, ref, onValue } from 'firebase/database';

  let exams = [];

  onMount(() => {
    // Initialize Firebase database
    const db = getDatabase();

    // Reference to 'jachai/exams/practice'
    const examsRef = ref(db, 'exams');

    // Fetch exams from Firebase
    onValue(examsRef, (snapshot) => {
      exams = snapshot.val();
    });
  });
</script>

<style>
:root {
  --blue: #007bff;
  --indigo: #6610f2;
  --purple: #6f42c1;
  --pink: #e83e8c;
  --red: #dc3545;
  --orange: #fd7e14;
  --yellow: #ffc107;
  --green: #28a745;
  --teal: #20c997;
  --cyan: #17a2b8;
  --white: #fff;
  --gray: #6c757d;
  --gray-dark: #343a40;
  --primary: #007bff;
  --secondary: #6c757d;
  --success: #28a745;
  --info: #17a2b8;
  --warning: #ffc107;
  --danger: #dc3545;
  --light: #f8f9fa;
  --dark: #343a40;
  --breakpoint-xs: 0;
  --breakpoint-sm: 576px;
  --breakpoint-md: 768px;
  --breakpoint-lg: 992px;
  --breakpoint-xl: 1200px;
  --font-family-sans-serif: -apple-system, BlinkMacSystemFont, "Segoe UI",
    Roboto, "Helvetica Neue", Arial, "Noto Sans", sans-serif,
    "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji";
  --font-family-monospace: SFMono-Regular, Menlo, Monaco, Consolas,
    "Liberation Mono", "Courier New", monospace;
}

.exam-title{
    font-size: 19px;
}

.exam-details{
    text-align: center;
    font-family: Google Sans, 'SiyamRupali';
}

.score{
    margin-top: 10px;
    display: flex;
    justify-content: space-around;
    font-size: 17px;
}

.mark, .score-wa, .score-ng, .score-na{
    background: #fff;
}

.mark{
    color: var(--success);
    padding: 5px;
    box-shadow: 0 2px 5px rgba(0,0,0, .2);
    border-top: 3px solid var(--success);
    border-bottom-left-radius: 5px;
    border-bottom-right-radius: 5px;
}

.score-wa{
    color: var(--danger);
    padding: 5px;
    box-shadow: 0 2px 5px rgba(0,0,0, .2);
    border-top: 3px solid var(--danger);
    border-bottom-left-radius: 5px;
    border-bottom-right-radius: 5px;
}


.score-na{
    color: var(--teal);
    padding: 5px;
    box-shadow: 0 2px 5px rgba(0,0,0, .2);
    border-top: 3px solid var(--teal);
    border-bottom-left-radius: 5px;
    border-bottom-right-radius: 5px;
}

.score-ng{
    color: var(--warning);
    padding: 5px;
    box-shadow: 0 2px 5px rgba(0,0,0, .2);
    border-top: 3px solid var(--warning);
    border-bottom-left-radius: 5px;
    border-bottom-right-radius: 5px;
}

.questions{
    margin-top: 20px;
}

.fixToTop{
    position: fixed;
    top: 0%;
    left: 0;
    font-size: 19px;
    box-shadow: 0 2px 4px rgba(0,0,0, .2);
    transition: 0.2s;
}

.exam{
display: flex;
align-items: center;
justify-content: space-evenly;
padding: 10px;
box-shadow: 0 2px 5px rgba(0,0,0, .2);
margin: 10px 5px;
text-align: center;
border-radius: 5px;
position: relative;
background: #fff;
/* width: 100%; */
}

/* 
.exam .bg{
    position: absolute;
    background: var(--danger);
    width: 60px;
    height: 100%;
    left:  0;
    z-index: 1;
    border-top-left-radius: 5px;
    border-bottom-left-radius: 5px;
    border-bottom-right-radius: 45px;
    border-top-right-radius: 45px;

} */

.exam .logo{
 height: 50px;
 width: 50px;
 display: flex;
 justify-content: center;
 align-items: center;
 font-size: 25px;
 border-radius: 50%;
 border: 2px solid var(--teal);
 color: #fff;
 background: var(--teal);
}

.exam .title{
  font-size: 17px;
  font-weight: bold;
  color: #000;
}

.exam .others{
  font-size: 13px;
  color: #000;
}
.details{
    text-align: center;
    font-family: Google Sans, 'SiyamRupali';
}

.modal{
    min-height: 100% !important;
    width: 100%;
    top: 0 !important;
   
    /* overflow: hidden; */
    /* position: relative; */

}

.modal .modal-content {
    padding: 10px;
    overflow-y: scroll;
    scroll-behavior: smooth;
}





.exam-container{
    margin-top: 40px;
}

.exam-title{
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    background: var(--teal);
    z-index: 98;
    text-align: center;
    box-shadow: 0 2px 8px rgba(0,0,0, .2);
    padding: 10px;
    color: #fff;

}

  
</style>

<main>
  {#if exams && exams.length > 0}
    {#each exams as exam (exam.key)}
      <div class="exam">
        <div class="logo">{exam.details.name[0]}</div>
        <div class="details">
          <div class="title">{exam.details.name}</div>
          <div class="others">
            প্রশ্নঃ {exam.questions.length} টি | সময়ঃ {exam.details.duration} মিনিট | স্কোরঃ {exam.questions.length} | নেগেটিভঃ {exam.details.negative}
          </div>
        </div>
      </div>
    {/each}
  {:else}
    <p>No exams available</p>
  {/if}
</main>
