# Routine Maker

## Aký problém riešim

Ľudia si stanovujú rutiny (posilňovňa ráno, beh, chodenie do kancelárie), ale nemajú jednoduchý spôsob, ako automaticky sledovať, či ich naozaj dodržiavajú.

## Kto sú moji používatelia

- **Ľudia, ktorí si budujú zdravé návyky:** chcú chodiť pravidelne do posilňovne, behať alebo na tréningy a potrebujú vidieť, či to naozaj robia.
- **Študenti:** chcú pravidelne chodiť na prednášky, do knižnice alebo na krúžky.
- **Každý, kto chce viac disciplíny a nechce si všetko zapisovať ručne:** aplikácia zaznamenáva návštevy automaticky na základe polohy.

## Ako chcem problém vyriešiť (MVP)

Používateľ si na mape označí miesta svojej rutiny a nastaví si pre ne pravidlá. Aplikácia potom na pozadí pomocou geofencingu automaticky zaznamenáva návštevy a vyhodnocuje, či pravidlá splnil.

Funkcie MVP:

1. **Pridanie miesta na mape:** dlhým podržaním na mape používateľ vyberie bod a zadá názov (napr. „Gym“) a polomer zóny (napr. 100 m).
2. **Pravidlá pre miesto:**
   - *čas príchodu:* byť na mieste pred určitým časom (napr. pred 8:00),
   - *minimálna dĺžka pobytu:* napr. aspoň 45 minút,
   - *frekvencia:* koľkokrát za týždeň alebo v ktoré dni (napr. 3× týždenne, Po–Pi).
3. **Automatické zaznamenávanie návštev:** geofence udalosti (vstup a výstup) sa ukladajú do lokálnej databázy s časom príchodu a odchodu.
4. **Denné a týždenné zhrnutie:** prehľad splnených a nesplnených pravidiel a percento úspešnosti.
5. **Pripomienky:** notifikácia pred termínom, ak používateľ ešte nie je na mieste (napr. „Je 7:30 a ešte nie si v posilňovni“).
6. **Onboarding povolení:** obrazovka, ktorá vysvetlí, prečo aplikácia potrebuje polohu na pozadí, a navedie používateľa k jej povoleniu.

## Čo v MVP nebude (non-goals) a prečo

| Non-goal | Prečo nie |
|---|---|
| **Sledovanie trasy (napr. celý beh po GPS)** | Vyžaduje nepretržité sledovanie GPS cez foreground service, čo výrazne míňa batériu a komplikuje implementáciu. V MVP stačí overiť, že používateľ bol v oblasti (napr. v parku) dostatočne dlho. |
| **Používateľské účty a synchronizácia do cloudu** | Pridáva backend, autentifikáciu a riešenie ochrany osobných údajov (poloha je citlivý údaj). V MVP sú všetky dáta len lokálne v telefóne. |
| **Sociálne funkcie (zdieľanie, rebríčky, priatelia)** | Nie sú potrebné na riešenie hlavného problému a vyžadujú server. |
| **Verzia pre iOS** | Predmet je zameraný na Android. |
| **Pokročilé štatistiky a grafy, série (streaky)** | Sú to užitočné rozšírenia, ale MVP funguje aj bez nich. Doplnia sa, ak zostane čas. |
| **Integrácia s inými aplikáciami (Google Fit, kalendár)** | Mimo rozsahu MVP, zvyšuje závislosti a počet potrebných povolení. |
| **Widget na domovskú obrazovku** | Je to kozmetické rozšírenie, nie jadro aplikácie. |

## Aké technológie potrebujem

| Oblasť | Technológia |
|---|---|
| Jazyk | Kotlin |
| UI | Jetpack Compose (Material 3) |
| Mapa | Google Maps SDK pre Android (Maps Compose) |
| Poloha a geofencing | Google Play Services Location (`GeofencingClient`, `FusedLocationProviderClient`) |
| Lokálna databáza | Room (SQLite) |
| Úlohy na pozadí | WorkManager (denné a týždenné vyhodnotenie, plánovanie pripomienok) |
| Notifikácie | Android Notifications (NotificationManager, notifikačné kanály) |
| Architektúra | MVVM (ViewModel, StateFlow, Kotlin Coroutines) |
| Vývojové prostredie | Android Studio, Android Emulator (simulácia polohy) |
| Verziovanie | Git + GitHub |

### Hardvér a služby telefónu

- **GPS a sieťová lokalizácia (Wi-Fi, mobilná sieť):** určovanie polohy a detekcia vstupu do zóny a odchodu z nej.
- **Internetové pripojenie:** načítanie mapových podkladov.
- **Notifikácie a vibrácie:** pripomienky a zhrnutia.

### Potrebné povolenia

- `ACCESS_FINE_LOCATION`, `ACCESS_COARSE_LOCATION`
- `ACCESS_BACKGROUND_LOCATION` (geofencing na pozadí)
- `POST_NOTIFICATIONS` (Android 13+)
- `RECEIVE_BOOT_COMPLETED` (opätovná registrácia geofence zón po reštarte telefónu)
- `INTERNET`
