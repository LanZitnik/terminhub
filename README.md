# TerminHub

Spletna aplikacija za rezervacijo športnih igrišč in terminov. Uporabniki poiščejo igrišče (nogomet, košarka, tenis, badminton, odbojka), pogledajo proste termine in jih rezervirajo. Upravljavci objektov urejajo igrišča in termine ter spremljajo rezervacije.

V tej fazi projekta je izdelan samo **čelni del** (Angular). Zaledni del (Node.js), podatkovna baza, prava prijava in avtorizacija so **načrtovani**, še ne implementirani.

## Problem, ki ga aplikacija rešuje

Rezervacije igrišč se pogosto dogajajo po telefonu ali e-pošti. Pregleda nad prostimi termini ni, prihaja do dvojnih rezervacij in težav pri usklajevanju. TerminHub vse to združi na enem mestu.

## Ciljna skupina

Rekreativci, športna društva, ekipe, šole in upravljavci športnih objektov.

## Načrtovane funkcionalnosti

**Avtentikacija:** registracija, prijava, odjava.

**Vsebinske funkcionalnosti:**
1. Pregled in filtriranje igrišč (po športu, lokaciji, ceni)
2. Rezervacija termina (izbira igrišča, datuma in ure)
3. Moje rezervacije (pregled in preklic)
4. Administracija igrišč in terminov (dodajanje, urejanje, brisanje)
5. Pregled vseh rezervacij (administrator)

**Informacijske strani:** O nas, Kontakt.

## Uporabniške vloge in pravice

| Vloga | Pravice |
|---|---|
| Gost | Ogled igrišč, informacijske strani, registracija, prijava |
| Uporabnik | Vse kot gost + rezervacija, pregled in preklic lastnih rezervacij, urejanje profila |
| Administrator | Vse kot uporabnik + upravljanje igrišč, terminov, uporabnikov in vseh rezervacij |

## Podatkovni model (načrt)

- **User:** id, ime, priimek, e-pošta, geslo (hash), vloga
- **Sport:** id, naziv
- **Facility (igrišče):** id, naziv, opis, lokacija, cena na uro, slika, sportId
- **TimeSlot (termin):** id, facilityId, datum, začetek, konec, prost (da/ne)
- **Booking (rezervacija):** id, userId, timeSlotId, status (aktivna/preklicana), čas nastanka

**Povezave:** Sport 1:N Facility, Facility 1:N TimeSlot, User 1:N Booking, TimeSlot 1:1 Booking.

## Zasloni (prikazi) in poti

| Pot | Prikaz |
|---|---|
| `/` | Domača stran |
| `/igrisca` | Seznam igrišč s filtri |
| `/igrisca/:id` | Podrobnosti igrišča in izbira termina |
| `/prijava` | Obrazec za prijavo |
| `/registracija` | Obrazec za registracijo |
| `/moje-rezervacije` | Rezervacije uporabnika |
| `/admin` | Nadzorna plošča administratorja |
| `/o-nas` | Informacije o aplikaciji |
| `/kontakt` | Kontaktni podatki |

Osnutki zaslonov (namizna in mobilna različica): [`docs/TerminHub_osnutki_zaslonov.pdf`](docs/TerminHub_osnutki_zaslonov.pdf)

## Angular komponente

### Hierarhija

```
AppComponent
├── HeaderComponent
├── <router-outlet>
│   ├── HomePageComponent
│   │   ├── HeroComponent
│   │   └── FacilityCardComponent (×3)
│   ├── FacilitiesPageComponent
│   │   ├── PageTitleComponent
│   │   ├── FilterBarComponent
│   │   └── FacilityCardComponent (×več)
│   ├── FacilityDetailPageComponent
│   │   ├── PageTitleComponent
│   │   └── TimeSlotComponent (×več)
│   ├── LoginPageComponent
│   ├── RegisterPageComponent
│   ├── MyBookingsPageComponent
│   │   ├── PageTitleComponent
│   │   └── BookingItemComponent (×več)
│   ├── AdminPageComponent
│   │   ├── PageTitleComponent
│   │   └── BookingItemComponent (×več)
│   ├── AboutPageComponent
│   │   └── PageTitleComponent
│   └── ContactPageComponent
│       └── PageTitleComponent
└── FooterComponent
```

### Seznam in opis

