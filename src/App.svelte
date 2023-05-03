<script lang="ts">
  import { onMount } from "svelte";

  const baseURL = "http://localhost:3000";

  const signin = () => {
    const url = new URL("/api/auth/signin", baseURL).toString();
    fetch(url, {
      method: "POST",
      credentials: "include",
    })
      .then((resp) => {
        if (resp.ok) {
          return resp.json();
        }
        throw new Error("Network response was not ok.");
      })
      .then((data) => {
        console.log("data", data);
      });
  };

  const access = () => {
    const url = new URL("/api/secure/resource", baseURL).toString();
    fetch(url, {
      credentials: "include",
    })
      .then((resp) => {
        if (!resp.ok) {
          throw new Error("Network response was not ok.");
        }
        return resp.json();
      })
      .then((data) => {
        console.log("data", data);
      });
  };

  const destroy = () => {
    const url = new URL("/api/auth/signout", baseURL).toString();
    fetch(url, {
      method: "POST",
      credentials: "include",
    })
      .then((resp) => {
        if (resp.ok) {
          return resp.json();
        }
        throw new Error("Network response was not ok.");
      })
      .then((data) => {
        console.log("data", data);
      });
  };

  let status = "unknown";

  onMount(() => {
    const url = new URL("/health", baseURL);
    fetch(url).then((resp) => {
      if (resp.ok) {
        status = "reachable";
      } else {
        status = "down";
      }
    });
  });
</script>

<main>
  <button on:click={signin}> Authenticate </button>
  <button on:click={access}> Access Secret </button>
  <button on:click={destroy}> Destroy Session </button>
  <p>Service is {status}</p>
</main>
