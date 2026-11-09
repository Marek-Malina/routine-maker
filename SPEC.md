# Routine Maker – Špecifikácia požiadaviek

Verzia 1 (MVP). Súvisiace dokumenty: [project.md](project.md), [plan.md](plan.md).

---

## 1. Problém

Ľudia si stanovujú rutiny (posilňovňa ráno, beh, chodenie do kancelárie), ale nemajú jednoduchý spôsob, ako automaticky overiť, či ich naozaj dodržiavajú. Ručné zapisovanie ich rýchlo prestane baviť a bez spätnej väzby rutina zanikne.

## 2. Používatelia a roly

Aplikácia má **jednu rolu: Používateľ**. Neexistuje administrátor ani používateľské účty. Aplikácia beží na jednom telefóne a všetky dáta patria jeho majiteľovi.

Typickí používatelia:
- ľudia, ktorí si budujú zdravé návyky (posilňovňa, beh, tréningy),
- študenti, ktorí chcú pravidelne chodiť na prednášky, do knižnice alebo na krúžky,
- ktokoľvek, kto chce viac disciplíny a nechce si nič zapisovať ručne.

## 3. Pojmy

| Pojem | Význam |
|---|---|
| **Miesto** | Bod na mape s názvom a polomerom zóny (napr. „Gym“, 150 m). |
| **Zóna** | Kruh okolo miesta. Ak je telefón v zóne, používateľ je „na mieste“. |
| **Pravidlo** | Čo chce používateľ na mieste dosiahnuť: kedy tam má prísť, ako dlho tam má byť a ako často. Každé miesto má práve jedno pravidlo. |
| **Návšteva** | Obdobie od vstupu do zóny po odchod zo zóny. |
| **Platná návšteva** | Návšteva, ktorá spĺňa podmienky pravidla (čas príchodu a minimálnu dĺžku pobytu). |
| **Deň** | Kalendárny deň 0:00 – 23:59 v časovej zóne telefónu. |
| **Týždeň** | Pondelok – nedeľa. |

## 4. Funkcie

Číslovanie zodpovedá user stories v [plan.md](plan.md).

**1. Správa miest na mape**
- **1.1** Používateľ pridá nové miesto dlhým podržaním prsta na mape a zadá jeho názov.
- **1.2** Používateľ nastaví polomer zóny okolo miesta a vidí ho na mape ako kruh.
- **1.3** Používateľ vidí zoznam všetkých svojich miest.
- **1.4** Používateľ upraví názov, polohu alebo polomer miesta, prípadne miesto zmaže.

**2. Pravidlá rutiny**
- **2.1** Používateľ nastaví čas, do ktorého má byť na mieste (napr. pred 8:00).
- **2.2** Používateľ nastaví minimálnu dĺžku pobytu, aby sa návšteva rátala (napr. 45 minút).
- **2.3** Používateľ nastaví, v ktoré dni alebo koľkokrát za týždeň chce miesto navštíviť.

**3. Automatické zaznamenávanie návštev**
- **3.1** Aplikácia na pozadí sama zaznamená príchod na miesto a odchod z neho.
- **3.2** Zaznamenávanie funguje aj po reštarte telefónu bez toho, aby používateľ otvoril aplikáciu.
- **3.3** Používateľ vidí históriu svojich návštev zoskupenú podľa dní.

**4. Vyhodnotenie a zhrnutie**
- **4.1** Používateľ vidí denné zhrnutie, ktoré pravidlá dnes splnil, ktoré nie a ktoré ešte čakajú.
- **4.2** Používateľ vidí týždenné zhrnutie s percentom úspešnosti za každé miesto aj celkovo.
- **4.3** Používateľ dostane večer notifikáciu so zhrnutím dňa.

**5. Pripomienky**
- **5.1** Používateľ dostane pripomienku pred termínom, ak ešte nie je na mieste.
- **5.2** Používateľ nastaví, koľko minút vopred ho má aplikácia upozorniť, alebo pripomienku pre miesto vypne.

**6. Onboarding a povolenia**
- **6.1** Pri prvom spustení používateľ vidí, čo aplikácia robí a prečo potrebuje jeho polohu.
- **6.2** Aplikácia navedie používateľa k povoleniu polohy „Povoliť vždy“ a notifikácií.
- **6.3** Ak povolenie chýba alebo je vypnutá poloha, používateľ vidí upozornenie s tlačidlom na nápravu.

