<!-- <h1>Welcome to SvelteKit</h1>
<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p> -->
<script>
  import JobFeed from "./lib/JobFeed.svelte";
  import JobSummary from "./lib/JobSummary.svelte";
  import Navbar from "./lib/Navbar.svelte";
  import Search from "./lib/Search.svelte";
  import { onMount } from "svelte";
  import JobPage from "./lib/JobPage.svelte";
  import JobCreate from "./lib/JobCreate.svelte";
  //   import JobFeed from "./JobFeed.svelte";
  //   import JobSummary from "./JobSummary.svelte";
  //   import Navbar from "./Navbar.svelte";
  //   import Search from "./Search.svelte";
  //   import { onMount } from "svelte";
  //   import JobPage from "./JobPage.svelte";
  //   import JobCreate from "./JobCreate.svelte";
  let jobdata = {};
  let searchdata = [];
  let switcher = false;

  function handleclick(event) {
    console.log(event.detail.jobdata);
    jobdata = event.detail.jobdata;
  }

  function handlesearch(event) {
    console.log(event.detail.searchdata);
    searchdata = event.detail.searchdata;

    switcher = true;
  }

  let pageselected = 0;
  function changepage1() {
    pageselected = 0;
  }

  function changepage2() {
    pageselected = 1;
    console.log("sanity check");
  }

  function changepage3() {
    pageselected = 2;
  }
</script>

<main>
  <!-- <Navbar/> -->
  <button on:click={changepage3}>Find Job</button>

  <button on:click={changepage1}>Create Job</button>

  <button on:click={changepage2}>Login</button>

  <Search on:eventsearchclicked={handlesearch} />
  <div class="container-fluid">
    <div class="row" style="margin:auto">
      {#if pageselected === 0}
        <JobCreate />
      {:else if pageselected === 1}
        <JobPage />
      {:else if pageselected === 2}
        <JobFeed on:eventjobclicked={handleclick} {searchdata} bind:switcher />
        <JobSummary {jobdata} />
      {/if}
    </div>
  </div>
</main>

<style>
</style>
