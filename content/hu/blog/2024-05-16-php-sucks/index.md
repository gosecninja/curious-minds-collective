---
Title: Szerver biztonsági program PHP-ban? Hülye vagy?
description: ""
summary: ""
date: 2024-05-16
draft: false
weight: 50
categories: [DevSecOps]
tags: [
  DevSecOps,
  PHP,
  Experience,
  MentalInCyber,
]
contributors: [Zoltan Toma]
pinned: false
homepage: false
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

A tapasztalatom szerint a DevSecOps írja le leginkább, amit csináltam. Bár jobban szeretem a Lead Security Software Engineert (Ehhez találj HR számára érthető leírást). De amikor egy beszélgetés során megemlítem, hogy server biztonsági szoftver fejlesztettem PHP-ban, mindenki meglepődik.

- El ment az eszed? A PHP nem biztonságos. Lassú. Szar kódot lehet benne írni.

És körülbelül ennyivel le is van zárva a beszélgetés. (Sajnos ez általában interjúkon fordul elő.)

- Miért nem írtátok Phyton-ban vagy Go-ban?

Teljesen jogos felvetések. De amikor a YouTube véletlenül feldobta ezt a videót és láttam ezt a reakciót át kellett gondolnom a hozzáállásom.

![wtf_php](cover_wtf_php.png)
Forrás: [PHP Doesn't Suck Anymore? | Prime Reacts](https://youtu.be/WsnHWxO7Krw?si=Um836Ej1P7FuSyKU)

Eredeti videó: [PHP doesn't suck (anymore)](https://youtu.be/ZRV3pBuPxEQ?si=kJvJOAWX5siIGvuJ)

## Miért PHP?

Kezdjük az elején. Miért PHP? A válasz egyszerű. Nem én kezdtem a projectet.

Kellett pár fejlesztő, aki képes kódolni. És kellett egy server oldali script nyelv, amit könnyen lehet futatni. A szoftver shared hosting server problémákat hivatott megoldani. És mit látnak shared hosting rendszer gazdák nap mint nap? PHP kódot.

Persze, van Bash és minden Linux szerveren elérhető a Phyton. De mit olvasol nap mint nap, miközben próbálsz rájönni, hogy megint, hogy a fenébe törték meg a szervert? Az ügyfél oldalát? PHP-t.

De miért nem váltottunk egy gyorsabb biztonságosabb nyelvre később? Majd ezt is leírom.

Volt egy másik ok, amiért PHP-ban volt a kód. Ezt azóta sokan elfelejtették. Hogyan bízol meg egy ismeretlen kis cég "biztonsági" szoftverében?

- Utána Google-zel, hogy mikor alapították. Ó, még egy éve sincs. (Nem telepítem)
- Utána nézel social médián. Ó, még nincsenek review-ok. (Nem telepítem)
- Folyton csesztetnek a reportjaikkal. Most már kíváncsi vagyok rá, hogy mi a frászt árulnak. Hogyan tudom megnézni a kódot? Ja, ez csak sima PHP. (Nem telepítem)
- Na jó ezek a reportok már tényleg idegesítőek. Olvassunk csak bele, hogy ez mit is csinál. Na várj csak. Ez most komolyan egy több számon futó, moduláris, több mindent is figyelő, tényleges biztonsági szoftver? Ami a támadási információkat majdnem realtime osztja meg a védet szerverekkel? Ezt nem hiszem el! (Ki kell próbálnom...)

## A PHP kód nem biztonságos

Mikor ezt a kijelentést hallom, mindig felhúzom a szemöldökömet. Miért nem biztonságos?
- Nincsenek benne classok.
- Tele van XSS-el, SQL injectionnel. Stb.
- Nincsenek típusok.

Mikor néztél utoljára tényleges PHP projektet? Ami nem egy Wordpress plugin volt? (De azért már ott is nagy fejlődést értek el.) Amire gondolnak általában, a bootcampek-ben tanított, vagy esetleg kezdő fejlesztők által készített HTML, CSS, JavaScript, PHP szörnyűségek. Amik mind egy darab 20.000-200.000 soros monstrum fájlban vannak. Na azok tényleg nem biztonságosak. De nem a nyelv miatt. Hanem a fejlesztő tapasztalatlansága miatt.

Ugyanezek a hibák jelen vannak bármely nyelvben. Ugyan úgy tudsz XSS hibát elkövetni, ha nem validálod az inputot vagy nem formázod HTML biztonságosra a kimentet, Javában, C#, Go-ban, Phytonban. Ha nem használsz prepared statement, akkor pedig SQL injection sebezhetőséged lesz.

De kellett valaha Buffer overflow exploitot írnod? Vagy esetleg format string exploitot? Ezek nincsnek jelen a nyelvben, mert nyelvi szinten kezelve vannak.

A típusokkal kapcsolatban pedig meglepődsz, ha megnézed a videót.

## A PHP lassú

Mikor azt hallom, hogy a PHP lassú, csak lassan bólogatok. Hát igen, PHP 5.6 korában tényleg lassú volt a nyelv. De hogyan lehetett rajta gyorsítani? Mondjuk web servert váltottál? Igen. PHP kódot általában egy web server szolgál ki. Csak hát maga a web szerver nem képes elég gyorsan fogadni a kéréseket.

De a kód amit mi írtunk, szerver oldalon futott. Semmilyen módon nem kapcsolódott a szerveren található web serverhez. Így nem volt ilyen limitációnk. De ez felvett egy másik érdekes kérdést. Kell e nekünk olyan PHP plugin, ami web kiszolgáláshoz kell? Nem. Akkor mi lenne, ha ezeket nem használnánk? Sőt nem is töltenénk be?

Ehhez mi kell? Egyedi PHP futatót kell készíteni, ahol ezek a pluginok nem kerülnek bele a készült binárisba. Csak ami feltétlen szükséges a PHP-CGI futatásához. És képzeld, ha nem töltesz be a programodba fölösleges modulokat, gondolnád, hogy gyorsabb lesz? Sokkal gyorsabb.

- De mihez képest lesz gyorsabb? Phytonnál biztos nem!

Már rég csináltam benchmarkot. De ki kell ábrándítanom Titeket. String feldolgozás terén a Phyton 3.x hasonló sebességen mozog, mint a PHP 5.6. Hogyan lehet ezen javítani? Úgy hogy Python C bindingokat használsz. Ami lényegében a Python kód működését átadja egy gyorsabb C-ben írt pluginnek.

Ehhez mi kell? Elsőnek is telepítened a Phyton 3-at. Mert az legtöbb operációs rendszeren nincs fent alapból. Majd a szervere található C fordítóval a szerveren lokálisan kell építened egy C csomagot. (Csodálkozol, hogy Alpine-on miért nem jó Pythonban dolgozni? Ezért.)

Szóval telepítenem kell egy csomagot? Amiben C-ben írt kiegészítések gyorsítják a feldolgozást? Na várj csak. Ezt PHP-ban is el lehet érni. Mert bárkinek lehetősége van C-ben C++ PHP plugin-t, expension-t írni. És a PHP 7.3 már alapból kb 8x gyorsabb volt a Phyton társához képest.

És ha elmesélem, hogy a `php_aho_corasick` modul beépítése után 7000% gyorsulást értünk el a szöveg felismerésben, még ezek után is, gondolom nem hiszel nekem. (Nem írtam el. 7000% volt a javulás.) Pedig én voltam az aki elkezdte portolni az extensiont 5.6-ról PHP 7.2-re. Lásd: [https://github.com/ph4r05/php_aho_corasick/pull/5](https://github.com/ph4r05/php_aho_corasick/pull/5). (Bocs srácok, hogy nem tudtam többet tenni, hogy segítsem a munkátokat.)

Akkor miért nem C-ben írtuk? Próbáltál már megoldani egy remote szerveren egy edgecase-t? Úgy, hogy nincs más jogosultságod, csak SSH és nano? Egy dinamikus nyelv kellett.

## PHP-nak nincsenek jó security megoldásai

Egy jó ideje próbálok bekerülni egy olyan security szoftver fejlesztésébe, ahol Go-t használnak. Ennek is meg van az oka. De mikor beszélgetni kezdünk biztonság technikáról egyből előjönnek a betűszók. Foglalkoztál már application security-vel?

- Hát hogyne foglalkoztam volna!
- Milyen SAST, DAST, XYZ-t használtál?

Amire gondolnak az ez az ábra az OWASP DevSecOps Guideline-ból:

![OWASP DevSecOps Guideline](https://owasp.org/www-project-devsecops-guideline/latest/assets/images/Pipeline-view.png)
Forrás: https://owasp.org/www-project-devsecops-guideline/latest/

Ó, hogy ne ismerném ezt az ábrát. Sok eszközt én is kipróbáltam róla és más alternatívákat is. De őszintén szóval nem találtam jobbat a php composerrel telepíthető ingyenes pluginoknál. És képzeld. Lokálisan is futathatóak. Nem kell egy SaaS megoldásra előfizetni csak azért, hogy biztonságos kódot írjak.

Az egészbe az a bosszantó, amikor netalán bekerülök a projektre és látom, hogy nem hogy a full DevSecOps pipeline nincs implementálva, de még egy alap Linter sincs beállítva. És arról ment az interjú, hogy használtam e már SonarQube-ot és Snyk-et, vagy állítottam be ZAProxy-t.

Szerinted nekünk milyen security elvárásaink voltak, ha a szoftverünket olyan potenciálisan rootra tört szerverekre telepítették, mivel már kb semmi más nem segített? Ráadásául ugyanúgy kellett működnie minden támogatott distribución.

- Miért nem használunk Cloudot? Ott már megoldották ezeket a problémákat.

Ki kell, hogy ábrándítsalak. A cloudon is csak Linux serverek vannak. Telepítették a szoftverünket AWS-re, Azure-re, GCP-re, Linode-ra, stb. És képzeld. Onnan is jöttek malware találatok és blokkolt incidensek.

A Cloud egyetlen dologra add megoldást. A skálázhatóságra. De leginkább a saját skálázhatóságukra.

## Pipeline

Kezdjük az elején. Hol kezdődik a DevSecOps pipeline? Lokálisan. Egy fejlesztő leül a gépe elé és elkezd pötyögni valamit. Itt nincs commit, nincs kollaboráció nincs semmi. Egy fejlesztő leült a gép elé és dolgozik. Ha végzett a feladatával szeretné ezt publikálni.

Kell e git? Nem feltétlen. Elegendő feltölteni a kódot egy szerverre. Mondjuk egy zipben. (Köszönöm AWS, hogy még mindig vannak ilyen marhaságok...)

De ha többen szeretnénk együtt dolgozni a kódon, már ki kell találni ezt hogyan kell. Hát akkor miért ne használnánk verzió követőt? Ez lehet git. A git feladata, hogy segítse az embereket szöveges fájlok közös szerkesztésében. Se több, se kevesebb. De mi kerül be és mi történik a a változtatásokkal az egy teljesen más kérdés.

Nincs bosszantóbb, mint mikor péntek este valaki bekommitol a masterbe egy olyan kódot, amiből hiányzik egy pontos vessző. Aztán hajnalban csörög a telefon, hogy javítani kell az oldalt, mert az ügyfelek panaszkodnak. Erre való a Lint. Telepíteni composerrel lehet. Mondjuk így `composer require --dev overtrue/phplint`. Aztán parancssorból tudom ellenőrizni, hogy a kódomban nincs e nyelvi hiba. Nincs több péntek esti telefon csörgés.

De várj! Composer? Vagyis tudok csomagokat kezelni és egyszerűen letölteni? Igen lehet. A csomag információk elérhetőek a `composer.json`-ben. Amit tudok ellenőriztetni, mondjuk Trivy-vel, Dependabot-tal, régebben volt hozzá composerrel telepíthető security checker. De ahogy most olvasom, már elérhető a `composer update --audit` kapcsoló is.

Két dolog amit szoktunk mondani. Udpateld a szoftvereidet. És rendszeresen ellenőrizd, hogy nincs benne sebezhetőség. Ez is pipa egy paranccsal.

De ha már composer-t használhatunk, akkor más kiegészítőket is telepíthetünk, amik megkönnyítik a dolgunkat. Ezeket elérhetjük az autoloaderből. És ezzel meg is ismerkedhetünk a PSR-4 standarddel. Standard szintjén meg van határozva, hogyan tudok használni több mappára szétbontott kódot.

Nincs meg, hogy hova tegyem a kiegészítő scriptjeimet? A utils mappába, scripts mappába? Hogyan tudok rájuk hivatkozni? Megtud őrjíteni, mikor egy ismeretlen kódbázisban azt kell kutatnom, hogyan kapcsolódnak össze az egyes fájlok.

De ha már standardeknél járunk. PRS-2? Hogyan formázzuk a kódunkat? Most szóközöket, vagy tabokat használjunk? A kapcsos zárójel a sor végén, vagy új sorban legyen. Kell e zárójel? Egy standardel el van intézve. És, ami még jobb, hogy egy parancs lefutatásával javíthatjuk a kód formázását a standardnek megfelelően.

De ha már kód minőség ellenőrzés. Hol vannak a testek? Testek nélkül nem lehet biztonságos a kód. Composer segítségével telepíthetünk PHPUnitot és már írhatjuk a tesztek.

Ha jót akarunk magunknak a kód minőség ellenőrzést megoldhatjuk egy kombinált eszközzel, mondjuk a phpqa-val. `composer require --dev edgedesign/phpqa`. Aztán egy parancs lefutatásával lokálisan szép átlátható, kereshető HTML reportokat tudunk belőle generálni. Mennyi most a Code Smell-em? Ja ennyi.

Mivel nem elegendő azt tudni, hogy mennyit változott a kód minőség. Ha a szar nem változik, attól még szar marad. (Igen rád gondolok SonarQube)

De vannak olyan tesztelési feladatok, amik nem végezhető el Unit testekkel. Szóval mi lenne, ha valami komolyabban használnánk? Mondjuk Codeceptiont? `composer require codeception/codeception`. Ez egy BDD framework. És akár cucumber testet is írhatunk benne.

Amit eddig leírtam, mind működik lokálisan. Nem kell hozzá előfizetés. Nem kell pipeline. Csak elegendő egy-egy parancsot futatni. A probléma ott kezdőik, hogy nem mindenkinek jut eszébe ezeket a parancsokat futatni. Erre megoldás a `git pre-commit hook`. Addig nem kommitolsz, amíg a parancsok sikeresen le nem futnak. És már megint nem kell pipeline.

A gond akkor van, mikor ezek ellenére sem hajlandó valaki használni a toolokat. Erre való egy git pipeline. Nem számít, hogy ki mennyire makacs. Mennyire szeretne sietni. A pipeline minden push után lefut. (Vagy merge előtt, de ez már branching stratégia).

- Ebben nincs benne DAST.

A Selenium teszt szerinted micsoda? És a Codeceptionben, van Selenium test driver. De persze megpróbálhatunk belőni egy ZAP-t, vagy más crawlert, ami fuzzingolja az alkalmazást. De ez már körülményesebb tesztelést igényel. Amit lehet érdemes egy build szerveren futatni. De ez már nem PHP specifikus probléma.

Szerinted mennyit tudok a tesztelésről, ha készítettem PHP Codeception-hoz egy Asyncron FakeApi fuzzing module-t? Lásd: [module-fake-api](https://github.com/LeeShan87/module-fake-api)

Nem hiába mondom, hogy ne beszéljünk DevSecOps best practicekről, ha a kód, amit felügyelni kellene 0% teszt lefedettséggel rendelkezik. Számomra ez a legfontosabb mérőszám. Ha nincs legalább 80% lefedettség, ne is gondolkodjunk DAST-ban. Mit kezdesz, ha talál valamit? Már pedig fog. Ugyan ez a helyzet a PenTest-el. Mit kezdesz egy reporttal, ha találnak valamit? Már pedig fognak.

Az egész pipeline az éles production serveren ér véget. De ez már megint nem PHP specifikus probléma.

## Vannak dolgok amiket nem lehet PHP-ban megoldani

Igen tudom. Sok mindent nem lehet PHP-val megoldani. Vannak jobb és optimálisabb megoldások. De ebben az esetben a PHP csak manager kód volt.

Low level sebesség optimalizálás kell? PHP modul megoldja az esetek nagy részét.

De írtam már PAF-ot PHP-ban. Igen jól olvastad PAF-ot. Ez Protocol Application Firewall-t jelent. Egy átlagos szerveren nem csak a HTTP portok vannak nyitva (Sajnos). Így ez elég érdekes fogásokat eredményezett. A WannaCry vírust hónapokkal korábban érzékeltük, csak nem volt elegendő emberi erőforrásunk, hogy utánna járjunk. (Bocs világ.)

De a leginkább használt funkciója a HTTP kérések kiszolgálása volt a PAF-nak. Ezt a kérés mennyiséget nem volt képes a PHP nyelv kiszolgálni. Ezért le kellett cserélni a kódot egy hatékonyabb megoldásra, ami mi más lehetne, mint ModSecurity.

## Az 1% problémája

Nagyon szerettem ezen a projekten dolgozni, viszont sajnos eljutottunk az 1% problémájához. És sajnos ez az, ami miatt nem használnék ismételten PHP-t egy ekkora szabású biztonsági szoftver készítéséhez.

Nem lehet 1 csomagban, minden ügyfél, minden felmerülő igényét kielégíteni. Elsőnek is, nincs annyi ember, aki ezt eltudná látni. A másik, nem mindegyik ügyfélnek vannak ugyan olyan problémái. Ezt csak microservise architecturával lehet megoldani. És nem. Nem telepíthetsz Kubernetest ügyfél szerverére. Dockert sem. Semmi mást, ami komolyabb pár binárisnál. Nincs hozzá jogosultságod, mivel nem Te menedzseled a szerverekt.

Vagyis kell valami, ami OS független. Gyorsan lehet binárisokat készíteni. Függetlenül lehet releaselni. Könnyen meg lehet tanulni. Ez pedig a Go.

Szerinted a Kubernetes miért Go-ban írták? Pont ezért a problémáért.

A vicces az egészben, hogy nem a PHP volt a limitáció. Szerinted ehhez a projecthez mihez kellett érteni?
- Programozási nyelvek: PHP, Bash, Phyton, Ruby, C, C++, Javascript, Lua
- Operációs rendszerek: Ubuntu, Debian, Centos, CloudLinux, Kali, OpenSuse, stb. (Windows neeeeem. Azért nekem is vannak határaim.)
- Security toolok: ModSecurity, Auditd, Osquery, minden ami proxy, BurpSuite, Yara, netfilter (szándékosan nem Iptables írtam), honeypotok, stb. (Bocs, nem jut mind eszembe.)
- Virtualizáció: Docker, Kubernetes, Docker Swarm, Virtualbox, Level 1 és 2 hyper visorok.
- Cloud: Ahol képes vagy Linuxot futatni...
- Build toolok: Docker (igen bináris építésre is lehet használni), chroot, fpm, rpmbuild, aptly
- Repó toolok: Artifactory, Nexus, Docker image repok.
- Pipeline-ok: Jenkins, BitBucket, Github
- Security technikák: Reverse Enginering, Malware analyzis, Incidens értelmezés és mitigálás, WAF szabályok írása.
- Configmanager-ek: Ansible, SaltStack, Chef
- Interprocess kommunikáció: System-V, SharedMemory, X-MQ (pub-sub, ne szépítsünk), API, RestAPI (szeinted ez mi más lenne, mint 2 process közötti kommunikáció?)

És már megint elfáradtam a felsorolásban. Szóval lehet e PHP-ban hatékony server biztonsági szoftvert írni? Igen lehet. De ne csodálkozz mi minden másra is szükség van mellette.

- Mennyire skálázható ez e megoldás?

Mikor ott hagytam a projektet világ szerte 7000 szervert védet és weoldalak millióit. De ettől sokkal többre is képes lett volna. Ez csak ügyfél szerzés és megtartás kérdése volt.

## Mentál egészség

Hogy kapcsolódik ez a mentál egészséghez? Szerinted képes az emberi agy ennyi mindent észben tartani? Voltál már felelőse milliónyi weboldalnak? Keltél már arra, hogy több, mint 200 szerver megállt és több százezer oldal nem működik?

Ne csodálkozz rajta, hogy kiégtem. És azon sem, hogy elfelejtek dolgokat. Vagy, hogy sokáig tart előhívni őket. Ha lehet interjúkon, ne használjunk betűszavakat. Sokmindent jelenthetnek. És ha valakinek nem jutt eszébe egy tool, nem biztos, hogy nem használt hasonlót. Ne alázzuk porrá a másikat, csak azért mert nem ismeri az XYZ-t.

## Felhívás a cselekvésre?

Sokat gondolkodtam egy hatásos cselekvésre ösztönző szekción. Az eredmények? Nos, alapvetően folyamatosan nézegettem a különféle statisztikákat, e-maileket és közösségi oldalakat.

Szóval, ha tetszett amit írtam, kérlek:

- NE tapsolj!
- NE nyomj egy lájkot!
- NE oszd meg!
- NE akarj velem beszélni a Calendly-n: [https://calendly.com/zoltantoma87/one-on-one](https://calendly.com/zoltantoma87/one-on-one), naponta csak egy embert tudok elbírni egy óráig.
- NE vegyen fel ismerősnek a LinkedIn-en: [https://www.linkedin.com/in/toma-zoltan/](https://www.linkedin.com/in/toma-zoltan/)
- NE támogasson a Patreonon: [https://www.patreon.com/CuriousMindsCollective](https://www.patreon.com/CuriousMindsCollective)
- NE akarj velem dolgozni! Irreális elképzeléseim vannak a közös munkáról: [https://medium.com/@zoltantoma/how-much-should-i-charge-for-my-service-b2c8b380e66e](https://medium.com/@zoltantoma/how-much-should-i-charge-for-my-service-b2c8b380e66e)

És ne! Ismétlem: NE kövesd a munkámat. Csak a saját egómat táplálná, miközben a #MentalInCyber témán dolgozom, és megpróbálom átadni a (kiber)biztonság-első üzleti modellt.
