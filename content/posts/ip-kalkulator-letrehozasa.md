+++
date = '2026-02-10T18:03:26+01:00'
draft = false
title = 'IP kalkulator létrehozása'
summary = 'Szerettem volna egy böngészőben futtatható IP kalkulátort összehozni Python alapokon, mutatom mire jutottam.'
tags = ['python', 'halozat', 'fejlesztes', 'hugo']
+++
# IP kalkulátor létrehozása Pythonban

Már régebben is agyaltam azon, hogy jó lenne egy saját ip kalkulátor amit bármikor használhatnék egy böngészőből. Most jutottam el oda, hogy ezt már valószínűleg össze tudom hozni többé kevésbé magamtól is. Mivel a fejlesztéshez és működtetéshez szükséges környezet már adott volt csak neki kellett állnom a feladatnak. A program persze nem egy hiánypótló alkalmazás, -kismillió változata létezik már régóta, de nem is az a célom.

Mivel még tanulom a Pythont természetesen kértem egy kis AI segítséget is kód vázlatához amit persze a Gemini megoldott egy pillanat alatt. A számomra érdekes rész innen következik és amiért szeretem az AI-kat a tanulás kiegészítőjeként az az, hogy segít értelmezni, bármilyen könyvtárat, kódot ami szükséges a program futtatáshoz azonnal le tudok kérdezni, hogy miért arra van szükség, mit csinál és miért úgy.

A kódot pl. VS Code-ban futtatva, localhoston egy böngészőben valós időben lehet tesztelgetni, állítgatni ami nagyon hatékony tanulási módszer a számomra.

## Célok

- megérteni, hogy mi kell ahhoz hogy egyáltalán működjön egy ilyen szkript a böngészőben
- egy olyan publikus kalkulátor elkészítése, amivel hálózattervezéskor könnyen és gyorsan lehet hálózati címkiosztásokat kiszámolni

## Amire szükségem volt:

- a Python ipaddress könyvtárára
- a Streamlit-re a kezelőfelülethez ami ráadásul gyors és látványos megjelenítést garantál

Szóval a következő órák azzal teltek, hogy hozzáadtam vagy elvettem funkciókat vagy éppen időlegesen használhatatlanná tettem a nagy fejlesztésemet. Viszonylag hamar kész lett amit szerettem volna elérni -egy alap kalkulátor de a kihívás másik része a Github, Cloudflare, Nginx, HTTPS kombó összehozása volt. Miután ezt sikerül megoldanom még mindig nem akartam kiengedni a netre, mert és finomítani szerettem volna a program logikáján és a vizualizáción.

![](https://pub-91da716c25164a4a9bb1755980b809e1.r2.dev/IP-kalkulator.webp)

## Összegezve

- Sikerült elérnem, hogy az adatok (Hálózat, Maszk, Broadcast) azonnal megjelenjenek.
- Szerettem volna bináris számokkal is szemléltetni kikalkulált eredményeket ami színekkel (kék és zöld) választja szét a hálózati és a host biteket. Ez segített megérteni, mi történik a "gépházban", amikor elmozdítom a maszkot.
- Az alhálózati tervező része a programnak egy dinamikus táblázaton,  kilistázza az alhálózatokat és megmutatja a konkrét használható IP tartományokat is (pl. iroda vs. teamszoba).Felkészítettem az appot a "való életre". 
- Haa felhasználó elfelejt maszkot megadni, vagy elgépeli az az IP-t, az alkalmazás nem omlik össze, hanem segítőkész üzenetet küld.

## További info:

https://ip-kalkulator.probbi.com/

https://github.com/probbi/ip-calculator

