+++
date = '2026-01-20T10:42:27+01:00'
draft = false
language = "hu"
title = 'Migrálás Hugora'
summary = 'Egy statikus HTML weboldal migrálása és átalakítása Markdown alapú Hugo projektté.'
tags = ['hugo', 'papermod', 'migralas', 'cloudflare-pages', 'git']
lastmod = 2026-02-05
+++

Úgy gondoltam, itt az ideje az egyoldalas statikus HTML-oldalamat átköltöztetni valamilyen egyszerű és gyors keretrendszerre. Mivel ez a bemutatkozós–blogolós weboldalötlet amúgy is a Linux–Git–Cloudflare szentháromságban született, kézenfekvő volt, hogy olyan keretrendszert keressek, ami erre könnyen ráhúzható.

Rövid keresgélés után meg is találtam a számomra megfelelőt: a Hugo keretrendszert. Terminálból pár kattintás volt telepíteni, elsőre a PaperMod téma tökéletesen megfelelt. A bejegyzéseket Markdown leíró nyelven lehet létrehozni, ami egy nagyon egyszerű formátum, szóval nincs vele sok macera. A statikus oldalam index.html tartalmát Markdown formátumúra átszerkeszteni nagyjából 10 perces mutatvány volt, és külön öröm, hogy saját CSS-sel lehet csinosítgatni az oldalt.

Terminálból lehet localhost szervert indítani, mellé pedig egy Markdown-szerkesztővel (pl. ghostwriter) WYSIWYG módon szerkeszthető a szöveg. Ha elkészültem, mehet Gitre a commit, és már kint is lesz a világhálón.

Szóval ezzel is megvagyok, jöhetnek a tartalmak.

## update: - 2026-02-05

A Githubról Cloudflare párosítást a Hugoval összehozni egyáltalán nem volt egyszerű. Beletelt egy kis időbe amíg az új Cloudflare UI-n megtaláltam a Pages oldalt, mert eldugták a jobb felső sarokba a + Add gomb mögé.

![Pages](https://pub-91da716c25164a4a9bb1755980b809e1.r2.dev/Cloudflare-New-UI-Pages.webp)

A helyi gépen a commit hiba nélkül lefutott, a Pagesben beállítottam mindent amit helyesnek gondoltam, a repository másolás rendben volt de nem mégsem töltött be a weboldalam. A hiba abból eredt, hogy nemes egyszerűséggel kihagytam a Framework Presets beállításokat.

| Framework preset:                            | Hugo                   |
|----------------------------------------------|------------------------|
| Build command:                               | hugo                   |
| Output directory: public                     | public                 |
| Environment variable: HUGO_VERSION = 0.xxx.0 | HUGO_VERSION = 0.xxx.x |

Szuper. Pages letörölve, a folyamat újrakezdve, a végén a domaineket beállítani és kész. Majdnem de nem. Még mindig nem jó pedig commit szerint jó kellene, hogy legyen. 

És végül az érzés, hogy amikor rájövök, hogy azért sikertelen a feltöltés mert a Cloudflare kapcsolat lehal állítólag a Telekom miatt és VPN-t kell indítanom, hogy elérjem a Cloudflare fiókomat és a Protonos levelezésemet az fenomenális.
