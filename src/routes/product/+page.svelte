<script defer>
  import { onMount } from 'svelte';

  let searchValue = '';
  let fetchPhotosUrl;
  let fetchVideosUrl;
  let fetchPopularPhotosUrl;
  let page = 1;
  let gallery = [];
  let auth_key = '563492ad6f917000010000013457deb42ca2403a9074ac93b29156e6'; // Replace with your actual authorization key

  function updateInput(event) {
    searchValue = event.target.value;
  }

  function clear() {
    gallery = [];
    searchValue = '';
    console.log('Gallery Data Cleared');
  }
const galleryImages = document.querySelectorAll('.photoContainer img');
const lightbox = document.getElementById('lightbox');

if (galleryImages.length > 0 && lightbox) {
    galleryImages.forEach((image) => {
        image.addEventListener('click', (e) => {
            lightbox.classList.add('active');
            const img = document.createElement('img');
            img.src = image.src;
            while (lightbox.firstChild) {
                lightbox.removeChild(lightbox.firstChild);
            }
            lightbox.appendChild(img);
        });
    });
}

  async function fetchApi(url) {
    const response = await fetch(url, {
      method: 'GET',
      headers: {
        Accept: 'application/json',
        Authorization: auth_key,
      },
    });
    const data = await response.json();
    return data;
  }

  function generatePhotos(data) {
    gallery = [...gallery, ...data.photos.map(photo => ({
      photographer: photo.photographer,
      downloadLink: photo.src.original,
      imageSrc: photo.src.large,
    }))];
  }

  function generateVideos(data) {
  data.videos.forEach((video) => {
		video.video_files.forEach((video_file) => {
			//=> Create Video Container:
			const videoContainer = document.createElement('div');
			videoContainer.classList.add('videoContainer');

			//=> Conditions:
			if (
				video_file.quality === 'hd' &&
				video_file.width === 1280 &&
				video_file.height === 720
			) {
				videoContainer.innerHTML = `
				<div class="gallery-info">
				<p>${video_file.quality}</p>
				<a  href = ${video_file.link} target = '_blank'> <i class="fas fa-download"></i> </a>
				</div>
				<video muted src = ${video_file.link}></video>
				`;
			} else if (
				(video_file.quality === 'sd' || video_file.width === 1920, 960, 640)
			) {
				videoContainer.style.display = 'none';
			}

			//=> Append to Videos Gallery:
			gallery.appendChild(videoContainer);
		});
	});

	//==> Preview and Play Videos:
	const videos = gallery.querySelectorAll('video');

	videos.forEach((video) => {
		video.addEventListener('mouseover', function () {
			this.play();
		});

		video.addEventListener('mouseout', function () {
			this.pause();
		});
	});
    
  }

  function imageLightbox() {
    const lightbox = document.createElement('div');
	lightbox.id = 'lightbox';
	document.body.appendChild(lightbox);

	const galleryImages = document.querySelectorAll('.photoContainer img');
	galleryImages.forEach((image) => {
		image.addEventListener('click', (e) => {
			lightbox.classList.add('active');

			const img = document.createElement('img');
			img.src = image.src;
			while (lightbox.firstChild) {
				lightbox.removeChild(lightbox.firstChild);
			}
			lightbox.appendChild(img);
		});
	});

	lightbox.addEventListener('click', (e) => {
		if (e.target !== e.currentTarget) {
			return lightbox.classList.remove('active');
		}
	});
  }

  async function curatedPhotos() {
    fetchPhotosUrl = `https://api.pexels.com/v1/curated?per_page=16&page=1`;
    const data = await fetchApi(fetchPhotosUrl);
    generatePhotos(data);
  }

  async function populerPhotos() {
    fetchPopularPhotosUrl = `https://api.pexels.com/v1/popular?per_page=16&page=1`;
    const data = await fetchApi(fetchPopularPhotosUrl);
    generatePhotos(data);
  }

  async function popularVideos() {
    fetchVideosUrl = `https://api.pexels.com/videos/popular?per_page=16&page=1`;
    const data = await fetchApi(fetchVideosUrl);
    generateVideos(data);
  }

  async function searchPhotos(query) {
    clear();
    fetchPhotosUrl = `https://api.pexels.com/v1/search?query=${query}&per_page=16&page=1`;
    const data = await fetchApi(fetchPhotosUrl);
    generatePhotos(data);
  }

  async function searchVideos(query) {
    clear();
    fetchVideosUrl = `https://api.pexels.com/videos/search?query=${query}&per_page=16&page=1`;
    const data = await fetchApi(fetchVideosUrl);
    generateVideos(data);
  }

  async function loadMorePhotos() {
    page++;
    fetchPhotosUrl = `https://api.pexels.com/v1/curated?per_page=16&page=${page}`;
    const data = await fetchApi(fetchPhotosUrl);
    generatePhotos(data);
  }

  async function loadMoreVideos() {
    page++;
    fetchVideosUrl = `https://api.pexels.com/videos/popular?per_page=16&page=${page}`;
    const data = await fetchApi(fetchVideosUrl);
    generateVideos(data);
  }

  async function loadMorePopularPhotos() {
    page++;
    fetchPopularPhotosUrl = `https://api.pexels.com/v1/popular?per_page=16&page=${page}`;
    const data = await fetchApi(fetchPopularPhotosUrl);
    generatePhotos(data);
  }

  function randomPhotosGenerate() {
    clear();
    if (window.location.pathname === '/product') {
      loadMorePhotos();
    } else {
      loadMorePopularPhotos();
    }
  }

  function randomVideosGenerate() {
    clear();
    loadMoreVideos();
  }

  onMount(() => {
    popularVideos();
    curatedPhotos();
  });
