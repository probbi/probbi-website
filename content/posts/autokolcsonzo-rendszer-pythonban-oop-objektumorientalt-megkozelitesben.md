+++
date = '2026-04-15T21:00:49+01:00'
draft = false
language = "hu"
title = 'Magyarországi CO₂‑kibocsátás elemzése KSH-adatok alapján'
summary = 'Egyetemi projektként készített Python autókölcsönző CLI alkalmazás az objektumorientált programozás (OOP) alapelveinek bemutatására.'
+++

# Autókölcsönző Rendszer Pythonban – OOP Objektumorientált Megközelítésben

A programozási alapok és az **objektumorientált programozás (OOP)** elveinek mélyebb megértéséhez az egyik legjobb gyakorlat egy klasszikus domain modell megvalósítása. Ebben a cikkben az általam készített **Autókölcsönző CLI (parancssori) alkalmazást** mutatom be.

##  A Projekt Célja

A feladat egy olyan Python alapú rendszer megtervezése volt, amely képes kezelti egy autókölcsönző flotta mindennapi műveleteit:
- Autók nyilvántartása és bérlése.
- Bérlések visszavonása/lemondása.
- Árkalkuláció és dátumalapú ütközésvizsgálat.

---

##  Architektúra és Osztályszerkezet

A projekt szigorúan követi az OOP alapelveit (öröklődés, egységbe zárás, absztrakció):

```text
       ┌──────────────┐
       │     Auto     │ (Absztrakt / Szülőosztály)
       └──────┬───────┘
              │
      ┌───────┴───────┐
      ▼               ▼
┌───────────┐   ┌───────────┐
│Szemelyauto│   │ Teherauto │
└───────────┘   └───────────┘

```
---

### Kapcsolódó hivatkozások

- **GitHub forráskód:** [probbi/autokolcsonzo](https://github.com/probbi/autokolcsonzo)
- **Projekt típusa:** Egyetemi feladat (OOP / Python)
