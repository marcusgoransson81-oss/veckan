# Hugos vecka – så uppdateras sidan

Publiceras via GitHub Pages: https://marcusgoransson81-oss.github.io/veckan/

- `index.html` är mallen och ändras sällan. Allt veckoinnehåll ligger i `data.json`.
- Varje söndag, efter föräldrarapporten (Skolrapport Melvin & Hugo på claude.ai): skriv om `data.json` för kommande två veckor och pusha till `main`.

## Regler för innehållet
- Skrivet direkt till Hugo (åk 8), peppande och coachande, inte pressande. Inga betyg, merit eller frånvaro.
- Inga minutsiffror eller summeringar av plugg. Inga formuleringar om "ingen plugg"/"ledigt från plugg".
- Princip: börja tidigt, lite i taget, utspritt. `borja` på provkort sätts gott om tid före provet.
- Gymnasiet: han siktar på NA men det är inte säkert att han kommer in. Prata om flera alternativ.
- Ämnen med prov snart får detaljerade checklistor i `grupper` (rubrik + kunna). Bygg dem från lärarens lista i Classroom/planeringen, hitta inte på omfattning som inte är känd.
- Checkrutornas lagring nycklas på ämne+datum+text, så oförändrade punkter behåller sina bockar.
- NotebookLM-recept (`notebooklm`, `notebooklm_extra`, `notebooklm_tips`) och studietips (`veckans_teknik`, `teknik_bank`) anpassas efter vilka ämnen och prov som är aktuella.
- `nytt` = förändringar sedan förra söndagen.

## Bockar och föräldravy
- Bockar synkas till Supabase (projekt wpjgybjmezxorhltbdcr, tabell `checks`: key, done, updated_at). Publishable key ligger i index.html och koll.html; RLS tillåter bara select/insert/update på nycklar som börjar med `k-` eller `p-`.
- Nycklar: `k-<ämne>-<provdatum>-<hash av punktens text>` och `p-<datum>-<index>`. Behåll texten på en punkt oförändrad om bocken ska ligga kvar.
- Föräldravy: https://marcusgoransson81-oss.github.io/veckan/koll.html. Läs den (i webbläsaren) på söndagar för att se vad Hugo bockat av och anpassa veckan.
- Marcus har en fäst kopia på claude.ai ("Hugos vecka", artifact 316828c0-ea2e-4f8f-a583-1ac9d7cc0035, delad publikt). Uppdatera den också varje söndag med samma data.json och index.html, men med "Pappas kopia"-raden överst (länkar till sidan och koll.html) och utan `await sync();` (claude.ai blockerar anrop till Supabase).
- Vid söndagsuppdateringen: behåll gamla `dagar`-poster och provposter i data.json tills provet de hör till är gjort (koll.html använder dem för att visa progress). index.html visar bara innevarande och kommande veckor och kommande prov; koll.html döljer automatiskt prov som passerat och pluggpunkter vars prov passerat. Rensa bort passerade prov och deras dagar ur data.json när de inte längre behövs.
- koll.html visar en påminnelseindikator per prov (I fas / Dags att påminna / Påminn nu) utifrån dagar kvar och andel avbockat jämfört med förväntad takt från `borja` till provdatum. Sätt därför `borja` rimligt på varje prov.
