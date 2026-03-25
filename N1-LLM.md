# Specifikacija zahtev

## Informacijski sistem za program lojalnosti Maestro

## Dokumentni podatki

| Polje                  | Vrednost                 |
| ---------------------- | ------------------------ |
| Oznaka dokumenta       | N1-LLM                   |
| Verzija                | 1.1                      |
| Status                 | Delovni osnutek          |
| Datum izdaje           | 2026-03-18               |
| Datum zadnje spremembe | 2026-03-25               |
| Jezik                  | Slovenščina              |
| Vir zahtev             | 01-Program-lojalnosti.md |
| Avtor                  | Martin Hovnik            |

## Zgodovina verzij

| Verzija | Datum      | Opis spremembe                                                                     |
| ------- | ---------- | ---------------------------------------------------------------------------------- |
| 1.0     | 2026-03-18 | Pripravljena začetna strukturirana specifikacija zahtev.                           |
| 1.1     | 2026-03-18 | Dodani naslovnica, dokumentni metapodatki, status in formalna struktura dokumenta. |
| 1.2     | 2026-03-25 | Dodani diagrami sistema in slovar izrazov.                                         |

## Namen dokumenta

Dokument opredeljuje funkcionalne in nefunkcionalne zahteve, omejitve, vmesnike ter terminologijo za rešitev, ki podpira program lojalnosti trgovske verige Maestro.

## Obseg dokumenta

Dokument zajema zahteve, ki izhajajo iz izhodiščnega opisa problema in nalog v dokumentu 01-Program-lojalnosti.md.

---

# 1. Kratek opis sistema

Sistem podpira program lojalnosti trgovske verige Maestro. Namen sistema je povečati ponovne nakupe in dolgoročno zvestobo strank z dodeljevanjem točk ter upravljanjem statusov lojalnosti (osnovni, bronasti, srebrni, zlati).  
Strankam omogoča registracijo, pregled in koriščenje točk ter vpogled v nakupe, podjetju pa centralizirano upravljanje pravil, nagrad, statistik in statusov.

---

# 2. Funkcionalne zahteve

Oznake prioritet: **[M] Must**, **[S] Should**, **[C] Could**, **[W] Won't (v tej fazi)**.

## 1. Upravljanje članstva in računov

- [M] Sistem mora omogočiti vključitev stranke v program lojalnosti.
- [M] Sistem mora omogočiti spletno registracijo z vnosom osebnih podatkov.
- [M] Sistem mora zagotoviti varno preverjanje e-naslova ob registraciji (zahteva: preprečiti registracijo z ne-lastniškim e-naslovom).
- [M] Ob registraciji mora sistem ustvariti uporabniški račun za prijavo v portal.
- [S] Ob uspešni vključitvi mora sistem podpreti izdajo kartice lojalnosti in pripravo pošiljanja po navadni pošti.
- [M] Ob včlanitvi mora stranka dobiti začetni status **osnovni**.

## 2. Nivoji lojalnosti

- [M] Sistem mora podpirati nivoje: **osnovni, bronasti, srebrni, zlati**.
- [C] Sistem mora omogočati možnost kasnejše spremembe delitve na nivoje (konfigurabilnost nivojev).

## 3. Mesečni izračun točk zvestobe

- [M] Sistem mora 1x mesečno izračunati točke za pretekli mesec.
- [M] Sistem mora za izračun uporabiti zneske nakupov iz poslovnega informacijskega sistema trgovske verige.
- [M] Sistem mora dodeliti točke glede na zneskovni razred nakupov in status stranke.
- [M] Sistem mora omogočiti kasnejše spreminjanje pravil/vrednosti točkovnika.

### 3.1 Točkovnik

| Znesek nakupov v mesecu | Osnovni | Bronasti | Srebrni | Zlati |
| ----------------------- | ------: | -------: | ------: | ----: |
| do 200 EUR              |       5 |        0 |     7.5 |    10 |
| med 200 EUR in 1000 EUR |      10 |        5 |      15 |    20 |
| nad 1000 EUR            |      20 |       10 |      30 |    40 |

**Predpostavka A1:** zaradi mej v opisu se razredi interpretirajo kot:

- `znesek <= 200`,
- `200 < znesek <= 1000`,
- `znesek > 1000`.

## 4. Pravila prehajanja med statusi

- [M] Ob včlanitvi: status je **osnovni**.
- [M] Ko stranka prvič preseže 499 EUR mesečnih nakupov, preide v **srebrni** status.
- [M] Če nato še dvakrat preseže ta prag (>500), preide v **zlati** status.
- [M] Za ohranjanje **srebrnega** statusa mora imeti vsaj 200 EUR mesečnih nakupov.
- [M] Za ohranjanje **zlatega** statusa mora imeti vsaj 500 EUR mesečnih nakupov.
- [M] Če ne izpolni pogoja za **zlati**, se status zniža na **srebrni**.
- [M] Če ne izpolni pogoja za **srebrni** dva zaporedna meseca, preide v **bronasti** status.
- [M] V **bronastem** statusu ostane, dokler:
  - dva zaporedna meseca ne opravi vsaj 200 EUR nakupov, **ali**
  - če opravi nakup pod 50 EUR, preide nazaj v **osnovni** status.
