<script>
  import { v4 as uuidv4 } from "uuid";
  import { onMount } from "svelte";
  let name;
  let county;
  let city;
  let state;
  let email;
  let phone;
  let link;
  let id = "";

  async function postemployer() {
    id = uuidv4();
    let data = {
      Name: name,
      County: county,
      City: city,
      State: state,
      Email: email,
      Phone: phone,
      Link: link,
      LoginID: id,
    };

    let response = await fetch("https://internships.herokuapp.com/employers", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(data),
    });

    console.log(response);
  }
  onMount(async function () {
    let form = document.getElementById("form");
    let elements = form.getElementsByTagName("*");
    for (let i = 0; i < form.children.length; i++) {
      form.children[i].setAttribute("required", "");
    }
  });
</script>

<form id="form" on:submit|preventDefault={postemployer}>
  <label for="name">First & Last Name</label>
  <input id="name" bind:value={name} />

  <label>Organization Name</label>
  <input />

  <br />
  <br />
  <br />
  <br />

  <h6>Location</h6>

  <label for="county">County</label>
  <input id="county" bind:value={county} />
  <label for="city">City</label>
  <input id="city" bind:value={city} />
  <label for="state">State</label>
  <input id="state" bind:value={state} />

  <br />
  <br />
  <br />
  <br />

  <h6>Contact Information</h6>

  <label for="email">Email</label>
  <input id="email" bind:value={email} />
  <label for="phone">Phone</label>
  <input id="phone" type="number" bind:value={phone} />
  <label for="link">Link</label>
  <input id="link" type="url" bind:value={link} />

  <button type="submit">Submit</button>
  <br />
  <label for="code">Your Employer Code: </label>
  <h6 id="code">{id}</h6>
</form>
