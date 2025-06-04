# Ohjelmistokehityksen sovellusprojekti

Tälle sivulle on kasattu ohjaavien opettajien kommentteja ja vinkkejä projektiin liittyen

- Digitaloceaniin (Linux-palvelin pilvestä) saa $200 vuodeksi, jos hakee täältä voucherin students.oamk.fi emaililla: [https://education.github.com/pack](https://education.github.com/pack])

- Huomatkaa myös että Linux on "case sensitive" ja Windows ei. Ja lisäksi UniServerin MySQL luo aina taulut pienin kirjaimin vaikka kirjoitatte ne isolla alkukirjaimilla. Teidän tulisi kirjoittaa backendissä SQL koodissa taulun nimet niin kuin ne on tietokannassa. Jos taulun nimi on car ja si
ellä kenttä brand ja kirjoitetaan SQL-koodia näin

   ```text
   SELECT Brand FROM Car;
   SELECT Brand FROM car;
   ->Eka kyselyn antaa virheen, koska Car-taulua ei ole
   ->Toka kysely toimii, se että Brand on kirjoitettu "väärin" ei haittaa.
   ```

- Kortti kytketty debit ja credit tiliin: Jos toteutetaan ominaisuus, että kortilla voi olla sekä debit, että credit ominaisuus, niin sehän tarkoittaa että "yksi kortti voidaan liittää useampaan tiliin ja yhdelle tilille voidaan liittää monta korttia"
  - Tämä on siis monen-suhde-moneen yhteys, joka kannattaa ratkaista ihan tavanomaisella relaatiotietokannan ratkaisulla
  - Ei kannata antaa sen hämätä, että kortti on kuitenkin käytännössä liitetty korkeintaan kahteen tiliin
  - Useampi ryhmä on koettanut ratkaista tuon laittamalla card-tauluun kaksi viiteavainta osoittamaan account-tauluun. Sen saa toimimaan, mutta ei ole paras ratkaisu

- Kannattaa tehdä se kanban yhdessä ryhmän kanssa ja miettiä että mitä kaikkia eri asioita siinä vois olla. Sehän on semmoinen jatkuvan prosessin työkalu mihin voi lisätä kyllä niitä taskeja matkan varrella, ettei sitä ole tarkoituskaan saada heti kerralla valmiiksi:
  - Esim. se API voisi olla sitten ihan pilkottuna, että mitä erilaisia urleja tai toimintoja sen kautta voidaan tehdä ihan niillä funktioilla mitä siellä Qt:ssä ajetaan, kun ne käyttää sitä rajapintaa
  - Kun nämä on niin pieniä taskeja ja projekti, niin ei kannata tehdä semmoisia taskeja sinne kanbaniin joiden tekeminen kestää useamman päivän vaan just semmoisia muutaman tunnin hommia
  - Ylipäätänsä ei kannata tietotekniikassa (softaa, rautaa, mitä vaan) yrittää tehdä kerralla mitään valmista, vaan oikein pienissä vaiheissa eteenpäin ja testaa sitä mukaa että toimii jokin juttu ja sitten vasta eteenpäin
  - Eli teette ensin tietokannan, sitten API siihen eteen ja koitatte vaikka postmanilla sitä APIa ja kun API on iskussa, niin sitten sillä Qt:lla tekemään käyttöliittymää
  - Semmoiset reverse proxyt jne (jos olette tekemässä) ehtii sitten kyllä lisätä lopuksikin sinne väliin
  - Kun tietokanta on tehty ja hyväksytty, niin voi lähteä tekemään sitä backendiä. Siinä voisi olla työnjakona sellainen, että kukin ryhmän jäsen tekee yhdelle tietokanta-taululle CRUD-operaatiot

- Jos otatte sen Linux-palvelimen käyttöön, niin tässä muutamia asioita. En suhtautuisi tietoturvaan liittyviin juttuihin asenteella "tämähän on vain koulun projekti". Huonot käytänteet jäävät helposti päälle.
  - 1. Pankkiautomaattia ajatellen MySQL:ään kannattaa luoda käyttäjä jolla on vain ne oikeudet jotka tarvitaan. Eli varmaan jotain tämmöistä:
     - oikeudet päivittää (UPDATE) account-taulua
     - oikeudet lisätä(INSERT) transaction tauluun
     - oikeudet suorittaa proseduuri/proseduureja
  - Näistä on esimerkkejä sivulla: [https://peatutor.com/databases/mysql.php#usermanagement](https://peatutor.com/databases/mysql.php#usermanagement)
  - Mutta siinä vaiheessa kun testaatte Postmanilla APIa niin tulee olla MySQL käyttäjä jolla on kaikki oikeudet siihen tietokantaan.
  - 2. Varmaankin siinä Linuxissa on oletuksena seuraavat rajoitukset:
    - palvelin kuuntelee vain 127.0.0.1 osoitteesta tulevia pyyntöjä
    - root saa kirjautua vain localhostista
  - Tuohon ekaan olen tietokannat kurssilla näyttänyt ohjeet. kts. [https://youtu.be/sQCz1m72wno](https://youtu.be/sQCz1m72wno). Noin kohdassa 1:45 alkaa tuo Linux-juttu. Eli voidaan laittaa se kuuntelemaan muualtakin tulevia pyyntöjä.
  - Mutta jos se node.js sovellus on samalla koneella, niin ei tuota tarvitse avata. Tietokantaan voi kytkeytyä niin että ottaa SSH-yhteyden ja käyttää komentorivi-clienttia. Tai sitten Workbenchillä voi ottaa yhteyden, kun konffaa siihen "Connection methodiksi" Standard TCP/IP over SSH".

- Javascriptistä ja tuosta backendistä pari juttua:
  - Noudattakaa MVC-mallia eli tehkää mieluiten sen minun mallin mukaan, jossa Models-kansiossa on ne SQL-jutut ja Routes-kansiossa se "kontrollerit". Tätä pitäisin ehdottomana vaatimuksena
  - Mielellään teette projektin mallin mukaan perinteisillä "callbackeillä". Vaikka tähän on uudemmat systeemit "Promises" ja "async-await", niin olisi hyvä ymmärtää tuo callback systeemi. Seuraavassa projektissa saatte käyttää noita uudempia juttuja. Tässä sovelluksessa ei edes synny ns. "ca
llback-helvettiä", jonka vuoksi nuo uudemmat systeemit on kehitetty

- Tuolla on esitelty noita UML-kaavioita: [https://www.cs.helsinki.fi/u/pohjalai/ke10/ohma/slides/ohma_03-UML.pdf](https://www.cs.helsinki.fi/u/pohjalai/ke10/ohma/slides/ohma_03-UML.pdf)
  - Ja Teemun web-sivullahan on linkkejä kirjoihin.
  - Kuitenkin tässä projektissa on tärkeämpää tuo koodaaminen kuin nuo dokumentit.

- Nyt kun näitä eri projekteja on useimmilla menossa, niin tässäpä taas yleinen muistutus. Vaikka Github onkin erityisesti hajautettu versionhallintapalvelu, niin moni käyttää sitä lähinnä ja tai myös oman ”devaaja-CV:n” / portfolion rakentamiseen Tässä awesome-listassa on iso kasa esimerkkej
ä: [https://github.com/matiassingers/awesome-readme](https://github.com/matiassingers/awesome-readme). Joitakin poimintoja:
  - [https://github.com/sourcerer-io/sourcerer-app#readme](https://github.com/sourcerer-io/sourcerer-app#readme)
  - [https://github.com/ma-shamshiri/Spam-Detector#readme](https://github.com/ma-shamshiri/Spam-Detector#readme)
  - [https://github.com/NSRare/NSGIF#readme](https://github.com/NSRare/NSGIF#readme)
  - [https://github.com/sulu/sulu#readme](https://github.com/sulu/sulu#readme)

 
- MySQL:n Bind-address: Uniserveristä se my.ini:n löytää helposti:
  - Avaa Control-paneelin
  - MySQL palvelin ei saa olla käynnissä
  - Valikosta MySQL valitaan "Edit file my.ini"
  - Tietokantojen luennolla näytin kuinka sen löytää Linuxissa
  - Linuxissa voi MySQL:n asentaa hyvin monella tavalla ja ne eri versiot sijoittaa sen cnf-tiedoston usein eri kansioon

- Ja ettei menis ihan liian vakavaksi: 😊  [https://open.spotify.com/playlist/0MJBni0UzdnML1amikx0Rc](https://open.spotify.com/playlist/0MJBni0UzdnML1amikx0Rc)

- Versionhallintamalleista: Versionhallintamalleja on monia ja tässä projektissa käytetty malli noudattaa ns. GitHub Flow mallia:
  - **GitHub Flow:**
     - Käyttö: Jokainen uusi ominaisuus tai korjaus tehdään omassa haarassaan, joka yhdistetään päähaaraan tarkistuksen jälkeen. Käytössä on yleensä vain yksi päähaara.
     - Sopii: Tiimeille, jotka julkaisevat jatkuvasti tai haluavat yksinkertaisen työnkulun ilman erillistä kehityshaaraa.
     - Hyöty: Helpompi ottaa käyttöön kuin Git Flow, koska ei ole monimutkaisia haaramalleja.
  - **Git Flow:**
     - master-haara edustaa tuotantoversiota
     - develop-haara toimii aktiivisen kehityksen pohjana
     - Erilliset feature, release, ja hotfix-haarat hoitavat spesifit tehtävät.
  - **Trunk-based development:**
     - Kaikki kehitys tapahtuu suoraan päähaarassa tai hyvin lyhytaikaisissa haaroissa
     - Hyöty: Nopea julkaisu (Continuous Integration ja Continuous Deployment).
     - Haitta: Suuremmat riskit, jos ei ole vahvoja automaattisia testejä ja tiivistä koodiarviointia.

- Jos node-serveri on käynnissä ja suljette komentokehotteen, se node todennäköisesti jää käyntiin. Ja kun annatte komennon npm start saatte ilmoituksen "portti 3000 on jo käytössä". Tuolla on ohje, kuinka etsitte netstat-komennolla prosessin id:n ja sitten tapatte sen. [https://peatutor.com/express/nodejs.php#kill](https://peatutor.com/express/nodejs.php#kill)
  - Linuxissa voi tappaa prosessit myös ihan pkill komennolla antamalla nimen tyyliin: pkill nimi
  - Jos tappaa prosessin, mikä on bindattu johonkin TCP-sokettiin, niin joskus protokollapino pitää sen portin varattuna noin 4 minuuttia. Siellä on käyttiksestä riipuen sellainen TCP wait delay, mikä pitää sen portin varattuna, eikä anna käyttää sitä uudestaan, jos siihen porttiin on saapumassa myöhässä jotain vanhaa liikennettä
  - Joku muu pitää luultavasti teille tän mun suunnitteleman kurssin myöhemmin. viikot 3 ja 4 sivuaa noita [https://tl.oamk.fi/iot/](https://tl.oamk.fi/iot/)
  - Mistä tulikin mieleen, että teillähän on ens periodissa se pilvipalvelut-kurssi, mikä on aika valmis, mutta pitää vähän tsekata sisältöä vielä. Sen sivu on täällä: [https://tl.oamk.fi/cloudservices/](https://tl.oamk.fi/cloudservices/)

- Yleisenä muistutuksena, että kun teette API- ja tietokantajuttuja, että fiksut rajapinnat noudattaa aina PoLP-periaatetta (Principle of Least Privilege) alustasta riippumatta
  - Kannattais käyttää useita eri tunnuksia kantaa tai tarkemmin vaikka taulujakin vasten
  - Esim. API-operaatio mikä vain lukee, käyttää vain tunnusta, millä on vain SELECT-oikeus eikä muuta
  - Vastaavasti operaatio mikä muuttaa dataa, on vain UPDATE-oikeudella, eikä mitään muita oikeuksia jne.
  - Tuossa on se hyvä puoli, että jos API:sta löytyy joku bugi ja se on vaikka vain semmoisessa toiminnossa, missä on käytössä vain SELECT-oikeuksin oleva tunnus, niin joku ei voi tuhota tai muuttaa kantaa, vaan vain esim. varastaa sillä bugilla dataa
  - Tuotahan ei ollut speksattu nyt API-vaatimuksiin tän projektin osalta, mutta olisi toteutettavaissa ainakin siten, että tekee useita erillisiä API-rajapintoja, jotka käyttää vastaavasti eri oikeuksilla olevia yhteyksiä kantaan

- Jos ja kun teillä on HTTP API, niin sen ja web-sivujen tietoturvasorkkimiseen:
   - (paras) [https://portswigger.net/burp](https://portswigger.net/burp) tai [https://portswigger.net/burp/communitydownload](https://portswigger.net/burp/communitydownload)
   - (open source vastine): [https://www.zaproxy.org/](https://www.zaproxy.org/)
   - Pythonilla tehty SQL-sorkkimistyökalu: [https://sqlmap.org/](https://sqlmap.org/)
   - IPPSec korkkaa hacktheboxin käytöstä poistuneita virtuaalikoneita. Videoilla on aika usein jotain web- tai api-juttuja, missä tarkoituksella reikiä: [https://www.youtube.com/@ippsec/videos](https://www.youtube.com/@ippsec/videos)

- Tuon backendin kuvaaminen luokkakaaviossa voi tuntua hankalalta. Laitoin tuonne esimerkkiin vähän mallia siitä
  - Eli siis "app" on yksi luokka. student (eli router/student.js tiedostossa oleva koodi) muodostaa luokan, jolla on yhteys(assosiaatio luokan app kanssa)
  - Ei ole siis perintä eikä kompositio
  - [https://github.com/tvt24kmo-project/group_x](https://github.com/tvt24kmo-project/group_x)
  - Piirtelin sen kaavion nyt LucidChartilla. Tuon StudentRouter-luokan osalta en noudattanut ihan perinteistä UML tyyliä noiden metodien kuvauksessa. Mielestäni tuo tapa jolla ne tein antaa paremman informaation, jos tuon avulla pitäisi koodata

- Kannattaa muuten seurata tuon Julia Evansin blogia. Kirjoittaa ihan hyviä postauksia ohjelmistokehittäjän arjesta ja oivalluksista miten se itse kehittyy (varsinkin jos lukee vanhoja postauksia) [https://jvns.ca/](https://jvns.ca/)

- Pull request -pohdintaa / template: [https://ashleemboyer.com/blog/pull-request-template/](https://ashleemboyer.com/blog/pull-request-template/)

- Jos ja kun räpellätte jotain vscodella ja jos tuntuu oikein seikkailuhaluiselta, niin kokeilkaapa pair programmingia: [https://marketplace.visualstudio.com/items?itemName=MS-vsliveshare.vsliveshare](https://marketplace.visualstudio.com/items?itemName=MS-vsliveshare.vsliveshare)
  - Siinä toinen tai toiset koodaa ja toiset katselmoi koodia reaaliajassa. Huono puoli tietysti että menee kahden devaajan aika
  - Toisaalta sitten semmoisessa mentoroinnissa tai softan tekijä perehdyttää uutta työkaveria softaan, niin saattaa olla erinomainen tapa
  - Esim: [https://www.reddit.com/r/ExperiencedDevs/comments/10rgn4w/does_anyone_find_full_time_pair_programming/](https://www.reddit.com/r/ExperiencedDevs/comments/10rgn4w/does_anyone_find_full_time_pair_programming/)
  - Ja muutenkin tuo r/experienceddevs subreddit on ihan hyvää seurattavaa devaajille: [https://www.reddit.com/r/ExperiencedDevs/](https://www.reddit.com/r/ExperiencedDevs/)

- Generoin tekoälyn avulla artikkelin "Hyödyt erillisen DLL:n käytöstä": [https://peatutor.com/qt/dll.html](https://peatutor.com/qt/dll.html)
  - Ja tuossa esittelen tekemääni REST API DLL kokeilua: [https://www.youtube.com/watch?v=HOvhjwfuD4Q](https://www.youtube.com/watch?v=HOvhjwfuD4Q)
 
  
- Eilen palaverissa esitettiin hyvä kysymys:"Kannattaako API:sta palauttaa kerralla iso joukko tietoa?" Tuota kannattaa pohtia tilanteen mukaan
  - Tässä sovelluksessa se tulee vastaan ainakin tilitapahtumien näyttämisessä. Kun toiminta on sellainen, että näytetään 5 viimeistä tapahtumaa ja käyttäjä voi pyytää 5 lisää ja kelata sitten taaksepäin
  - Tässähän voidaan tehdä niin että haetaan kaikki tapahtumat ja sitten frontendissä suodatellaan niitä. Tai sitten niin että API antaa ne 5 ja jos halutaan 5 lisää lähetetään uusi request
  - Tässä tilanteessa voisi miettiä, että yleensä asiakas haluaa nähdä vain 5 viimeistä, joten API antaisi vain ne. Ja sitten tehtäisiin aina uusi pyyntö
  - Jos jostain syystä on päädytty ratkaisuun, että API palauttaa kaikki tapahtumat, niin sitten on järkevintä tehdä kaikki suodatukset frontendissä. On hölmöä lähettää uusia requesteja, jos data on jo kerran haettu
  - Ja siis yhteenvetona: Aina kannattaa palauttaa APIsta vain välttämätön data, mutta toisaalta kannattaa minimoida tarvittavien requestien tarve
  - Tietoa ei kannata siirtää turhaan: Esim. jos haetaan kaikki data API:sta, niin monissa pilvipalveluissa (kuten AWS) usein ne isoimmat kulut on tietoliikennekulut sieltä konesalista ulospäin lähtevälle datalle. Jos datamäärät on isoja, niin tuo kerryttää tarpeettomasti laskua. Ei välttämättä nyt tässä tapauksessa, mutta noin ylipäätänsä
  - Nykyserverit on niin tehokkaita, että aika halpa ja tehoton palvelin pystyy käsittelemään kymmeniä tai satoja pyyntöjä sekunnissa
  - Kannattaa tehdä operaatiot aina tietokannassa jos voi (ei ole mitään järkeä siirtää dataa ulos kannasta, prosessoida sitä clientissa ja siirtää sitä takaisin kantaan ellei ole pakko)
  - Clienttiin ei voi koskaan luokkaa tietoturvamielessä, vaikka olisi mikä. Kaikki tietosuojan tai tietoturvan kannalta kriittiset jutut pitää tehdä aina ainoastaan palvelinpäässä

- Tuli muuten mieleen nämä meikäläisen kavereiden vierailujaluennot. enimmäkseen softadevaukseen liittyviä luentoja. 3-4 vuotta vanhoja, mutta nuo asiat ei ole sinänsä muuttunu miksikään: [https://tl.oamk.fi/asiantuntijavieraat/](https://tl.oamk.fi/asiantuntijavieraat/)

- IT-yleissivistyksenä future crewin second reality -demo vuodelta -93. tuo oli aikoinaan jotain ihan käsittämätöntä, kun se julkaistiin [https://www.youtube.com/watch?v=iw17c70uJes](https://www.youtube.com/watch?v=iw17c70uJes)

- Tuli tuossa vähän keskustelua ajasta ja aikavyöhykkeistä datalle ja muutenkin
  - Kun käsittelee aikaa, niin tietokantaan kannattaa varastoida aika aina UTC:nä ja sitten ku sitä dataa käsitellään, niin korjaa tarvittaessa UI:ssa/clientissa aikavyöhykkeen
  - Lisäksi kannattaa selailla tämä githubin gist, missä on aikaan liittyviä eriskummallisuuksia koodaajille: [https://gist.github.com/timvisee/fcda9bbdff88d45cc9061606b4b923ca](https://gist.github.com/timvisee/fcda9bbdff88d45cc9061606b4b923ca)
  - Tuo hoituu helpoiten käyttämällä MySQL:ssä tietotyyppiä TIMESTAMP. Sen voi sitten SELECT:ssä muokata haluttuun aikavyöhykkeeseen
  - Tuossapa pieni info tuosta time_zone:sta: [https://youtu.be/j7kLNyKv36U](https://youtu.be/j7kLNyKv36U)

- Peli Gitin opetteluun: [https://github.com/git-learning-game/oh-my-git](https://github.com/git-learning-game/oh-my-git)

- 80-luvun alkupuolen ohjelmointikirjoja lapsille. Vastaavista sitä tuli itsekin opeteltua atk-alkeita pohjimmiltaan monet asiat on edelleen ihan samoin vaikka välissä onkin erilaisia kerroksia piilottamassa toimintaa
   - [https://usborne.com/row/books/computer-and-coding-books](https://usborne.com/row/books/computer-and-coding-books)
   - Esim. Basicista ja konekielestä on hyvä aloittaa! 🙂 [https://drive.google.com/file/d/0Bxv0SsvibDMTcHNXalEtYkVtU00/view?resourcekey=0-YtCcUG7ytBL-ls8gciJ7ig](https://drive.google.com/file/d/0Bxv0SsvibDMTcHNXalEtYkVtU00/view?resourcekey=0-YtCcUG7ytBL-ls8gciJ7ig)
   - Mistä tulikin mieleen, että tämä peli opettaa jonkin verran algoritmiajattelua. Siinä pitää pelin omalla assemblyllä koodata rikkinäiseen keskustietokoneeseen toimivia logiikkakomponentteja hyvin matalalla tasolla. Ratkaisuja on aina useampia, mutta tavoitteena on saada tietysti mahdollisimman suorituskykyinen toteutus. Youtubessa on tietty paljon ratkaisuja ja esimerkkejä ko. pelistä: [https://store.steampowered.com/app/370360/TIS100/](https://store.steampowered.com/app/370360/TIS100/)

- Muistutuksena että CACM-lehti on nykyään ilmaisena luettavissa netissä. Sellainen aika paljon tavallista tietokonelehteä syvempiä artikkeleita sisältävä julkaisu, missä vieraskirjoittajat on usein maailmanluokan osaajia jossain tietyssä jutussa tai muuten vaan kuuluisia: [https://cacm.acm.org/](https://cacm.acm.org/)

- CUPID - for joyful coding [https://dannorth.net/cupid-for-joyful-coding/](https://dannorth.net/cupid-for-joyful-coding/)

- Luokkakaaviosta
   - Luokkakaaviossa ei kannata esitää niitä Qt:n luokkia, vaan ainoastaan ne jotka itse teette
   - Kun lisäätte niitä formeja ja niin ne voi sinne laittaa esimerkiksi MainWindow, Login ym.
   - Mutta esimerkiksi kaikki ne Ui:n luokat QPushButton, QLineEdit jne. kannattaa jättää pois
   - Teidän sovelluksennehan sisältää noiden luokkien olioita, mutta ei tosiaan kannata niitä laittaa

- Nämä beej:n tekemät oppaat on aika kuuluisia: [https://beej.us/guide/bggit/](https://beej.us/guide/bggit/)

- Sellaisena yleisenä kommenttina siihen tämän 5. viikon aikana valmiiksi tehtävään tekniseen määrittelyyn (näemmä sen ympärillä velloo hieman keskustelua):
  - Alkää jumittako liikaa siihen formaaliin muotoon, vaan miettikää sitä tehdessä, että näkeehän siitä dokumentista, että miten applikaatio toimii tai on tarkoitus toimia, millainen tietokantarakenne on ja mitkä on ne keskeiset toiminnot, rajapinnat, kirjastot ja luokat mitä applikaatio käyttää
  - Ei noihin ole olemassa mitään ehdotonta standardia (no UML ja erilaiset vuokaavion on toki näppäriä) vaan se, että siitä dokumentista näkee em. listatut asiat selkeästi. Se on se homman henki mitä sillä dokumentilla haetaan eikä minkään muodollisuuden toteutuminen
  - Esimerkki: Projektiin tulee uusi koodaaja, niin tuosta dokkarista ja lähdekoodia katsomalla voi sitten perehtyä siihen kokonaisuuteen, että miten se toimii. Näkeekö siitä dokkarista tarpeeksi hyvin että miten ne eri rajapinnat toimiii? Mitkö on ne keskeiset luokat? Miten tietokanta toimii? Millainen arkkitehtuuri (missä hostattu, onko proxyä yms). Millaisia ominaisuuksia suunnitteilla vielä lisää jne.
  - Ajatelkaa vaikka itsenne tilanteeseen, että liitytte uuteen tiimiin ja pitää alkaa korjaamaan bugeja tai ylläpitämään jostain softaa missä on useita eri komponentteja, verkkoyhteyksiä, palvelimia, tietokantoja jne. Mitä toivoisitte että dokkarista käy ilmi että siihen järjestelmään pääsee paremmin käsiksi? (lähdekoodien lisäksi)

- Ootte käyttänyt postmania, niin tässä on avoimen lähdekoodin MIT-lisensoitu vaihtoehto, mikä myös suosittu [https://github.com/hoppscotch/hoppscotch](https://github.com/hoppscotch/hoppscotch)

- Esimerkki sellaisesta miten voisi tehdä täydellisessä IT-maailmassa blogipostauksesta. Sinänsä ihan hyviä pointteja API-avaimien ja salasanojen varastointiin ja käyttöön liittyen: [https://www.nodejs-security.com/blog/do-not-use-secrets-in-environment-variables-and-here-is-how-to-do-it-better](https://www.nodejs-security.com/blog/do-not-use-secrets-in-environment-variables-and-here-is-how-to-do-it-better)

- 10 vuotta devaajana ja mietteitä: [https://chriskiehl.com/article/thoughts-after-10-years](https://chriskiehl.com/article/thoughts-after-10-years)


- Ihan mielekästä pohdintaa LLM AI vaikutuksesta softainssihommiin [https://serce.me/posts/2025-02-07-the-llm-curve-of-impact-on-software-engineers](https://serce.me/posts/2025-02-07-the-llm-curve-of-impact-on-software-engineers)

- Prompt-ideoita copilotin kanssa koodin korjailuun: [https://github.blog/ai-and-ml/github-copilot/how-to-refactor-code-with-github-copilot/](https://github.blog/ai-and-ml/github-copilot/how-to-refactor-code-with-github-copilot/)

- Sellaisena villinä heittona tuli mieleen, että jos  teillä pyörii MySQL ja node.js:llä tehty API samassa raudassa, niin joku shared memory tai IPC (käytännössä named pipes) mahdollistais luultavasti nopeamman tiedonsiirron node.js:n ja MySQL:n välillä. Ei tietoa miten onnistuu nodella, jos  edes onnistuu: [https://stackoverflow.com/questions/271864/best-way-to-connect-to-mysql-locally](https://stackoverflow.com/questions/271864/best-way-to-connect-to-mysql-locally) Ne TCP/IP-soketit on kuitenkin aina vähän hitaita saman palvelimen sisällä käytettynä vrt putkiin tai suoraan muistin kautta käsiteltävään dataan ilman välissä olevaa protokollapinoa

- Suosittelen tutustumaan OSB:ään jos vähänkään enemmän teette screencastina videoita tai streamaatte: [https://obsproject.com/](https://obsproject.com/)

- Jos ei ole vielä tullut vastaan tämä repo "This repository is a compilation of well-written, step-by-step guides for re-creating our favorite technologies from scratch." [https://github.com/codecrafters-io/build-your-own-x](https://github.com/codecrafters-io/build-your-own-x)
