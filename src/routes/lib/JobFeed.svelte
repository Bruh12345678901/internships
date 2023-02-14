<script>
  import { onMount } from "svelte";
  import Job from "./Job.svelte";
  import { beforeUpdate, afterUpdate } from "svelte";
  import { paginate, LightPaginationNav } from "svelte-paginate";
  import Pagination from "./Pagination.svelte";

  export let searchdata;
  export let switcher;
  async function getfirstjob() {
    // let job = await fetch("http://127.0.0.1:3000/jobs");
    let job = await fetch("https://internships.herokuapp.com/jobs");
    let data = await job.json();

    return data.slice(0, 10);
  }
  let values = [];
  // let data2 = {}
  let firstime = true;
  let data2;
  beforeUpdate(function () {
    if (searchdata.length !== 0 && switcher === true) {
      console.log(searchdata);
      data2 = searchdata;
      switcher = false;
    } else if (firstime === true) {
      console.log("dat2");
      data2 = getfirstjob();
      firstime = false;
    }
    console.log(searchdata);
  });

  // let items = []
  // let currentPage = 1
  // let pageSize = 10
  // $: paginatedItems = paginate({ data2, pageSize, currentPage })
</script>

<div
  class="col"
  style="border-width: 1px;border-style: solid;display: flex; flex-direction: column; align-items: center;"
  id="jobfeed"
>
  <div class="row">
    <h5 style="text-align: center;">Job Feed</h5>
  </div>
  {#await data2}
    <p>loading. . .</p>
  {:then data2}
    {#if values}
      {#each values as data}
        <Job on:eventjobclicked jobdata={data} />
      {/each}
    {/if}
    <Pagination rows={data2} perPage={3} bind:trimmedRows={values} />
  {/await}
</div>