## 5. Rozhodnuté otázky

| Otázka | Rozhodnutie |
|---|---|
| **R1** Čo presne znamená „bol som na mieste“? | Telefón vstúpil do zóny a zostal v nej aspoň **5 minút**. Kratšie návštevy (napr. prechod okolo) sa neukladajú. |
| **R2** Čo ak GPS na chvíľu „vyskočí“ zo zóny? | Ak sa používateľ vráti do zóny do **10 minút** od odchodu, ide stále o tú istú návštevu. |
| **R3** Ku ktorému dňu patrí návšteva cez polnoc? | K dňu, v ktorom **začala**. |
| **R4** Musí mať pravidlo nastavený čas aj dĺžku? | Nie. Obe podmienky sú voliteľné. Ak nie je nastavená ani jedna, stačí akákoľvek návšteva (R1). |
| **R5** Kedy je návšteva platná? | Ak začala **najneskôr** v nastavený čas príchodu (ak je nastavený) **a** trvala aspoň minimálnu dĺžku (ak je nastavená). Pri prebiehajúcej návšteve sa dĺžka ráta po aktuálny čas, takže pravidlo sa môže splniť, kým je používateľ ešte na mieste. |
| **R6** Aké typy plánu existujú? | **A – Konkrétne dni:** používateľ vyberie dni Po – Ne a v každý vybraný deň má byť na mieste. **B – X-krát týždenne:** cieľ 1 – 7 návštev za týždeň v ľubovoľné dni. |
| **R7** Koľko návštev sa ráta za jeden deň? | Najviac **jedna** platná návšteva na miesto za deň. Dve návštevy posilňovne v jeden deň sa pri pláne B neratajú ako 2. |
| **R8** Aké stavy môže mať pravidlo v daný deň? | **Splnené:** dnes existuje platná návšteva. **Čaká:** ešte sa to dá stihnúť (čas príchodu neuplynul, alebo používateľ je práve na mieste a dobieha mu dĺžka, alebo pravidlo nemá čas príchodu a deň ešte neskončil). **Nesplnené:** už sa to nedá stihnúť. **Nenaplánované:** pri pláne A dnes nie je vybraný deň. Pri pláne B sa namiesto toho zobrazuje priebeh týždňa, napr. „2 / 3“. |
| **R9** Ako sa počíta týždenná úspešnosť? | **Plán A:** splnené naplánované dni / naplánované dni. **Plán B:** min(dni s platnou návštevou, cieľ) / cieľ. **Celkovo:** súčet splnených / súčet požadovaných cez všetky miesta, zaokrúhlené na celé percentá. Rozpracovaný týždeň sa ukazuje priebežne. |
| **R10** Mení zmena pravidla históriu? | Nie. Zmena platí od **aktuálneho dňa**. Uzavreté dni (včera a skôr) zostávajú tak, ako boli vyhodnotené. |
| **R11** Kedy príde pripomienka? | Len ak má pravidlo nastavený čas príchodu, pripomienky sú pre miesto zapnuté, dnes je pravidlo relevantné (plán A: vybraný deň; plán B: týždenný cieľ ešte nie je splnený), používateľ práve nie je na mieste a dnes ešte nemá platnú návštevu. Predvolene **30 minút** pred časom príchodu, nastaviteľné 5 – 120 minút. |
| **R12** Kedy príde denné zhrnutie? | Každý deň o **21:00**, ak existuje aspoň jedno miesto. |
| **R13** Aký môže byť polomer zóny? | **100 – 500 m**, predvolene **150 m**. Menší polomer by bol kvôli presnosti polohy nespoľahlivý. |
| **R14** Koľko miest môže mať používateľ? | Najviac **100** (systémový limit geofencingu na aplikáciu). |
| **R15** Čo sa stane po zmazaní miesta? | Po potvrdení sa zmaže miesto, jeho pravidlo, všetky jeho návštevy aj história vyhodnotení. |

## 6. Akceptačné kritériá

Kritériá slúžia ako testovací plán. Polohu je možné simulovať v Android emulátore (Extended controls → Location).