- [M] Pri mesečni obdelavi mora sistem **najprej posodobiti status** in **šele nato** dodeliti točke.

**Predpostavka A2:** izraza `>499` in `>500` se obravnavata kot prag preseganja 500 EUR.

## 5. Portal za člane

Portal mora članom omogočati:

- [M] pregled zbranih točk zvestobe,
- [M] koriščenje (unovčevanje) točk,
- [S] pregled nakupnega programa,
- [S] pregled zneskov nakupov.

## 6. Administracija

Administratorski del mora omogočati:

- [M] pregled statusov strank za poljubno obdobje,
- [M] pregled statistike nakupov,
- [W] poljubne poizvedbe po podatkovni bazi,
- [M] upravljanje programa nagrad, ki je na voljo za točke,
- [M] upravljanje pravil prehajanja med statusi in nagrajevanja (predvsem spreminjanje vrednosti).

---

# 3. Nefunkcionalne zahteve

Oznake prioritet: **[M] Must**, **[S] Should**, **[C] Could**, **[W] Won't (v tej fazi)**.

## 1. Zmogljivost in kapaciteta

- [S] Sistem mora podpirati vsaj ~500.000 članov (vsaj 70 % strank).
- [S] Arhitektura mora omogočati bistveno večje število uporabnikov (širitev izven Slovenije).
- [M] Mesečni obračun točk mora biti izvedljiv nad celotno bazo članov.

## 2. Varnost

- [M] Registracija mora vključevati varen mehanizem potrditve lastništva e-poštnega naslova.
- [M] Uporabniški računi morajo omogočati identifikacijo uporabnikov pri prijavi v portal.

## 3. Razširljivost in vzdrževanje

- [M] Pravila točkovanja in statusnih prehodov morajo biti nastavljiva brez spremembe koncepta sistema.
- [C] Sistem mora podpirati morebitno spremembo delitve statusnih nivojev.

## 4. Uporabniška izkušnja

- [S] Uporabniški vmesnik mora biti intuitiven.
- [S] Uporabljene morajo biti sodobne tehnologije.

## 5. Lokalizacija

- [S] Celotna rešitev mora podpirati dva jezika: **slovenščino** in **angleščino**.

**Predpostavka A3:** podpora jezikom velja za članski in administratorski del portala.

---

# 4. Omejitve

- Podatkovna baza mora biti **Oracle** (obstoječe licence v podjetju).
- Izračun točk je periodičen in vezan na mesečni cikel (1x mesečno za pretekli mesec).
- Sistem je odvisen od podatkov o nakupih iz obstoječega poslovnega IS.
- Kartica lojalnosti se pošilja po navadni pošti (operativna omejitev procesa).
- Pravila programa so v grobem statična; pričakovane so spremembe vrednosti, ne pa popolna sprememba logike.
- Eksplicitne pravne zahteve (npr. skladnost s predpisi varstva osebnih podatkov) v izvoru niso navedene.

---

# 5. Vmesniki

## 1. Podatkovni vmesnik do Oracle DB

- Branje/pisanje članov, statusov, točk, pravil, nagrad in analitičnih podatkov.

## 2. Vmesnik za potrjevanje e-pošte

- **Predpostavka A4:** potreben je integracijski mehanizem za pošiljanje/verifikacijo e-pošte ob registraciji.

## 3. Podpora procesu pošiljanja kartic

- **Predpostavka A5:** sistem mora zagotoviti podatke/izvoz za proces fizičnega pošiljanja kartic po pošti.

---

# 6. Diagrami sistema

## 6.1 Diagram razgradnje

<img src="shared/decomposition-diagram.png" alt="Decomposition Diagram" width="1500"/>

## 6.2 Diagram primerov uporabe

<img src="shared/use-case-diagram.png" alt="Use Case Diagram" width="900"/>

# 7. Slovar izrazov

| Izraz                   | Opis                                                                     |
| ----------------------- | ------------------------------------------------------------------------ |
| Program lojalnosti      | Sistem pravil in ugodnosti za nagrajevanje nakupne aktivnosti strank.    |
| Član programa           | Stranka, ki je vključena v program in ima kartico/račun.                 |
| Status lojalnosti       | Nivo člana (osnovni, bronasti, srebrni, zlati), ki vpliva na točkovanje. |
| Točke zvestobe          | Enote nagrajevanja, dodeljene na podlagi mesečnih nakupov in statusa.    |
| Mesečni obračun         | Periodični izračun točk za pretekli mesec.                               |
| Točkovnik               | Pravila za pretvorbo zneska nakupov + statusa v točke.                   |
| Prehod statusa          | Sprememba statusa člana glede na izpolnjevanje pragov nakupov.           |
| Poslovni IS             | Obstoječi informacijski sistem trgovske verige, vir podatkov o nakupih.  |
| Portal                  | Spletna aplikacija za člane in administratorje.                          |
| Unovčevanje točk        | Koriščenje zbranih točk za nagrade/ugodnosti.                            |
| Nakupni program         | Vsebine, povezane s ponudbo za člane (izraz iz izvornega besedila).      |
| Program nagrad          | Nabor nagrad/ugodnosti, ki so na voljo za točke.                         |
| Konfigurabilnost pravil | Možnost spreminjanja vrednosti pravil brez večjih sprememb sistema.      |
