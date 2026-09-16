+++
title = "Cowrie honeypot tapasztalatok pár nap tesztelés után"
date = 2026-09-16
draft = false
language = "hu"
summary = "Cowrie SSH honeypot üzemeltetése éles VPS-en: mit csinál az internet egy nyitott 22-es porttal 72 óra alatt?"
+++

Régóta szerepelt a bakancslistámon egy honypot kipróbálása éles szerveren. Hétvégén belevágtam és felállítottam egy Cowrie SSH honeypotot egy éles VPS-en, és 72 órán át figyeltem, mi történik. Röviden: az internet nem sokat várt.

## Mi az a honeypot?

A Cowrie egy SSH csapda: úgy viselkedik, mint egy valódi Linux szerver, elfogadja a bejelentkezési kísérleteket, hamis shell promptot ad, és mindent naplóz - a jelszavaktól kezdve a futtatott parancsokig és a feltöltött fájlokig. A támadó azt hiszi, bejutott. Valójában egy homokozóban van.

## Az infrastruktúra

- **Szolgáltató:** egy GDPR-kompatibilis VPS szolgáltató
- **OS:** Debian 12
- **Honeypot port:** 22 (az SSH daemon áthelyezve 2222-re)
- **Eszköz:** Cowrie 3.0.13, authbind-dal, izolált `cowrie` felhasználó alatt

A valódi SSH-t használat előtt még egy kicsit körbebástyáztam: kulcsalapú belépés, root login tiltva, ufw tűzfal, fail2ban a 2222-es portra.

## Mit mutattak a számok?

72 óra alatt a honeypot **55 528 eseményt** rögzített, **341 egyedi forrás IP**-ről, **7 305 SSH kapcsolatban**. A sikeres "bejelentkezések" aránya 99,4% volt - a Cowrie szándékosan engedékeny, hogy a támadók "bejussanak" és megmutassák, mit csinálnának.

Az összes kapcsolat közel 80%-a egyetlen `/24`-es alhálózatból érkezett (`109.160.32.*`) - kilenc különböző IP-cím, valószínűleg egyetlen botnet-operátor, koordináltan párhuzamos szkennelést futtatva.

## A leggyakrabban próbált jelszavak

A lista nem meglepő: `123456`, `123`, `1234`, `root`, `admin`, `password`. Ezek automatizált szótár-támadások, nem emberi kéz írja őket.

Érdekesebb a felhasználónevek listája: `root` mellett feltűnik a `bhwang`, `aagorban`, `ansible` (érdekes története van ezeknek a neveknek, egy keresést megér). Az utóbbi az infrastruktúra-automatizálást célozza, az első kettő valószínűleg kiszivárgott hitelesítő adatok alapján fut.

## A támadási lánc

Egy jellemző sikeres munkamenet így nézett ki:

1. **Bejelentkezés** - `root` / `r@@t` vagy hasonló
2. **OS azonosítás** - `uname -s -v -n -r -m` (6 080-szor futtatták összesen)
3. **SSH backdoor telepítése** - a bot bejegyzi saját publikus kulcsát az `authorized_keys`-be, hogy később jelszó nélkül vissza tudjon térni
4. **Lateral movement kit** - privát kulcs + SSH config feltöltése, amivel a fertőzött gép maga is továbbterjed más szerverekre (ha a tűzfal engedé)
5. **Cryptominer payload** - `sshd` nevű fájl SFTP-vel feltöltve, valójában egy Linux CoinMiner bináris

A 3. pont az ún. *mdrfckr kampány* - ez egy jól dokumentált, tömeges SSH-backdoor terjesztési kampány, ahol ugyanaz a publikus kulcs jelenik meg rengeteg különböző IP-ről.

## A VirusTotal megerősíti

A két legérdekesebb letöltött fájlt feltöltöttem a VirusTotalra:

- **SSH backdoor kulcs** (`a8460f44...`) - 32/61 vendor jelölte kártékonynak, `Trojan.Shell.Malkey` / `authorized_keys` kategóriában
- **Trójai sshd bináris** (`94f2e4d8...`) - 46/63 vendor jelölte kártékonynak, `miner.multiverze` / `Linux/CoinMiner` kategóriában, ELF 64bit futtatható

A terv tehát: bejut, hátsó ajtót nyit, lecseréli az SSH daemonnál egy cryptominerre, és a szerver CPU-ján kriptovalutát bányász - miközben a folyamatlista `sshd`-t mutat.

## Interaktív dashboard

Az összes adat vizualizálva elérhető itt: [Interaktív dashboard megnyitása →](/static/cowrie_dashboard.html)

## Tanulságok

Egy nyilvános IP-n lévő 22-es port perceken belül megjelenik a scannerek radarján. Az automatizált támadások könyörtelenek és állandóak - nem személyes célpontok, hanem tömeges, olcsó permetezés. A védekezés nem bonyolult: kulcsalapú SSH-hitelesítés, nem szokványos port, tűzfal - de ezeket tényleg be kell állítani, nem elég tudni róluk.


---

*A projekt során használt eszközök: Claude AI, Cowrie 3.0.13, Debian 12, Python 3, VirusTotal. A honeypot logok és az elemző script elérhető a [GitHub repómban](https://github.com/probbi).*