**1. Správa miest**
- **K1.1-a** Po dlhom podržaní na mape sa otvorí formulár nového miesta so súradnicami podržaného bodu.
- **K1.1-b** Miesto bez názvu nie je možné uložiť. Uložené miesto sa zobrazí na mape ako značka.
- **K1.2-a** Polomer sa dá nastaviť iba v rozsahu 100 – 500 m, predvolená hodnota je 150 m.
- **K1.2-b** Pri zmene polomeru sa kruh na mape okamžite prekreslí.
- **K1.3-a** Zoznam obsahuje všetky miesta s názvom a stručným popisom pravidla (napr. „pred 8:00 · 45 min · Po, St, Pi“).
- **K1.4-a** Po úprave polohy alebo polomeru sa zaznamenávanie riadi novou zónou.
- **K1.4-b** Zmazanie vyžaduje potvrdenie. Po zmazaní miesto zmizne z mapy, zo zoznamu, z histórie aj zo zhrnutí (R15).

**2. Pravidlá**
- **K2.1-a** Čas príchodu sa vyberá v 24-hodinovom formáte a dá sa aj zrušiť (pravidlo bez času).
- **K2.2-a** Minimálna dĺžka sa dá nastaviť na 5 – 600 minút alebo zrušiť.
- **K2.3-a** Pri pláne A sa nedá uložiť pravidlo bez vybraného dňa. Pri pláne B sa dá zvoliť cieľ 1 – 7.
- **K2.3-b** Zmena pravidla sa prejaví v dnešnom zhrnutí hneď po uložení a nezmení vyhodnotenie predchádzajúcich dní (R10).

**3. Zaznamenávanie návštev**
- **K3.1-a** Keď simulovaná poloha vstúpi do zóny, najneskôr do 5 minút sa v aplikácii zobrazí prebiehajúca návšteva s časom príchodu.
- **K3.1-b** Po opustení zóny sa návšteva uzavrie s časom odchodu a dĺžkou pobytu.
- **K3.1-c** Návšteva kratšia ako 5 minút sa v histórii neobjaví (R1).
- **K3.1-d** Odchod a návrat do 10 minút vytvorí jednu návštevu, nie dve (R2).
- **K3.1-e** Zaznamenávanie funguje, aj keď je aplikácia zatvorená (odstránená zo zoznamu posledných aplikácií).
- **K3.2-a** Po reštarte telefónu sa vstup do zóny zaznamená bez otvorenia aplikácie.
- **K3.3-a** História zobrazuje návštevy zoskupené podľa dní, najnovšie hore, s názvom miesta, časom príchodu, odchodu a dĺžkou.

**4. Vyhodnotenie a zhrnutie**
- **K4.1-a** Pravidlo „pred 8:00, aspoň 45 min“: príchod 7:40 a odchod 8:30 → *Splnené*.
- **K4.1-b** To isté pravidlo: príchod 8:10 → *Nesplnené*.
- **K4.1-c** To isté pravidlo: príchod 7:40 a odchod 8:00 (20 min) → *Nesplnené*.
- **K4.1-d** To isté pravidlo o 7:00 bez návštevy → *Čaká*. O 8:01 bez návštevy → *Nesplnené*.
- **K4.1-e** Plán A s vybranými dňami Po, St, Pi: v utorok je pravidlo *Nenaplánované*.
- **K4.2-a** Plán B s cieľom 3 a platnými návštevami v Po a Ut → týždeň ukazuje 2 / 3 (67 %).
- **K4.2-b** Plán B s cieľom 3 a 5 platnými návštevami → 100 %, nie viac (R9).
- **K4.2-c** Celková úspešnosť zodpovedá výpočtu podľa R9.
- **K4.3-a** O 21:00 (± 15 min) príde notifikácia s počtom splnených pravidiel, napr. „Dnes 2 z 3 ✓“. Kliknutie otvorí denné zhrnutie.

