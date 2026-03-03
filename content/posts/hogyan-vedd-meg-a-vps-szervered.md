+++
date = '2026-02-25T10:46:51+01:00'
draft = false
language = "hu"
title = 'Hogyan védd meg a VPS szervered'
summary = 'Hogyan védd meg a VPS-ed a botnetek rajzásától? SSH biztonság, UFW és Fail2Ban beállítások utáni eredmények valós adatok és logok alapján.'
+++

> *Mottó: A szerző nem professzionális hálózatguru, csak egy lelkes amatőr. Az itt leírtakat mindenki saját felelősségére alkalmazza!*

Amikor először nézel bele az /var/log/auth.log fájlba, hirtelen úgy érzed magad, mint a Nabukodonozor legénysége: az őrszemek ezrei kaparásszák a falaidat. A botok megállás nélkül próbálnak betörni a 22-es porton, pontosan úgy, ahogy a Mátrixban az őrszemek rajzottak Zion ostrománál. Ha nem akarsz áldozatul esni, ki kell építened a saját védelmi rendszeredet.

Ha nem akarod, hogy a szervered egy botnet része legyen, kövesd ezeket az alapvető lépéseket.

## 1. SSH kulcspár használata (A belépőjegyed)

A jelszavas belépés az első gyenge láncszem. Hozz létre egy **4096 bites RSA** vagy egy modern **Ed25519** kulcspárt a saját gépeden

```bash
ssh-keygen -t ed25519 -C "vps_kulcsod"
```

**Másold fel a publikus kulcsot a szerverre:**

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub felhasznalonev@szerver_ip
```

## 2. Saját felhasználó létrehozása - Ne légy mindig mindenható!

Legyen egy user fiókod és csak akkor légy root ha nagyon muszáj.

```bash
sudo adduser neved  # Adj neki egy erős jelszót!
sudo usermod -aG sudo neved
```

Másold át a kulcsodat az új usernek:
Mivel a későbbi lépésekben letiltjuk a jelszavas belépést, az új felhasználódnak is szüksége van a publikus kulcsodra:

```bash
sudo mkdir -p /home/neved/.ssh
sudo cp /root/.ssh/authorized_keys /home/neved/.ssh/
sudo chown -R neved:neved /home/neved/.ssh
sudo chmod 700 /home/neved/.ssh
sudo chmod 600 /home/neved/.ssh/authorized_keys
```

## 3. Tűzfal (UFW) beállítása – Ki ne zárd magad!

Mielőtt újraindítod az SSH-t, engedélyezd az új portot a tűzfalon, különben végleg kint maradsz:

```ini
sudo ufw allow 21349/tcp # Válassz egy egyedi portot (lehetőleg 1024 és 49151 között, ami még nem foglalt).
sudo ufw allow http
sudo ufw allow https
sudo ufw enable
```

## 4. SSH konfiguráció – A biztonság alapja

Szerkeszd az SSH konfigot: 

```bash
sudo nano /etc/ssh/sshd_config.
```

**Itt három kritikus módosítást kell elvégezned:**

Port megváltoztatása: A botok 99%-a a 22-es portot támadja. Válts például a 21349-re, de nem felejtsd el feljegyezni valahova, mert ha bármilyen okból törlődne a bash history, gondban leszel ha nem jut eszedbe a portszám.

`Port 21349`

A jelszavas belépés tiltása, hogy csak a kulcsoddal lehessen belépni:

`PasswordAuthentication no`

Root belépés tiltása:

`PermitRootLogin no`

Vigyázz! Innentől már csak a létrehozott felhasználóddal tudsz belépni!

pl: ssh -p 21349 neved@szerver-ip

**Ha ezekkel megvagy, jöhet az SSH-szolgáltatás újraindítása:**

```bash
sudo systemctl restart ssh
```

**Ha minden jól ment akkor be is léphetsz a szerveredre:**

```bash
ssh -p 21349 neved@szerver-ip
```

**💡 Pro Tipp:** Mielőtt újraindítod az SSH-szolgáltatást vagy kilépsz, tarts nyitva egy élő munkamenetet! Ha elrontottál valamit a konfigurációban, ezen keresztül még javíthatod; ha bezárod, marad a nehézkes VNC-konzol. (pl: nem lesz magyar billentyűzetkiosztás).

🔗 Egy kis segítség szabad port kereséshez: [TCP és UDP portszámok listája – Wikipédia](https://hu.wikipedia.org/wiki/TCP_%C3%A9s_UDP_portsz%C3%A1mok_list%C3%A1ja)

## 5. Fail2Ban – A kapuőr

A Fail2Ban figyeli a naplófájlokat, és ha valaki túl sokszor hibázik, kitiltja az IP-címét.

Telepítés:

```bash
sudo apt update && sudo apt install fail2ban -y
```

Hozz létre egy helyi konfigurációt (`sudo nano /etc/fail2ban/jail.local`), és add hozzá az SSH védelmet:

```ini
[sshd]
enabled = true
port = 21349
filter = sshd[mode=aggressive]
backend = systemd
maxretry = 3
findtime = 1d
bantime = 24h
```

## 5. Ellenőrzés és karbantartás

Indítsd újra a szolgáltatásokat:

```bash
sudo systemctl restart ssh
sudo systemctl restart fail2ban
```

Ellenőrizheted a kitiltott IP-ket:

```
neved@xxxxxxxxxx:~# uptime
 06:47:29 up 18 days, 20:11,  1 user,  load average: 0.01, 0.00, 0.00
