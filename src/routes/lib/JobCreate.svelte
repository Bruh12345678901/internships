<script>
  import { onMount } from "svelte";

  let id;
  let email;
  let link;
  let phone;
  let county;
  let city;
  let state;

  let jobname;
  let jobdesc;
  let jobtime;
  let jobpay;
  let jobtimepost;

  let invalid = "";
  let employid;

  async function login() {
    let data = {
      LoginID: id,
    };
    console.log(id);
    // let response = await fetch("http://127.0.0.1:3000/employers/login", {
    let response = await fetch(
      "https://internships.herokuapp.com/employers/login",
      {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(data),
      }
    );
    console.log(response.body);
    let employer = await response.json();
    if ("Login" in employer) {
      invalid = "Invalid ID";
      console.log("bru");
    } else {
      let form = document.getElementById("form");
      let elements = form.getElementsByTagName("*");
      for (let i = 0; i < form.children.length; i++) {
        form.children[i].removeAttribute("disabled");
      }
    }
    console.log(employer);
    email = employer.Email;
    link = employer.Link;
    phone = employer.Phone;
    county = employer.County;
    city = employer.City;
    state = employer.State;
    employid = employer.id;
  }

  async function job() {
    let data = {
      County: county,
      City: city,
      State: state,
      Email: email,
      Phone: phone,
      Link: link,
      LoginID: id,
      Name: jobname,
      Description: jobdesc,
      Time: jobtime,
      Pay: jobpay,
      TimePost: jobtimepost,
    };

    let response = await fetch(
      "https://internships.herokuapp.com/employers/" + employid + "/jobs",
      {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(data),
      }
    );
    console.log(response);
  }

  onMount(async function () {
    let form = document.getElementById("form");
    let elements = form.getElementsByTagName("*");
    for (let i = 0; i < form.children.length; i++) {
      form.children[i].setAttribute("disabled", "");
    }
  });
</script>

<form on:submit|preventDefault={login}>
  <label for="id">Employer ID: </label>
  <input id="id" bind:value={id} />
  <p id="invalid">{invalid}</p>

  <button type="submit">Submit</button>
</form>

<form id="form" on:submit|preventDefault={job}>
  <h6>Location</h6>

  <label for="county">County</label>
  <input id="county" bind:value={county} />
  <label for="city">City</label>
  <input id="city" bind:value={city} />
  <label for="state">State</label>
  <input id="state" bind:value={state} />

  <h6>Contact</h6>

  <label for="email">Email</label>
  <input id="email" bind:value={email} />
  <label for="phone">Phone</label>
  <input id="phone" type="number" bind:value={phone} />
  <label for="link">Link</label>
  <input id="link" type="url" bind:value={link} />

  <h6>Job Information</h6>

  <label for="name">Job Name</label>
  <input id="name" bind:value={jobname} />

  <label for="desc">Job Description</label>
  <textarea id="desc" bind:value={jobdesc} />

  <label>
    <input type="radio" bind:value={jobtime} />
    Part - Time
  </label>
  <label>
    <input type="radio" bind:value={jobtime} />
    Full - Time
  </label>

  <label for="pay">Hourly Wage</label>
  <input id="pay" type="number" bind:value={jobpay} />

  <label for="timepost">Date For Job Posted</label>
  <input id="timepost" type="date" bind:value={jobtimepost} />

  <button type="submit">Submit</button>
</form>
