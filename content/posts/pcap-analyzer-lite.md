+++
date = '2025-05-15T10:12:55+01:00'
draft = false
title = 'Pcap Analyzer Lite'
summary = 'A cél egy egyszerű hálózatelemző Python szkript létrehozása volt.'
+++

# Egyszerű hálózati forgalom elemző

A célom az volt, hogy legyen egy egyszerű saját hálózati elemző programom, hogy jobban megértsem a hálózati forgalom és adatelemzést. 

A projekt Pythonban készült, tanulási céllal, dokumentációk, szakmai fórumok és AI-alapú eszközök bevonásával. A generált kódot átnéztem, módosítottam és a saját igényeimre szabtam.

A fejlesztés során a saját VPS szerverem forgalmát elemeztem, ami lehetőséget adott valós környezetben történő tesztelésre és finomhangolásra. Meglepő volt látni, milyen mennyiségű gyanús próbálkozás ér egy átlagos, publikusan elérhető szervert.

A szkript a pcap-analyzer-lite.py nevet kapta. Funkcionálisan egyszerű, de a célját betölti: tanulás, kísérletezés és a hálózati események jobb megértése.

### Leírás

Ez a Python script egy .pcap fájlban található hálózati forgalmat elemez, és összesíti a legfontosabb információkat:
 - forrás IP-címek és azok gyakorisága;
 - IP protokollok (TCP, UDP, ICMP, stb.) megjelenési aránya;
 - leggyakoribb TCP és UDP célportok;
 - valamint az egyes forrás IP-címek geolokációs helye (város, ország).

A geolokációhoz a MaxMind GeoLite2 adatbázist használja.


```
# file: pcap-analyzer-lite.py
# scapy.all.rdpcap a Scapy egy hálózati csomagkezelő könyvtár. Az rdpcap() függvény segítségével beolvasható egy .pcap fájl, és visszaadja a benne lévő összes csomagot.
#collections.Counter: egy speciális szótár, amely számlálja, hogy hányszor fordul elő egy adott elem.
from scapy.all import rdpcap, TCP, UDP, IP
from collections import Counter
import geoip2.database

# Beállítás: GeoIP adatbázis elérési útja

import os

SCRIPT_DIR = os.path.dirname(os.path.abspath(__file__))
GEOIP_DB_PATH = os.path.join(SCRIPT_DIR, "data", "GeoLite2-City.mmdb")
PCAP_FILE_PATH = os.path.join(SCRIPT_DIR, "data", "sample-traffic-analysis-exercise.pcap")

# Protokoll számok és rövidített nevek leképezése
proto_names = {
    1: "ICMP",
    6: "TCP",
    17: "UDP",
    2: "IGMP",
    
    # ide jöhetnek még protokollok, pl. 2: "IGMP", 89: "OSPF" stb.
}


app_proto_names = {
    80: "HTTP",
    443: "HTTPS",
    53: "DNS",
    123: "NTP",
    25: "SMTP",
    110: "POP3",
    143: "IMAP",
    22: "SSH",
    21: "FTP",
    20: "FTP (data)",
    23: "Telnet",
    3306: "MySQL",
    3389: "RDP",
    8080: "HTTP-alt",
    # ... lehet bővíteni még igény szerint
}
DETAILED_APP_PROTO = True  # vagy False, ha nem kell a részletes lista
# Beolvassa a megadott .pcap fájlt, és eltárolja a benne lévő összes hálózati csomagot a packets változóban. Ez egy listaszerű objektum lesz, amiben minden elem egy hálózati csomag.
packets = rdpcap(PCAP_FILE_PATH)
# Számlálók inicializálása
# ip_counter: ebben a szótárban számolja meg, hogy melyik forrás IP-cím hányszor fordul elő.

# proto_counter: itt számolja, hogy melyik IP-szintű protokoll (pl. TCP, UDP, ICMP) hányszor jelenik meg.
ip_counter = Counter()
proto_counter = Counter()
port_counter = Counter()

# GeoIP olvasó inicializálása
geo_reader = geoip2.database.Reader(GEOIP_DB_PATH)

ip_locations = {}

# Csomagok feldolgozása
# pkt.haslayer("IP"): csak azokat a csomagokat vizsgálja, amelyek IP réteget tartalmaznak.
# pkt["IP"].src: lekéri a forrás IP-címet.
# pkt["IP"].proto: lekéri az IP protokoll számát (például 6 = TCP, 17 = UDP, 1 = ICMP).
# Ezeket a számlálókat frissíti minden IP csomagra.


for pkt in packets:
    if IP in pkt:
       ip = pkt[IP].src
       ip_counter[ip] += 1
       proto = pkt[IP].proto
       proto_counter[proto] += 1
       
       # Portok vizsgálata
       if TCP in pkt:
            port_counter[pkt[TCP].dport] += 1
       elif UDP in pkt:
            port_counter[pkt[UDP].dport] += 1
       
       # Geolokáció csak egyszer per IP 
       if ip not in ip_locations:
            try:
                response = geo_reader.city(ip)
                city = response.city.name or "Ismeretlen város"
                country = response.country.name or "Ismeretlen ország"
                ip_locations[ip] = f"{city}, {country}"
            except:
                ip_locations[ip] = "Nem található"

geo_reader.close()
            
# Top forrás IP-k és geolokációk
print("\n Top forrás IP-k és helyük:")
for ip, count in ip_counter.most_common(5):
    location = ip_locations.get(ip, "N/A")
    print(f"{ip} ({location}): {count} csomag")

# Protokoll eloszlás kiírása
print("\n Protokoll eloszlás:")
for proto_num, count in proto_counter.items():
    proto_name = proto_names.get(proto_num, f"UNKNOWN({proto_num})")
    print(f"{proto_name}: {count} csomag")

print("\n Leggyakoribb TCP/UDP célportok és alkalmazásréteg protokollok:")
for port, count in port_counter.most_common(10):
    proto_name = app_proto_names.get(port, "Ismeretlen")
    print(f"Port {port} ({proto_name}): {count} csomag")
```


