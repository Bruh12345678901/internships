<script>
  import { onMount } from "svelte";
  import { beforeUpdate, afterUpdate } from "svelte";

  async function getemployer(id) {
    // let employer = await fetch("http://127.0.0.1:3000/employers/" + id);
    let employer = await fetch(
      "https://internships.herokuapp.com/employers/" + id
    );
    let employerdata = await employer.json();

    return employerdata;
  }

  // // let name = ""
  // let employer = ""
  // let location = ""
  // let pay = ""
  // let hours = ""
  // let qualifications = []
  export let jobdata;
  let employerdata = {};
  beforeUpdate(async function () {
    if ("Name" in jobdata) {
      //Check if the current job is the same as the new job
      employerdata = await getemployer(await jobdata.employer);
    }
    console.log("spam");
  });

  // onMount(async function () {
  //     // const jobdata = await getfirstjob()
  //     console.log(jobdata)
  //     // const employerdata = await getemployer(await jobdata.employer)

  //     name = jobdata["Name"]
  //     // employer = employerdata["Name"]
  //     // location = employerdata["Location"]
  //     // pay = jobdata["Pay"]
  //     // hours = jobdata["Hours"]
  //     // qualifications = jobdata["Qualifications"]
  // })

  // export function updatejob(jobd) {
  //     console.log(jobd)
  // }
</script>

<div class="col" style="border-width: 1px;border-style: solid;">
  <div class="row">
    <h5 style="text-align: center;">Job Summary</h5>
  </div>
  <div class="row">
    <div id="desc">
      {#if "Name" in jobdata && "Name" in employerdata}
        <h4>{jobdata.Name}</h4>
        <button class="btn btn-primary">{employerdata.Name}</button>
        <p style="line-height: 1em;">{jobdata.County}, {jobdata.City}</p>
        <p style="line-height: 0.7em;">{jobdata.TimePost}</p>
        <div style="border-style:solid;border-width:1px;">
          <h6>Job Details</h6>
          <p>Salary :</p>
          <p style="line-height: 0.7em;">${jobdata.Pay} per hour</p>
          <!-- <p>Job Type : </p>
                    <p style="line-height: 0.7em;">{jobdata.Hours}</p> -->
        </div>
        <div style="border-style:solid;border-width:1px;">
          <h6>Description</h6>
          <p style="line-height: 0.7em;">{jobdata.Description}</p>
          <!-- <ol>
                        {#each jobdata.Qualifications as qualification}
                            <li style="text-align: left;">{qualification}</li>
                        {/each}
                    </ol> -->
        </div>
        <div>
          <a href="jobpage.svelte">Click Here For More Information</a>
        </div>
      {/if}
    </div>
  </div>
</div>
