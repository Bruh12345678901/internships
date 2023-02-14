<script>
  import { beforeUpdate, createEventDispatcher, onMount } from "svelte";

  async function getemployer(id) {
    // let employer = await fetch("http://127.0.0.1:3000/employers/" + id);
    let employer = await fetch(
      "https://internships.herokuapp.com/employers/" + id
    );
    let employerdata = await employer.json();

    return employerdata;
  }

  let name = "";
  let employer = "";
  let location = "";
  let pay = "";
  let hours = "";
  let qualifications = [];
  let data;
  export let jobdata;
  beforeUpdate(async function () {
    let response = await fetch(
      "https://internships.herokuapp.com/employers/" + jobdata["employer"],
      {
        method: "GET",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(data),
      }
    );

    let employerdata2 = await response.json();

    // const jobdata = await getfirstjob()
    console.log(jobdata);
    const employerdata = await getemployer(await jobdata.employer);

    name = jobdata["Name"];
    employer = employerdata2["Name"];
    location = jobdata["County"] + ", " + jobdata["City"];
    pay = "$" + jobdata["Pay"] + " per hour";
    // hours = jobdata["Hours"]
    qualifications = jobdata["Qualifications"];
  });

  const dispatch = createEventDispatcher();
  function dispatchjobclicked() {
    dispatch("eventjobclicked", { jobdata: jobdata });
    console.log("dispa");
  }

  // let name = "Gaming Gamer - Hiring NOW"
  // let employer = "Gamers LLC"
  // let location = "Irvine, CA"
  // let pay = "$150-$5000 an hour"
  // let hours = "Part-Time"
  // let qualifications = ["Must be gaming", "Must be a gamer", "Ability to game"]
</script>

<!-- <button class="btn btn-default row" onclick="job()"> -->

<button class="btn btn-default row" on:click={dispatchjobclicked}>
  <div style="display: flex; flex-direction: column; align-items: center;">
    <h4>{name}</h4>
    <p style="line-height: 1em;">{employer}</p>
    <p style="line-height: 0em;">{location}</p>
    <h6>{pay} {hours}</h6>
    <!-- <ol style="width: fit-content">
            {#each qualifications as qualification}
                <li style="text-align: left;">{qualification}</li>
            {/each}
        </ol> -->
  </div>
</button>
