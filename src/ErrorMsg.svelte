<script>
  //muuttuja virheviesti alustetaan ja lähetetään App.svelte-komponentin käytettäväksi proppina
  export let virheviesti = '';
</script>

<main>
  <p>{virheviesti}</p>
</main>

<style>
  p {
    color: red;
  }
</style>
