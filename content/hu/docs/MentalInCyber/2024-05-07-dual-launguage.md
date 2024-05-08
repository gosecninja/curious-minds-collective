---
title: "Több nyelvű pipeline"
description: ""
summary: ""
date: 2024-05-06
#lastmod: 2024-05-06
draft: false
weight: 110
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---
Most komolyan azon gondolkodom, hogy több nyelvűsitem a tartalmamat? Ennek mi haszna van? Ha sikeresek akarunk lenni, egy olyan nyelvet, platformot kell választanunk, amin kerestül sok potenciális ügyfelet érünk el.

Ez egy hatalmas vissza lépés. Csomó plusz munka és már megint megosztom az eddig is kevés figyelmemet. Hallom a fejemben a hangot és a számtalan üzlet fejlesztési tanács adó oktatást, amin részt vettem. Nem is beszélve az informatikában szerzet tapasztalatokról.

De ahogy ezen gondolkodom egyre szimpatikusabbnak tűnik az ötlet. A problémám jelenleg a lag. Vagyis a kommunikációban lévő kis idő kiesések. Hogy mi lehet ennek az oka, már korábban tárgyaltuk. Egy plusz csatorna beiktatása valóban több időt vesz igénybe. És persze, hogy add egy plusz eredmény költséget.

## Mi az érték?

Ide kell majd egy diagramot készítenem, az observability pattern folytatásáról. TODO. Na de, amit mondani szeretnék. Mi az értéke annak a gondolatnak, aminek nincs megtestesülése? A válasz egyszerű. Semmi. Más nem feltétlen tud róla. Még kevésbé hajlandó érte fizetni. És biztos hogy a baráti társaságunkból, pár váll veregetésen kívűl nem sokkal több támogatást fogunk kapni. (Remélem, Ti azért szerencsésebbek vagytok.)

Mi okozhatja, hogy az ötletből nem lesz valóság? Az információ egy lassú tudásbázisban van. Az agyunkban. Ha ezt szeretnénk közölni, ezt valahogy ki kell belőle nyerni. De az információt át kell fogalmazni, hogy megfeleljen egy kommunikációs protokollnak. Ennek is van egy költsége. Viszont nem egy átalakításon és szűrűn megy keresztül az információ, hanem számtalanon. És még csak ki sem nyitottuk a szánkat.

Ezek közül bármelyiknek lehet akkora költsége, hogy túl fárasztó lenne át verni rajta egy információt. Vagy egyszerűen blokkolja az információ megosztását. Ezt a koncepciót informatikában proxynak nevezzük. De Te lehet úgy ismered ebben a helyzetben, hogy komfortzóna, delegáció, vagy már mi más, ami hasonló feladatott lát el.

Viszont azt feltételezzük, hogy ez egy darab kis szűrő. A valóság az, hogy számtalan kis szűrő egymás után. Ami egy úgynevezett proxy chain-t (proxy lánc) hozz létre. És mindegyiken át kell mennie az információnak, hogy ebből kimenet legyen.

## Optimalizálás

Szóval van számtalan kis szűrű a fejünkben, ami akadályozza az információ áramlását. Mit lehet tenni ellene? Ha most csoda módszert és csoda szert vársz sajnos csalódást kell okoznom.

Nem tudunk minden szűrűt ki kapcsolni. De a már meglévőkön tudunk javítani. Egyeseket meg tudunk kerülni. Vagy meg tudjuk változtatni a sorrendjüket. Sőt gyorsíthatunk is rajtuk. 

Mi az első lépés? Azonosítani kell az egyes szűrőket. De nem mindet! Úgysem fogjuk elsőre megtalálni mindet. Ha megtaláltunk egyet, akkor lehet rajta javítani. Mondjuk, úgy hogy kiszervezzük, delegáljuk a feladatát. Vagy pedig egy új munka folyamatot hozunk számára létre, vagyis pipeline-t építünk neki.

- De hiszen ez sok hozzáadott munka. Miért lenne ez gyorsabb? Vagy jobb? És hogy jön ez a tartalom több nyelvűsitéséhez?

Ezek nagyon jó kérdések. (Lennének, ha nem én magam tenném fel őket saját magamnak.) A problémám nem az információ meglétével van, hanem annak átadásával. Ha ki iktatok egy szűrűt nagyobb eséllyel jut át az információ.

## Pipelines

Ha már valamilyen formában testet tud ölteni egy ötlet. Onnantól kezdve egy ToDo lesz belőle. A ToDokhoz pedig ki lehet alakítani egy munka folyamatot, hogy abból valami más legyen. Az egyes rész feladatokat pedig ki lehet adni másoknak, vagy pedig optimalizálni lehet rajta. 

Szóval az, hogy több nyelvűsitem a tartalmamat, nem visszalépés, hanem egy új munka folyamat kezdete. Adjunk ki valamit. Aztán a produktumot már csak át kell fordítani egy hasznosabb nyelvre. 

És idő közben van lehetőség további proxy-kat azonosítani és ha van rá mód kiszervezni vagy fejleszteni rajtuk.

Ezt a gondolat menetet Édes Apám egyik mondásával szeretném zárni.

"A munkához mindig úgy kell hozzáállni, hogy más is hozzáférjen."