</script>

<style>
    @import url(https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap);@import url(https://fonts.googleapis.com/css2?family=Righteous&display=swap);@import url(https://fonts.googleapis.com/css2?family=Poiret+One&display=swap);@import url(https://fonts.googleapis.com/css2?family=Raleway:ital,wght@0,500;0,600;0,700;0,800;0,900;1,400;1,500;1,600;1,700;1,800&display=swap);@import url(https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap);@import url(https://fonts.googleapis.com/css2?family=Big+Shoulders+Text:wght@300;400;500;600;700;800;900&display=swap);body .nav-Btn .moreBtn:hover,body main .title .random-generateBtn:hover{color:#9bff3e;box-shadow:0 4px 6px -1px rgba(0,0,0,.1),0 2px 4px -2px rgba(0,0,0,.1)}body .nav-Btn .moreBtn:hover,body main .title .random-generateBtn:hover,footer .copy-rights:hover,footer .links a:hover{box-shadow:0 4px 6px -1px rgba(0,0,0,.1),0 2px 4px -2px rgba(0,0,0,.1)}*{padding:0;margin:0;box-sizing:border-box}body{font-family:Poppins,sans-serif;-webkit-user-select:none;-moz-user-select:none;-ms-user-select:none;user-select:none;background:radial-gradient(ellipse at bottom,#fff 25%,#f3efef 100%)}body header{min-height:45vh;display:flex;align-items:center;justify-content:center;flex-direction:column;background:url("/img/photosBg.jpg") 50% 50% no-repeat;-o-object-fit:cover;object-fit:cover}body header h2{padding:2rem}body header h2 #logo{color:#003046;text-decoration:none;font-size:3rem;font-weight:400;font-family:Righteous,sans-serif}body header .search-form{display:flex;margin:2rem;background:#fff;border-radius:25px;filter:drop-shadow(0 20px 13px rgba(0, 0, 0, .03)) drop-shadow(0 8px 5px rgba(0, 0, 0, .08))}body header .search-form #select,body header .search-form input,body header button{border:none;font-weight:500;font-family:Poppins,sans-serif}body header .search-form #select{display:flex;justify-content:space-between;align-items:center;font-size:1.2rem;color:#003046;width:2rem;cursor:pointer;position:relative}body header .search-form #select i{width:15px;margin-left:1rem;pointer-events:none}body header .search-form input{width:25vw;height:2.8rem;font-size:1rem;background:#fff;outline:0;padding-left:1rem;text-align:left}body header button{width:5rem;border-radius:0 25px 25px 0;padding:auto;cursor:pointer;color:#2b2b2b;background:#f7e2df;box-shadow:0 1px 2px 0 rgba(0,0,0,.05);font-size:1rem;transition:.3s}body header button:hover{background:#003046;color:#ebebeb;box-shadow:#0073a8 0 0 10px}body section{width:100%}body section .navbar{width:100%;height:5vh;display:flex;align-items:center;justify-content:center;margin:2.5rem 0 5rem}body main .gallery,body main .title,footer{width:90%;margin:auto}body section .navbar .category{margin:0 4rem;display:flex;align-items:center;justify-content:center;border-radius:1rem;transition:.3s ease-out}body section .navbar .category a{text-decoration:none;color:#003046;background:#f7e2df;padding:10px 15px;border-radius:1.5rem}body section .navbar .category a h2{font-size:1rem;font-weight:500}body section .navbar .category a.active{color:#eef6ff;background:#003046;padding:10px 15px;border-radius:1.5rem}body section .navbar .category a.active h2{color:#fbe0d3}body section .navbar .category:hover{transition:.3s ease-in}body section .navbar .category:hover a{transition:.3s ease-in;background:#003046;color:#fbe0d3;padding:10px 15px;border-radius:1.5rem}body main .title{display:flex;justify-content:space-between;align-items:center}body main .title h2{font-size:2rem;font-family:Roboto,sans-serif}body main .title .random-generateBtn{background:#f7e2df;color:#0e0e0e;padding:.5rem 1rem;filter:drop-shadow(0 1px 2px rgba(0, 0, 0, .1)) drop-shadow(0 1px 1px rgba(0, 0, 0, .06));border:none;border-radius:.5rem;font-size:1rem;font-weight:500;cursor:pointer;font-family:Poppins,sans-serif;transition:.3s}body main .title .random-generateBtn:hover{background:#0e0e0e}body main .gallery{display:grid;grid-template-columns:repeat(auto-fill,minmax(350px,1fr));padding:2rem 0;row-gap:5rem;-moz-column-gap:3rem;column-gap:3rem}body main .gallery .photoContainer,body main .gallery .videoContainer{position:relative;cursor:pointer;transition:.3s ease-in-out}body main .gallery .photoContainer .gallery-info,body main .gallery .videoContainer .gallery-info{width:100%;margin-bottom:.2rem;position:absolute;left:0;bottom:0;z-index:2;padding:.5rem 0;display:flex;align-items:center;justify-content:space-between;font-family:Poppins,sans-serif;font-size:1rem;font-weight:600;color:rgba(14,14,14,.6431372549);text-transform:uppercase}body main .gallery .photoContainer .gallery-info p,body main .gallery .videoContainer .gallery-info p{padding:.5rem;margin-left:.5rem;background:rgba(238,246,255,.6156862745);border-radius:1rem;box-shadow:0 1px 2px 0 rgba(0,0,0,.05);transition:.5s ease-in-out}body main .gallery .photoContainer .gallery-info p:hover,body main .gallery .videoContainer .gallery-info p:hover{background:#0e0e0e;color:#eef6ff}body main .gallery .photoContainer .gallery-info a,body main .gallery .videoContainer .gallery-info a{background:rgba(238,246,255,.6156862745);padding:.5rem;margin-right:.5rem;display:flex;align-items:center;justify-content:center;border-radius:1rem;box-shadow:0 1px 2px 0 rgba(0,0,0,.05);transition:.5s}body .nav-Btn,footer .links{align-items:center;display:flex}body main .gallery .photoContainer .gallery-info a i,body main .gallery .videoContainer .gallery-info a i{pointer-events:none;color:#0e0e0e}body main .gallery .photoContainer .gallery-info a:hover,body main .gallery .videoContainer .gallery-info a:hover{background:#0e0e0e}body main .gallery .photoContainer .gallery-info a:hover i,body main .gallery .videoContainer .gallery-info a:hover i{color:#eef6ff}body main .gallery .photoContainer img,body main .gallery .photoContainer video,body main .gallery .videoContainer img,body main .gallery .videoContainer video{position:relative;width:100%;height:100%;filter:drop-shadow(0 20px 13px rgba(0, 0, 0, .03)) drop-shadow(0 8px 5px rgba(0, 0, 0, .08));-o-object-fit:cover;object-fit:cover;border-radius:1rem}body main .gallery .photoContainer:hover,body main .gallery .videoContainer:hover{transition:.3s ease-in-out;transform:scale(1.1)}body main .videos-gallery{grid-template-columns:repeat(auto-fill,minmax(450px,1fr))}body .nav-Btn{min-height:30vh;justify-content:center}body .nav-Btn .moreBtn{background:#003046;color:#f7e2df;padding:.5rem 2rem;border:none;border-radius:.5rem;font-size:1rem;font-weight:500;cursor:pointer;font-family:Poppins,sans-serif;transition:.3s}footer{padding:1rem 0;display:flex;align-items:flex-end;justify-content:flex-end}footer .copy-rights{background:#eaf4ff;border-radius:1rem;font-family:Roboto,sans-serif;font-size:.7rem;padding:.5rem 1rem;transition:.3s}footer .copy-rights h2 a{text-decoration:none;color:#0e0e0e}footer .copy-rights h2 a span{color:#575aff}footer .copy-rights h2 a:hover{transition:.5s;color:#575aff}footer .copy-rights h2 a:hover span,footer .links a i{color:#0e0e0e}footer .links{width:4.5rem;height:2rem;margin:0 .5rem;justify-content:space-between;font-size:1.3rem;color:#0e0e0e;-webkit-user-select:none;-moz-user-select:none;-ms-user-select:none;user-select:none}footer .links a{height:2.1rem;width:2.1rem;display:flex;align-items:center;justify-content:center;text-decoration:none;background:#eaf4ff;padding:.5rem;border-radius:50%;transition:.3s}footer .links a:hover{background:#0e0e0e}footer .links a:hover i{color:#eaf4ff}@media screen and (max-width:660px){body header .search-form #select{width:2rem}body header .search-form input{width:65vw;padding-left:1rem}body header button{width:4.5rem}body section .navbar .category{margin:0 2rem}body main .title h2{font-size:1.2rem}body main .title .random-generateBtn{padding:.3rem .7rem}body main .gallery .photoContainer .gallery-info p,body main .gallery .videoContainer .gallery-info p{font-size:.5rem;padding:.3rem;margin-left:.5rem}body main .gallery,body main .videos-gallery{grid-template-columns:repeat(auto-fill,minmax(150px,1fr));padding:1rem 0;row-gap:2rem;-moz-column-gap:1rem;column-gap:1rem}footer{display:flex;align-items:center;justify-content:center}footer h2{font-size:.9rem}}#lightbox{justify-content:center;align-items:center;position:fixed;z-index:1000;top:0;height:100%;width:100%;background-color:rgba(0,0,0,.8);display:none;transition:.3s ease-in-out}#lightbox img{max-width:90%;max-height:80%;padding:6px;background-color:#000;border:2px solid #fff;border-radius:1rem;cursor:pointer}#lightbox.active{display:flex;justify-content:center;align-items:center}
  
</style>
<form class="search-form">
				

				
							</form>
		
		<section>
			<div class="navbar">
				<div class="home category">
					<a href="./product"><h2>Home</h2></a>
				</div>
				<div class="videos category">
					<a href="./product"><h2>Videos</h2></a>
				</div>
				<div class="photos category">
					<a href="./product"><h2>Photos</h2></a>
				</div>
			</div>
		</section>

		<main>
			
			<div class="title">
				<h2>Popular Stock Photos</h2>
				<button class="random-generateBtn randomPhotos">
					<i class="fas fa-light fa-retweet"></i>
				</button>
			</div>

			
			<div class="gallery"></div>
		</main>

		<div class="nav-Btn">
			<button class="morePhotos-Btn moreBtn popularMore">More</button>
		</div>

	

<button on:click={curatedPhotos} class="moreBtn">Load Curated Photos</button>
<button on:click={populerPhotos} class="random-generateBtn">Load Popular Photos</button>
<button on:click={loadMorePopularPhotos} class="moreBtn">Load More Popular Photos</button>
<button on:click={randomPhotosGenerate} class="random-generateBtn">Random Photos Generator</button>
 


