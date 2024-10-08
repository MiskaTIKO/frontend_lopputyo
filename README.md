Frontend-perusteet -opintojakson harjoitustyö


- Tekijä: Miska Salonen, AD1317
- Harjoitustyön nimi: Ritarigeneraattori


Kuvaus:
- Ritarigeneraattori arpoo käyttäjälle ritarillisen nimen. Käyttäjä voi valita, onko hänen ritarinsa mies vai nainen.
Nimet arvotaan valmiiksi määritellyistä listoista ja arpomiseen vaikuttaa käyttäjän syötevalinnat. Osa nimistä on oikeita
nimiä, osa erinäisistä fantasiamedioista lainattuja ja osa itse keksittyjä. Kun sivu avataan, käyttäjälle esitetään
Jamkin hieno mainos, joka on suljettavissa ruksista. 


Osallistunko loppukeskusteluun?
- Harjoitustyötä tehdessäni tähtäsin maksimissaan 3:n arvosana vaatimuksiin, eli loppukeskustelu ei ole tarpeen.


Työssä hyödynnetyt materiaalit:
- Harjoitustyön toteutus tapahtui pääsääntöisesti omin käsin, mutta tietysti koska kaikkia kurssin asioita on vaikea muistaa
ulkoa - erityisesti näin kesäloman jälkeen - niin pieniä palasia piti hakea sieltä sun täältä, lähinnä kurssin materiaaleista,
aikasemmin tekemistäni tehtävistä, Stack Overflow- ja Reddit-keskusteluista, sekä hieman YouTuben puolelta. Eli perustoiminallisuuksien ymmärtämistä varten.

- Event forwardingin ymmärtämiseen hyödynsin Net Ninja-kanavan "Svelte Tutorial for Beginners #12 - Event Forwarding" -videota (https://youtu.be/SaMils0yx7s).


Tekoälyn hyödyntäminen
- Kirjoitin tästä jo aika kattavasti App.svelte-komponentin kommenteissa, joten laitan samaisen kommentin tänne:

- Tässä on nimen arposimeen liittyvä logiikka. Loogikan setvimiseen tarvitsin hieman tekoälyn apua. Sain lähestulkoon if-else -säännöt toimimaan omin käsin,
mutta sukunimi-kentän arvo jäi generoinnin jälkeen jostain syystä jumiin. Eli hyvin lähellä oikeaa ratkaisua oltiin. Tarkoitus oli siis alunperin, että generoi-painiketta voisi painella
uudestaan ja uudestaan, eli joka painalluksen jälkeen vanhat nimet katoaisivat kentistä ja uudet tulisivat tilalle. Päädyin sitten kuitenkin käyttämään kenttien ja radiovalintojen
resetointiin omalla resetointinapillaan. Tämä on siitä hieman monimutkainen, että radiopainikkeet ja käyttäjän mahdollisuus antaa nimiä manuaalisesti sekoittavat soppaa.

- Huomasin myös sellaisen seikan, että jos nimi on generoitu ja pyyhinkin tekstikentistä nimet pois, ritarinimi-div jää edelleen näkyväksi. Yritin tuota ratkoa omin avuin,
mutta ei tullut tulosta, enkä halunnut turvautua tekoälyyn sen enempää kuin mitä oli aivan pakko. Tästä kirjoitan myös readme-tiedostoon. Päätoiminnalisuus kuitenkin
toimii ihan hyvin ja olen siihen aika tyytyväinen.

- Eli siis kyseinen hyödynnetty tekoäly on ChatGPT. Paria kommenttia varten taisin kysyä myös tekoälyltä pientä selitystä, eritoten
onMount-funktion toiminnasta suhteessa setTimeOut-funktioon. Tiesin että näitä kahta tarvitaan modaali-ikkunan viivästettyyn esittämiseen,
mutta halusin selkeä selityksen sille, mitä näiden kahden välillä oikeasti käytännössä tapahtuu.
