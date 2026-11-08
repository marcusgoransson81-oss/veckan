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