neved@xxxxxxxxxx:~# sudo fail2ban-client status sshd
Status for the jail: sshd
|- Filter
|  |- Currently failed: 0
|  |- Total failed:     1170751
|  `- File list:        /var/log/auth.log
`- Actions
   |- Currently banned: 0
   |- Total banned:     1
   `- Banned IP list:
```

Szóval 18 nap alatt csaknem egymillió-kétszázezer próbálkozás egy noname, jelentéktelen szerverrel szemben.

### Miért nem várhatsz a védelemmel? (A számok nem hazudnak)

Sokan gondolják: *"Ki akarná az én kis szerveremet bántani?"*. Nos, lefuttattam egy gyors parancsot (köszi Gemini), hogy megnézzem, hány sikertelen próbálkozás történt és itt a top versenyző:

Bash

```
grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr | head -n 1
624410 75.xxx.xxx.xxx
```

**❗ Az eredmény brutális:** Egyetlen IP-címről (ami történetesen egy francia adatközpontban van) **több mint 624 000 alkalommal** próbálták kitalálni a jelszót.

Sokan azt hiszik, ha egy neves szolgáltatónál van a gépük, a belső hálózat védett. De a logokból hamar kiderült: a támadó ugyanannál a szolgáltatónál bérelt gépet, ahol én (pl: `tracepath IP` és meg is lesz támadó szerver szolgáltatója). 

## ❗ Update -avagy ne higgy a szemednek!

Megnyugodtam, hátradőlve hagytam, hogy a Fail2Ban tegye a dolgát. Egy héttel később ránézek, hogy hogyan dolgozik, hát sehogy. A `sudo fail2ban-client status sshd` parancs után a Total Banned: 1 db IP, ami túl szép volt, hogy igaz legyen.

Kiderült, hogy régen minden Linuxon az rsyslog uralkodott, ami egyszerű szöveges fájlokba (/var/log/auth.log) írta az eseményeket. A Fail2Ban alapértelmezés szerint még mindig ezt keresi, mert ez a "legkisebb közös többszörös" minden Unix-szerű rendszeren. A modern Linux rendszerek (Debian 12, Ubuntu 22.04+, Fedora...) viszont már a systemd-journald-t használják, ami nem szövegfájl, hanem egy bináris adatbázis.

Mivel a `/var/log/auth.log` dátumformátuma sokszor eltér a Fail2Ban által várt mintától, a szoftver egyszerűen "megvakul" és nem látja a támadásokat. A megoldás a `backend = systemd` és a `mode = aggressive` bekapcsolása volt a jail.local fájlban. Ezzel a Fail2Ban már nem egy szöveges fájlt bogarászik, hanem közvetlenül a rendszer belső naplójából (journal) kapja az adatokat.

