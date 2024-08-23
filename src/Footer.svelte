<script>
  //oman projektin nimi ja tekijän tiedot, jotka ovat muokkatavissa proppeina App.svelte-komponentissa
  //Proppien käyttäminen tässä tekee footerista yleiskäyttöisemmän, dynaamisen, kun tietoja voidaan muokata juurikomponentissa tarpeen mukaan
  export let projNimi = 'oma projekti';
  export let copyright = 'tekijän nimi yms yms';
  //nykyinen vuosi, joka lasketaan automaattisesti
  export const year = new Date().getFullYear();
</script>

<footer>
  <p style="display: inline;">{projNimi}, {copyright} - &copy; {year}</p>
</footer>

<style>
  /* Footer on kiinnitetty näytön alareunaan "position: fixed" -ominaisuudella */
  footer {
    background-color: rgb(31, 31, 31);
    position: fixed;
    bottom: 0;
    width: 100%;
    box-sizing: border-box;
  }
</style>