**5. Pripomienky**
- **K5.1-a** Pravidlo „pred 8:00“, pripomienka 30 min, používateľ nie je na mieste → o 7:30 (± 15 min) príde notifikácia s názvom miesta.
- **K5.1-b** Ak je používateľ o 7:30 už na mieste, pripomienka nepríde.
- **K5.1-c** Pri pláne A v nevybraný deň a pri pláne B so splneným týždenným cieľom pripomienka nepríde.
- **K5.2-a** Po vypnutí pripomienok pre miesto už pre toto miesto žiadna nepríde. Zmena počtu minút sa prejaví od nasledujúcej pripomienky.

**6. Onboarding a povolenia**
- **K6.1-a** Onboarding sa zobrazí iba pri prvom spustení. Po jeho dokončení sa aplikácia otvára na hlavnej obrazovke.
- **K6.2-a** Onboarding postupne požiada o polohu, potom o polohu na pozadí (presmerovaním do nastavení) a nakoniec o notifikácie.
- **K6.2-b** Ak používateľ niektoré povolenie odmietne, onboarding sa dá dokončiť a aplikácia nespadne.
- **K6.3-a** Ak chýba povolenie „Povoliť vždy“ alebo je vypnutá poloha telefónu, hlavná obrazovka zobrazí upozornenie s tlačidlom, ktoré otvorí príslušné nastavenia.
- **K6.3-b** Po udelení povolenia upozornenie zmizne a zóny sa znova zaregistrujú.

## 7. Obmedzenia

- Platforma: **Android 10 (API 29) a novší**, iba telefóny, orientácia na výšku.
- Technológie: **Kotlin + Jetpack Compose**. Mapa cez Google Maps SDK, preto zariadenie potrebuje Google Play Services.
- Jazyk rozhrania: **slovenčina**, 24-hodinový formát času, vzdialenosti v metroch.
- Dáta sú uložené **iba lokálne** v telefóne. Internet je potrebný len na načítanie mapových podkladov.
- Poloha sa sleduje iba cez **geofencing**, nie nepretržitým GPS, aby sa šetrila batéria. Akceptuje sa presnosť zóny približne 100 m a oneskorenie udalostí niekoľko minút.
- Pripomienky a zhrnutia môžu prísť s odchýlkou do 15 minút kvôli úspornému režimu systému.

## 8. Mimo rozsahu (verzia 1)

| Mimo rozsahu | Prečo |
|---|---|
| Sledovanie trasy (napr. celý beh po GPS) | Vyžaduje nepretržité GPS, výrazne míňa batériu. Stačí overiť pobyt v oblasti. |
| Používateľské účty a synchronizácia do cloudu | Vyžaduje backend a ochranu citlivých údajov o polohe. |
| Sociálne funkcie (zdieľanie, rebríčky, priatelia) | Nie sú potrebné na riešenie hlavného problému a vyžadujú server. |
| Verzia pre iOS | Predmet je zameraný na Android. |
| Grafy, dlhodobé štatistiky, série (streaky) | Rozšírenie, MVP funguje aj bez nich. |
| Integrácia s Google Fit alebo kalendárom | Zvyšuje počet závislostí a povolení. |
| Widget na domovskú obrazovku | Kozmetické rozšírenie. |
| Ručné pridanie alebo úprava návštevy | Pozri otvorenú otázku O4. |
| Viac pravidiel pre jedno miesto | Pozri otvorenú otázku O5. |

## 9. Otvorené otázky

| Otázka | Návrh riešenia |
|---|---|
| **O1** Ak je telefón vybitý alebo má vypnutú polohu, má sa deň rátať ako nesplnený, alebo sa má vynechať? | Rátať ako nesplnený. Aplikácia nevie spoľahlivo rozlíšiť, prečo návšteva chýba. |
| **O2** Čo s návštevou, pri ktorej nikdy nepríde odchod (napr. telefón sa vypol na mieste)? | Pri dennom vyhodnotení ju uzavrieť o 23:59 dňa, v ktorom začala. |
| **O3** Má byť čas denného zhrnutia nastaviteľný? | V MVP nie, pevne 21:00. |
| **O4** Má sa dať návšteva pridať ručne, ak GPS zlyhá? | V MVP nie. Ručné zápisy by oslabili automatické overovanie. |
| **O5** Má mať jedno miesto viac pravidiel (napr. iný čas cez víkend)? | V MVP nie. Ako náhradné riešenie stačí vytvoriť druhé miesto na rovnakom bode. |