Amint átírtam a konfigurációt és nyomtam egy restartot, a kép megváltozott. A Journal matches sor megjelent, és azonnal  megérkeztek az első kitiltott IP-címek is a listába!

```ini
[sshd]
enabled = true
port = 21349 #Természetesen a portszámot a saját választottadra cseréld.
filter = sshd[mode=aggressive]
backend = systemd
```

```
neved@xxxxxxx:~# sudo fail2ban-client status sshd
Status for the jail: sshd
|- Filter
|  |- Currently failed: 0
|  |- Total failed:     0
|  `- Journal matches:  _SYSTEMD_UNIT=sshd.service + _COMM=sshd
`- Actions
   |- Currently banned: 4
   |- Total banned:     4
   `- Banned IP list:   172.xxx.xxx.xxx 172.xxx.xxx.xxx 172.xxx.xxx.xxx 173.xxx.xxx.xxx
```

Azt az IP-t ahonnan egy bot folyamatosan bombázta a szerveremet véglegesen kibannoltam, biztos ami biztos alapon:

UFW

`sudo ufw insert 1 deny from 173.xxx.xxx.xxx to any`

### Tesztelés:

```bash
sudo iptables -L INPUT -n -v | grep 173.xxx.xxx.xxx
15509  930K DROP       0    --  *      *       173.xxx.xxx.xxx      0.0.0.0/0
```

- 15509 ennyi próbálkozás
- 927K forgalom megérkeztek
- DROP ez a státusz igazolja hogy a szerver egyáltalán nem válaszolt neki

```bash
tail -f /var/log/fail2ban.log
```

```
2026-xx-xx xx:xx:xx,xxx fail2ban.filter         [3090861]: INFO      findtime: 86400
2026-xx-xx xx:xx:xx,xxx fail2ban.actions        [3090861]: INFO      banTime: 86400
2026-xx-xx xx:xx:xx,xxx fail2ban.filter         [3090861]: INFO      encoding: UTF-8
2026-xx-xx xx:xx:xx,xxx fail2ban.jail           [3090861]: INFO    Jail 'sshd' started
2026-xx-xx xx:xx:xx,xxx fail2ban.filtersystemd  [3090861]: INFO    [sshd] Jail is in operation now (process new journal entries)
2026-xx-xx xx:xx:xx,xxx fail2ban.actions        [3090861]: NOTICE  [sshd] Restore Ban 172.xxx.xxx.xxx
2026-xx-xx xx:xx:xx,xxx fail2ban.actions        [3090861]: NOTICE  [sshd] Restore Ban 172.xxx.xxx.xxx
2026-xx-xx xx:xx:xx,xxx fail2ban.actions        [3090861]: NOTICE  [sshd] Restore Ban 172.xxx.xxx.xxx
2026-xx-xx xx:xx:xx,xxx fail2ban.actions        [3090861]: NOTICE  [sshd] Restore Ban 173.xxx.xxx.xxx
2026-xx-xx xx:xx:xx,xxx fail2ban.actions        [3090861]: NOTICE  [sshd] Unban 172.xxx.xxx.xxx
```

### Összegzés és tapasztalatok

A nyitva hagyott 22-es port vagy a jelszavas hitelesítés azonnali kockázatot jelent. A logok alapján látható, hogy rövid időn belül ~ 624 000 betörési kísérlet bizonyítja, hogy az automatizált botok nem válogatnak: minden IP-re lőnek, amit elérnek.

**Ami nálam alapvető beállítás egy új VPS indításakor:**

- **SSH kulcs alapú hitelesítés** (jelszó tiltva).

- **User létrehozása** (hogy csak akkor legyünk benn rootként ha indokolt)

- **Egyedi port** (hogy a botok 99%-a lepattanjon).

- **UFW** (szigorú szabályokkal).

- **Fail2Ban** (hosszú kitiltási idővel a visszaesőknek).

A legfontosabb tanulság pedig: amíg a konfigurációt piszkálod, **ne zárd be a terminált**. Mindig legyen egy menekülőút, mert ha kizárod magad, a VNC konzol minden lesz, csak nem barátságos.

**A biztonságodért te felelsz, nem a szolgáltatód!**