| Komponenta | Opis |
|---|---|
| HeaderComponent | Logotip, navigacija, gumba Prijava in Registracija |
| FooterComponent | Povezave, kontakt, avtorske pravice |
| HomePageComponent | Domača stran: hero in kartice priljubljenih igrišč |
| HeroComponent | Uvodni razdelek z naslovom in gumbom |
| FacilityCardComponent | Kartica igrišča: slika, naziv, lokacija, cena (**večkrat uporabljena**) |
| FacilitiesPageComponent | Seznam igrišč s filtri |
| FilterBarComponent | Filtri po športu, lokaciji, ceni |
| PageTitleComponent | Naslov strani z opisom (**večkrat uporabljena**) |
| FacilityDetailPageComponent | Podrobnosti igrišča, izbira datuma in termina |
| TimeSlotComponent | En termin: ura, prost/zaseden, gumb Rezerviraj (**večkrat uporabljena**) |
| LoginPageComponent | Obrazec za prijavo |
| RegisterPageComponent | Obrazec za registracijo |
| MyBookingsPageComponent | Rezervacije prijavljenega uporabnika |
| BookingItemComponent | Ena rezervacija z gumbom Prekliči (**večkrat uporabljena**) |
| AdminPageComponent | Tabela igrišč, rezervacije, urejanje |
| AboutPageComponent | Informacije o aplikaciji |
| ContactPageComponent | Kontaktni podatki in obrazec |

### Struktura map

```
src/app/
├── layout/   (header, footer)
├── pages/    (home-page, facilities-page, facility-detail-page, login-page,
│              register-page, my-bookings-page, admin-page, about-page, contact-page)
└── shared/   (hero, facility-card, filter-bar, page-title, time-slot, booking-item)
```

## Uporabljene tehnologije

- **Frontend:** Angular, TypeScript, HTML, CSS
- **UI (5. teden):** Angular Material (toolbar, button, card, form field/input, select, datepicker, tabs, table)
- **Ikone (5. teden):** Font Awesome prek `@fortawesome/angular-fontawesome` (uvoz v TypeScriptu, prikaz z `<fa-icon>`)
- **Orodja:** Git/GitHub, Visual Studio Code, Figma ali papir (osnutki), Trello (načrt dela)
- **Predvideno za kasneje:** Node.js (Express), podatkovna baza (PostgreSQL ali MongoDB), JWT avtentikacija

## Navodila za namestitev in zagon

Zahteve: Node.js (LTS), npm, Angular CLI (`npm install -g @angular/cli`).

```bash
git clone <povezava-do-repozitorija>
cd terminhub
npm install
ng serve
```

Aplikacija je dostopna na `http://localhost:4200`.

## Organizacija dela

Naloge spremljam v orodju **Trello** (tabla *TerminHub*), stolpci: **Načrtovano → V delu → Zaključeno**. Vsaka kartica ima naslov, kratek opis in rok. Povezava ali posnetek table: `docs/trello.png`

## Tedenski načrt

| Teden | Vsebina | Rezultat |
|---|---|---|
| 1 | Načrtovanje, dokumentacija, osnutki, seznam komponent, Trello, začetni projekt | Dokumentiran načrt in začetni projekt |
| 2 | Komponente z Angular CLI, postavitev, routing, statična vsebina | Povezana osnovna struktura |
| 3 | Obrazci, statični prikazi, barvna shema, tipografija, odzivnost | Zasloni z enotnim videzom |
| 4 | Odprava podvajanja, skupne komponente, izboljšana odzivnost | Dodelan osnutek čelnega dela |
| 5 | Angular Material (vsaj 4 vrste), Font Awesome (vsaj 5 ikon) | Poenoten uporabniški vmesnik |
| 6 | Preverjanje, popravki, README, predstavitev, oddaja | Končna oddaja |

## Tedenski dnevnik

### 1. teden
- **Načrtovano:** izbira ideje, dokumentacija, podatkovni model, osnutki zaslonov, seznam komponent, Trello, Angular projekt
- **Izdelano:** izbrana ideja (TerminHub), napisan opis, funkcionalnosti, vloge in podatkovni model, izdelani osnutki 5 zaslonov (namizna in mobilna različica), pripravljen seznam in hierarhija 17 komponent, ustvarjena Trello tabla, ustvarjen Angular projekt in repozitorij
- **Težave:** *(dopišite, če so bile, npr. namestitev orodij, povezava z GitHubom)*
- **Sledi:** 2. teden: generiranje komponent, osnovna postavitev in routing

### 2. teden
*(izpolnite med delom)*

### 3. teden
*(izpolnite med delom)*

### 4. teden
*(izpolnite med delom)*

### 5. teden
*(izpolnite med delom)*

### 6. teden
*(izpolnite med delom)*

## Neimplementirane funkcionalnosti in nadaljnji razvoj

Spodnje funkcionalnosti so načrtovane, vendar v tej vaji niso izdelane:

- Node.js/Express strežnik in REST API
- Podatkovna baza in shranjevanje podatkov
- Prava registracija, prijava in odjava (JWT)
- Avtorizacija po vlogah (route guards)
- Preprečevanje dvojnih rezervacij
- E-poštna obvestila o rezervacijah
- Spletno plačevanje
- Dinamičen prikaz podatkov (`@for`, `@if`, `input()`)
