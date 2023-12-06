<script>
  let query = '';
  let searching = false;
  let results = null;
  let errorMessage = '';
  let currentPage = 1;
  let resultsPerPage = 10;

  const search = async () => {
    searching = true;
    errorMessage = '';
    results = null;
    console.log(`Searching: http://103.226.138.10/query/${query}`);
    try {
      const response = await fetch(`http://103.226.138.10/query/${query}`);
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      const data = await response.json();
      results = data;
      console.log(results);
    } catch (error) {
      console.error('Search failed:', error);
      errorMessage = error.message;
      results = [];
    } finally {
      searching = false;
    }
  };

  const setPage = (pageNum) => {
    currentPage = pageNum;
  };

  $: paginatedResults = results 
    ? results.slice((currentPage - 1) * resultsPerPage, currentPage * resultsPerPage)
    : [];

  $: totalPages = results ? Math.ceil(results.length / resultsPerPage) : 0;

  const goToPreviousPage = () => {
    if (currentPage > 1) {
      setPage(currentPage - 1);
    }
  };

  const goToNextPage = () => {
    if (currentPage < totalPages) {
      setPage(currentPage + 1);
    }
  };

  const getPDFLink = (item) => `http://103.226.138.10/get-pdf/${item.replace("corpus/", "")}`;

  const icons = [
    "https://web.archive.org/web/20091027092920/http://geocities.com/pdrhp/Yellow_Star.gif",
    "https://web.archive.org/web/20090829052505/http://www.geocities.com/sbspecialty/yellowflr.gif",
    "https://web.archive.org/web/20091027124753/http://geocities.com/syyen20/face_yellow.gif",
    "https://web.archive.org/web/20091026105516/http://geocities.com/TimesSquare/Castle/7044/gavel.gif"
  ];

  const getRandomIcon = () => icons[Math.floor(Math.random() * icons.length)];

</script>

<body>
	<div class="banner">
		<img src="https://cdn.discordapp.com/attachments/790575942451462144/1181915477405081650/image.png?ex=6582cb90&is=65705690&hm=defdd8654b52dca3736e29ea67f3661e7496db1be68dd5a599ab94eb99160bc7&" alt="Banner" class="banner-image">
	</div>
	<h2>Simple Law Search Engine 😊</h2>
	<div class="search-container">
		<input type="text" bind:value={query} placeholder="Enter your search term...">
		<button on:click={search}>Search</button>
	</div>
	{#if searching}
	<img src="https://web.archive.org/web/20090821070459/http://geocities.com/Area51/5989/images/loading.gif" alt="Loading..." class="loading-image">
	{/if}

{#if results && results.length > 0}
  <div class="results-container">
    {#each paginatedResults as result}
      <div class="result-item">
        <a href={getPDFLink(result)} target="_blank">
          {result}
        </a>
        <img class="result-icon" src={getRandomIcon()} alt="Icon">
      </div>
    {/each}
  </div>

  <div class="pagination">
    <button on:click={goToPreviousPage} disabled={currentPage === 1}>
      <img src="https://web.archive.org/web/20091027055525/http://hk.geocities.com/moonwindsky/images/previousyellow.gif" alt="Previous Page">
    </button>
    <span>Page {currentPage} of {totalPages}</span>
    <button on:click={goToNextPage} disabled={currentPage === totalPages}>
      <img src="https://web.archive.org/web/20091020095606/http://hk.geocities.com/moonwindsky/images/nextyellow.gif" alt="Next Page">
    </button>
  </div>
{/if}
</body>

<style>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  html {
    margin: 0;
    padding: 0;
    border: 0;
  }

  :global(body) {
    padding-top: 150px;
    font-family: 'Comic Sans MS', sans-serif;
    background-color: #FFC47E;
    color: #333;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;
    min-height: 100vh;
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  .banner {
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
  }

  .banner img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .search-container {
    text-align: center;
    padding: 20px;
  }

  input[type='text'] {
    padding: 10px;
    font-size: 18px;
    border: 2px solid #333;
    margin-right: 10px;
    width: 300px;
  }

  button {
    padding: 10px 20px;
    font-size: 18px;
    background-color: #333;
    color: #fff;
    border: none;
    cursor: pointer;
  }

  button:hover {
    background-color: #555;
  }

  h1 {
    font-family: 'Brush Script MT', sans-serif;
    color: red;
    padding: 10px 20px;
    font-size: 80px;
    border-radius: 5px;
  }

  .left-image {
    position: absolute;
    left: 0;
    top: 50%;
    transform: translateY(-50%);
    max-height: 100%;
  }

  .right-image {
    position: absolute;
    right: 0;
    top: 50%;
    transform: translateY(-50%);
    max-height: 100%;
  }

  .loading-image {
    display: block;
    margin: 20px auto;
  }

  .results-container {
    margin-top: 20px;
  }

  .result-item {
    padding: 10px;
    border: 1px solid #ccc;
    margin-bottom: 10px;
    display: flex;
    align-items: center;
  }

  .result-item a {
    margin-right: 10px;
    text-decoration: none;
    color: #333;
  }

  .result-item a:hover {
    text-decoration: underline;
  }

  .result-icon {
    width: 20px;
    height: 20px;
  }
</style>
