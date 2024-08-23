<script>
  //Tässä importataan komponentit App.svelten käytettäväksi
  import EtunimetM from './EtunimetM.svelte';
  import EtunimetN from './EtunimetN.svelte';
  import Sukunimet from './Sukunimet.svelte';
  import ErrorMsg from './ErrorMsg.svelte';
  import Footer from './Footer.svelte';
  import Ad from './Ad.svelte';

  //Importataan onMount-funktio, jota käytetään käytetään viivästetyn modaali-ikkunan näyttämiseen.
  //Kun App.svelte on ladattu, onMount-funktio käynnistyy ja aloittaa 3 sekunnin setTimeOut-funktion,
  //jonka jälkeen nayhtaModaali-muuttujan arvo kääntyy arvoksi TRUE, jolloin modaali tulee näkyviin
  import { onMount } from 'svelte';

  //Importataan fade-animaatio, jota käytetään modaali-ikkunan pehmeään ilmestymiseen ja katoamiseen, sekä generoi-napin hehkuefektiin
  import { fade } from 'svelte/transition';

  //Nimien arpomiseen käytettävät muuttujat
  let etunimetM, etunimetN, sukunimet;
  let etunimi = '';
  let sukunimi = '';
  let sukupuoli = null;
  //naytaNimi-muuttuja määrittää ritari-divin ehdollisen
  let naytaNimi = false;

  //Tässä määritetään mainosmodaali-ikkunan sulkemiseen käytettävä muuttuja (totuusarvo), sekä funktio, joka kääntää totuusarvon ikkunan sulkemista varten
  let naytaModaali = false;
  function toggleModaali() {
    naytaModaali = !naytaModaali;
  }

  //funktio joka kääntää naytaNimi-muuttujan arvon, jolloin ritari-divi renderöidään
  //Mikäli sukunimi on annettu manuaalisesti, mutta etunimikenttä on tyhjä, ei diviä tällöin renderöidä. Vähän erikoinen ratkaisu, mutta se toimii.
  function toggleNimi() {
    if (!(sukunimi.length > 0 && etunimi.length == 0)) {
      naytaNimi = !naytaNimi;
    }
  }

  //Tässä on nimen arposimeen liittyvä logiikka. Loogikan setvimiseen tarvitsin hieman tekoälyn apua. Sain lähestulkoon if-else -säännöt toimimaan omin käsin
  //mutta sukunimi-kentän arvo jäi generoinnin jälkeen jostain syystä jumiin. Eli hyvin lähellä oikeaa ratkaisua oltiin. Tarkoitus oli siis alunperin, että generoi-painiketta voisi painella
  //uudestaan ja uudestaan, eli joka painalluksen jälkeen vanhat nimet katoaisivat kentistä ja uudet tulisivat tilalle. Päädyin sitten kuitenkin käyttämään kenttien ja radiovalintojen
  //resetointiin omalla resetointinapillaan. Tämä on siitä hieman monimutkainen, että radiopainikkeet ja käyttäjän mahdollisuus antaa nimiä manuaalisesti sekoittavat soppaa.

  //Huomasin myös sellaisen seikan, että jos nimi on generoitu ja pyyhinkin tekstikentistä nimet pois, ritarinimi-div jää edelleen näkyväksi. Yritin tuota ratkoa omin avuin,
  //mutta ei tullut tulosta, enkä halunnut turvautua tekoälyyn sen enempää kuin mitä oli aivan pakko.
  //Tästä kirjoitan myös readme-tiedostoon. Päätoiminnalisuus kuitenkin toimii ihan hyvin ja olen siihen aika tyytyväinen.
  function generoi() {
    // Jos sukupuoli on valittu ja sukunimi on annettu, arvotaan vain etunimi.
    if (sukupuoli == 1 && sukunimi.length > 0) {
      etunimi = etunimetM[Math.floor(Math.random() * etunimetM.length)];
    } else if (sukupuoli == 2 && sukunimi.length > 0) {
      etunimi = etunimetN[Math.floor(Math.random() * etunimetN.length)];
    }

    //Jos sukupuoli on valittu, mutta nimiä ei ole annettu, arvotaan sekä etunimi että sukunimi.
    else if (sukupuoli == 1 && sukunimi.length == 0 && etunimi.length == 0) {
      etunimi = etunimetM[Math.floor(Math.random() * etunimetM.length)];
      sukunimi = sukunimet[Math.floor(Math.random() * sukunimet.length)];
    } else if (sukupuoli == 2 && sukunimi.length == 0 && etunimi.length == 0) {
      etunimi = etunimetN[Math.floor(Math.random() * etunimetN.length)];
      sukunimi = sukunimet[Math.floor(Math.random() * sukunimet.length)];
    }

    // Jos etunimi on annettu mutta sukunimi puuttuu, arvotaan vain sukunimi/titteli
    else if (etunimi.length > 0 && sukunimi.length == 0) {
      sukunimi = sukunimet[Math.floor(Math.random() * sukunimet.length)];
    }
  }

  // Tämä lauseke nollaa sukupuoli-valinnan, jos käyttäjä syöttää etunimen manuaalisesti.
  // Reaktiivisuus varmistaa, että sukupuoli nollautuu automaattisesti.
  $: if (etunimi.length > 0 && sukupuoli !== null) {
    sukupuoli = null;
  }

  //Funktio-joka resetoi radiopainikevalinnat ja tekstikentät.
  //Tyhjenna-funktio toimii vain silloin jos naytanimi-muuttujan arvo on true.
  //Ilman tätä if-lausetta resetointinapin klikkailu toisi jatkuvasti ritari-divin esiin ja piiloon
  //silloinkin kun nimeä ei ole olisi vielä generoitu.
  function tyhjenna() {
    if ((naytaNimi = true)) {
      etunimi = '';
      sukunimi = '';
      sukupuoli = null;
    }
  }

  onMount(() => {
    setTimeout(() => {
      naytaModaali = true;
    }, 3000); // 3000 millisekuntia = 3 sekuntia
  });
