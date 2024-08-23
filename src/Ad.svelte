<script>
  //muuttuja naytaModaali viedään App.svelte-komponentin käytettäväksi modaali-ikkunan renderöintiä varten
  export const naytaModaali = false;
</script>

<!--taustaBlur-luokka löytyy App.svelte-komponentin tyylittelyistä. Olisin mielummin asettanut
kyseisen luokan tämän komponentin tyyleihin, mutta en saanut sitä toimimaan oikeellisesti.-->
<div id="modaali" class="taustaBlur">
  <img
    src="src\jamkImage\jamk_tunnus_sininen_nimella_suomi.png"
    alt="jamkLogo"
  />

  <p>
    "Tule sinäkin opiskelemaan Jamkiin! Täällä on hauskaa ja kivaakin jopa!
    Erityisesti Frontend!"
    <br />
    <br />
    - Miska Salonen, TIKO-nautiskelija
  </p>

  <!--Tyhjä on:click-tapahtuma, joka välitetään App.svelte-komponentille (event
  forwarding). on:click-tapahtuma lähetetään ylöspäin, jotta App.svelte voi hallita modaali-ikkunan sulkemista. -->
  <button id="ruksi" on:click>X</button>
</div>

<style>
  #modaali {
    width: 500px;
    height: 400px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    background-color: white;
  }

  #ruksi {
    border-style: solid;
    border-color: rgb(0, 0, 0);
    height: 30px;
    width: 30px;
    position: absolute;
    right: 0;
    top: 0;
  }

  img {
    display: block;
    margin-left: auto;
    margin-right: auto;
    width: 70%;
  }

  p {
    margin-left: 5px;
    margin-right: 5px;
    margin-top: 70px;
    font-weight: 700;
    text-align: center;
    color: black;
    font-size: 16px;
  }
</style>