### A szkript használata:

 - töltsd le a GeoLite2-City.mmdb adatbázist a MaxMind oldaláról és tedd a data/ mappába;
 - helyezz el egy .pcap fájlt a data/ mappában (pl. sample-traffic-analysis-exercise.pcap);
 - futtasd a scriptet Python 3 környezetben: `python3 pcap-analyzer-lite.py`;
 - nézd meg az eredményeket a konzolon.
 
### A futtatás utáni eredmények:

```
📊 Top forrás IP-k és helyük:
172.16.4.205 (Nem található): 13357 csomag
185.243.115.84 (Frankfurt am Main, Germany): 8571 csomag
166.62.111.64 (Ismeretlen város, United States): 5677 csomag
172.16.4.4 (Nem található): 457 csomag
31.13.70.52 (Los Angeles, United States): 218 csomag

📦 Protokoll eloszlás:
UDP: 190 csomag
IGMP: 8 csomag
TCP: 29011 csomag

🔢 Leggyakoribb TCP/UDP célportok és alkalmazásréteg protokollok:
Port 80 (HTTP): 12293 csomag
Port 49249 (Ismeretlen): 8571 csomag
Port 49201 (Ismeretlen): 1522 csomag
Port 49200 (Ismeretlen): 1051 csomag
Port 49198 (Ismeretlen): 940 csomag
Port 49190 (Ismeretlen): 848 csomag
Port 49202 (Ismeretlen): 697 csomag
Port 49199 (Ismeretlen): 616 csomag
Port 443 (HTTPS): 540 csomag
Port 49223 (Ismeretlen): 163 csomag
```

### Követelmények:

 - Python 3.x
 - Scapy (pip install scapy)
 - geoip2 (pip install geoip2)
 - GeoLite2 City adatbázis (MaxMind)

### Mit tanulhatok ezzel a projekttel?

 - alapvető hálózati protokollok kezelése Pythonban (IP, TCP, UDP, ICMP);
 - .pcap fájlok feldolgozása Scapy-vel;
 - egyszerű statisztikák készítése Python Counter osztállyal;
 - IP címek geolokációja MaxMind adatbázissal;
 - Python scripting gyakorlása, hibakezelés és kimenet formázás.

### Jövőbeli fejlesztési lehetőségek

 - Forrásportok és egyéb IP réteg attribútumok elemzése.
 - Protokollszűrés vagy időalapú elemzés.
 - Részletesebb alkalmazásréteg elemzés pl. HTTP, DNS.
 - Interaktív vagy webes megjelenítés.
 

### Használt eszközök / könyvtárak

| Eszköz / Könyvtár     | Funkció                              |
| --------------------- | ------------------------------------ |
| `Ubuntu Linux`        | Operációs rendszer                   |
| `tshark`              | Forgalom rögzítése `.pcap` fájlba    |
| `Scapy`               | PCAP beolvasása és elemzése          |
| `geoip2` + `GeoLite2` | IP geolokáció (város, ország)        |
| `venv`                | Python környezet izolálása           |
| `fish shell`          | Alternatív shell, aktiválás módosult |