</script>

<main>
  <body>
    <!--Modaali-ikkuna renderöidään, jos naytaModaali-muuttujan arvo on true-->
    {#if naytaModaali}
      <!--taustaBlur-luokka ja sen tyylit mahdollistavat modaali-ikkunan ulkopuolisen sisällön sumentamisen, jolloin mainos on selkeämmin esillä-->
      <div class="taustaBlur" transition:fade={{ duration: 170 }}>
        <!--Ad.svelte-komponentin on:click-tapahtuma välitetään App.svelte-komponentille (event forwarding)-->
        <Ad {naytaModaali} on:click={toggleModaali} />
      </div>
    {/if}

    <div id="tekstit"></div>
    <h1>·༺ RITARIGENERAATTORI ༻·</h1>
    <h2>Generoi oma hieno ritarinimesi!</h2>
    <p id="kuvaus">
      Generaattori arpoo ritarinimen. Voit valita onko ritarisi mies, vai
      nainen. Voi halutessasi generoida kokonimen, tai esimerkiksi kirjoittaa
      haluamasi etunimen ja generoida vain sukunimen/tittelin. Tittelit ovat
      tässä tapauksessa englanniksi. Esimerkki generoidusta nimestä voisi olla
      "Gerald Armstrong" tai vaikkapa "Marcus the Conqueror". Osa nimistä on ns.
      "oikeita nimiä" ja osa kuvitteellisia. Seasta löytyy myös fantasian
      faneille tuttuja nimiä, esimerkiksi Dark Souls- ja Skyrim-peleistä.
    </p>

    <!--Radionaviskat on liitetty sukupuoli-muuttujaan "bind:group"-lausekkeella
    sukupuolen valintaa varten.-->
    <div id="genElementit">
      <label>
        <input
          type="radio"
          name="option"
          id="mies"
          bind:group={sukupuoli}
          value="1"
        />
        Mies
      </label>
      <label>
        <input
          type="radio"
          name="option"
          id="nainen"
          bind:group={sukupuoli}
          value="2"
        />
        Nainen
      </label>

      <!--Tekstikentät on liitetty kaksisuuntaisella sidonalla etunimi- ja
      sukunimi muuttujiin. Tekstikentissä on placeholder-tekstit kertomassa
      käyttäjälle mitä niihin kuuluisi pistää, mikäli käyttäjä haluaa näin
      tehdä.-->
      <input type="text" placeholder="Etunimi" bind:value={etunimi} />
      <input type="text" placeholder="Sukunimi/Titteli" bind:value={sukunimi} />
      <!--Alla on resetointinappula, jota klikatessa suoritetaan tyhjenna-funktio, joka pyyhkii radiovalinnan ja tekstikentät.-->

      <button id="reset" on:click={tyhjenna} on:click={toggleNimi}
        >Resetoi</button
      >
      <br />
      <!--Mikäli nimen generoimiseen vaadittavat ehdot (eli etunimi, sukunimi tai sukupuoli
      annettu) on täytetty, generointinappi muuttuu värilliseksi ja funktiosta tulee
      aktiivisia. Eli painettava generointinappula renderöidään ehdollisesti. Generointinapin "on:click"-lauseke suorittaa generoi-funktion, joka generoi meille nimen.
      Mikäli napin renderöinnin ehtoja ei täytetä, se on väriltään harmaa.-->
      {#if sukupuoli == 1 || sukupuoli == 2 || etunimi.length > 0 || sukunimi.length > 0}
        <button id="generoi" on:click={generoi} on:click={toggleNimi}
          >Generoi!</button
        >
      {:else}
        <button disabled>Generoi!</button>
      {/if}

      <!--Mikäli sukupuolta ei ole valittu, ehdollisesti renderöidään ErrorMsg-komponentista proppina tuotu virheviesti.
      Virheviestin saa piiloon myös antamalla manuaalisesti etunimen, koska tässä tapauksessahan käyttäjä voi aivan itse
      päättää nimen, ja täten sukupuoli-identiteettinsä :) -->
      {#if sukupuoli == null && etunimi == ''}
        <ErrorMsg
          virheviesti={'Valitse sukupuoli/haluamasi etunimi TAI sukupuoli + haluamasi sukunimi'}
        />
      {/if}
    </div>

    <!--Miesten ja naisten etunimet sekä sukunimet/tittelit tuodaan omista komponenteistaan ja
  data sidotaan täällä, jotta varsinaiset nimet voidaan arvottaa.-->

    <EtunimetM bind:etunimetM />
    <EtunimetN bind:etunimetN />
    <Sukunimet bind:sukunimet />
    {#if naytaNimi == true}
      <div id="ritarinimi" transition:fade={{ duration: 300 }}>
        <h2>Ritarinimesi on...</h2>
        <p>{etunimi}</p>
        <p>{sukunimi}</p>
      </div>
    {/if}

    <!--Footerilla annetaan proppeina projektin nimi, tekijän tiedot ja vuosi-->
    <Footer
      projNimi="Frotend-perusteiden lopputyö"
      copyright="Miska Salonen (AD1317, HTK22)"
      year={2024}
    />
  </body>
</main>
/* Footer on kiinnitetty näytön alareunaan "position: fixed" -ominaisuudella */

<!--Ja tässä on tyylittelyt. Olen pyrkinyt nimeämään id:t ja luokat selkeästi.-->
<style>
  body {
    display: flex;
    flex-direction: column;
    flex-grow: 1;
    background-color: black;
    min-height: 100vh;
  }
  h1 {
    text-align: center;
    font-size: 50px;
  }

  h2 {
    text-align: center;
    font-size: 30px;
  }

  p {
    font-size: 20px;
    margin-left: 70px;
    margin-right: 70px;
    text-align: center;
  }

  #kuvaus {
    text-align: center;
    font-size: 20px;
  }

  /**"Generointilaatikon" elementit, eli radionapit, tekstikentät, sekä generoi- ja resetoi-napit*/
  #genElementit {
    margin-top: 20px;
    text-align: center;
  }

  /**Generoidulle nimelle tarkoitettu ID */
  #ritarinimi {
    margin-left: 300px;
    margin-right: 300px;
    margin-top: 30px;
    border-style: solid;
    border-color: rgb(153, 121, 80) rgb(131, 65, 28) rgb(107, 61, 5)
      rgb(131, 65, 28);
    border-width: 10px;
    box-shadow: 0 0 8px 8px #684c27;
  }

  /**Generointipainikkeen tyylit*/
  #generoi {
    padding: 5px;
    margin-top: 20px;
    border-color: rgb(153, 121, 80) rgb(131, 65, 28) rgb(107, 61, 5)
      rgb(131, 65, 28);
    border-width: 3px;
    font-size: 20px;
    background-color: rgb(165, 107, 0);
  }

  /**Aktiivisen generointipainikkeen efektit, eli efektit jotka näkyvät kun kursori asetetaan napin päälle*/
  #generoi:hover {
    box-shadow: 0 0 4px 4px #684c27;
    transition-duration: 0.1s;
  }

  /**Näitä tyylejä käytetään generoi-napille, kun generointiin tarvittavia ehtoja ei ole täytetty, eli nappi on "deaktivoitu"*/
  button:disabled {
    padding: 5px;
    margin-top: 20px;
    border-color: grey;
    border-width: 3px;
    font-size: 20px;
    background-color: rgb(66, 66, 66);
  }

  /**Modaali-ikkunan ulkopuolella olevien elementtien sumentaminen, kun modaali on näkyvillä*/
  .taustaBlur {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100vh;
    background: rgba(0, 0, 0, 0.068);
    z-index: 10;
    backdrop-filter: blur(3px);
  }
</style>
