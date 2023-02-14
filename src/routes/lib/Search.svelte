<script>
  import { createEventDispatcher, onMount } from "svelte";
  let city;
  let county;
  let state;

  async function search() {
    let data = {
      City: city,
      County: county,
      State: state,
    };
    let response = await fetch("https://internships.herokuapp.com/jobs/", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(data),
    });
    let searchdata = await response.json();
    console.log(searchdata);

    dispatchsearchclicked(searchdata);
  }

  const dispatch = createEventDispatcher();
  function dispatchsearchclicked(searchdata) {
    dispatch("eventsearchclicked", { searchdata: searchdata });
    console.log("dispa");
  }
</script>

<div class="container-fluid">
  <form
    class="d-flex"
    style="margin:2em; margin-left: 10em; margin-right: 10em;"
    on:submit|preventDefault={search}
  >
    <input
      class="form-control me-2"
      type="text"
      id="city"
      bind:value={city}
      placeholder="City"
    />
    <input
      class="form-control me-2"
      type="text"
      id="county"
      bind:value={county}
      placeholder="County"
    />
    <input
      class="form-control me-2"
      type="text"
      id="state"
      bind:value={state}
      placeholder="State"
    />
    <button class="btn btn-primary" type="submit">Find Jobs</button>
  </form>
</div>
