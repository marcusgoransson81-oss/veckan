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
- Söndagsuppdateringen lägger in två veckor framåt (kommande vecka + veckan efter). På lördag/söndag visar index.html kommande vecka som första flik och lägger helgens `helg`-post (lör/sön före) först i den fliken som "Idag · helgen", så helgens pass inte försvinner. Plugg-bockar nycklas på datum+index, så ändra inte ordningen på redan avbockade pass.
- Sätt `uppdaterad` (datum) och `uppdaterad_tid` (ÅÅÅÅ-MM-DDTHH:MM, svensk tid) vid varje uppdatering. index.html och koll.html visar den överst som "Uppdaterad: Lördag 14/11 kl 09:49".
- Bockar visar att Hugo gått igenom något, inte att han kan det. Beröm arbetet, men påstå aldrig att ett ämne "sitter". Låt nästa steg vara att testa sig utan bok (genrep, quiz, förklara högt).

## Nya delar (från 15/11) – fyll i vid varje söndagsuppdatering
- **Testa dig själv**: varje prov med checklista ska ha en sista grupp `{"rubrik":"Testa dig själv","typ":"gora","kunna":[...]}` med 2–4 punkter: quiz/provgenrep på hela området, gör om det jag missade, genrep med datum (en lugn dag några dagar före provet). Anpassa till ämnet (matte: provgenrep med penna utan att titta i exemplen; muntligt språk: samtal utan manus). koll.html räknar "testat sig x av y" på rubriken exakt, och påminner om inget är testat 4 dagar före.
- Alla `grupper` får `"typ":"gora"` (en bock per punkt). Ändra inte texten på befintliga punkter, då försvinner bocken.
- **Länkar**: `prov[].lankar` = `[{t, url}]` till det faktiska materialet i Classroom (URL `https://classroom.google.com/c/<kurs>/m|a|p/<base64(stream-item-id)>/details`, utan `/u/1/`, Hugos Chromebook har bara skolkontot). Hämta id:n från `[data-stream-item-id]` på kursens flöde/klassuppgifter. Pluggpass får `"lank"` när det finns ett material som passar; annars ingen länk. Hitta inte på länkar. Kurs-id: kemi ODkxNzgyNDg3MjA5, matte MTYyNjE2NTQ1NTMy, franska NzAzODg1NjQxMTY0, fysik MjIzMjAwMDY5NzRa, SO MTYyNTg2OTMwMDIy, svenska MTYyNzE3MDM3NTUy, engelska MTYyNzE3MjgzNTA5, HKK NjUzMjEzMjg0ODU0.
- **Så ser ett starkt svar ut**: `prov[].toppoang` = `{fraga, e, a, skillnad[], not}` för prov där innehållet är känt. Exemplet ska vara sakligt korrekt och bygga på lärarens lista; `not` ska säga att det inte är lärarens bedömningsmall. Hoppa över om omfattningen är okänd.
- **Efter provet**: index.html visar automatiskt en ruta för prov som var för 1–21 dagar sedan (svaren sparas i Supabase-tabellen `reflektioner`: key `r-<ämne>-<provdatum>-<bra|miss|nasta|resultat>`, amne, provdatum, fraga, svar, updated_at; anon får select/insert/update, ej delete). Behåll därför provposten i data.json i 21 dagar efter provet. Läs svaren (koll.html visar "Hugos tankar efter proven") och använd dem: nämn det han själv skrev om missar i `fokus`/`hej` inför nästa prov i samma ämne, och anpassa Testa dig själv efter det.
- **Övriga ämnen**: `ovriga` = `[{amne, vad, url}]` för ämnen utan prov i närtid: pågående uppgifter, material, datum. Bara det som syns i Classroom/Unikum/Edlevo.
- **Frågor till läraren**: `fragor` = `[{amne, fraga}]`. Bocken sparas som `k-fraga-<hash av frågan>`. Ta bort en fråga när den är besvarad (syns på koll.html eller i källorna) och lägg in svaret där det hör hemma (t.ex. i provets checklista). Ersätter `kolla`, som kan lämnas tom.
- **Repetition efter prov**: lägg ett kort repetitionspass ca 2 veckor efter varje prov (quiz på det han missade), som `ovrigt`-post när det ligger utanför de två veckorna och som pluggpass när det kommer in.
- **Idag**: visas automatiskt överst (dagens kort, eller helgkortet på helgen). Inget att fylla i.
- Pappas kopia på claude.ai: samma index.html med "Pappas kopia"-raden och utan `await sync();`. Där kan bockar och svar inte sparas.

## Bockar och föräldravy
- Bockar synkas till Supabase (projekt wpjgybjmezxorhltbdcr, tabell `checks`: key, done, updated_at). Publishable key ligger i index.html och koll.html; RLS tillåter bara select/insert/update på nycklar som börjar med `k-` eller `p-`.
- Nycklar: `k-<ämne>-<provdatum>-<hash av punktens text>` och `p-<datum>-<index>`. Behåll texten på en punkt oförändrad om bocken ska ligga kvar.
- Föräldravy: https://marcusgoransson81-oss.github.io/veckan/koll.html. Läs den (i webbläsaren) på söndagar för att se vad Hugo bockat av och anpassa veckan.
- Marcus har en fäst kopia på claude.ai ("Hugos vecka", artifact 316828c0-ea2e-4f8f-a583-1ac9d7cc0035, delad publikt). Uppdatera den också varje söndag med samma data.json och index.html, men med "Pappas kopia"-raden överst (länkar till sidan och koll.html) och utan `await sync();` (claude.ai blockerar anrop till Supabase).
- Vid söndagsuppdateringen: behåll gamla `dagar`-poster och provposter i data.json tills provet de hör till är gjort (koll.html använder dem för att visa progress). index.html visar bara innevarande och kommande veckor och kommande prov; koll.html döljer automatiskt prov som passerat och pluggpunkter vars prov passerat. Rensa bort passerade prov och deras dagar ur data.json när de inte längre behövs.
- koll.html visar en påminnelseindikator per prov (I fas / Dags att påminna / Påminn nu) utifrån dagar kvar och andel avbockat jämfört med förväntad takt från `borja` till provdatum. Sätt därför `borja` rimligt på varje prov.

## Föräldrarapporten (Skolrapport Melvin & Hugo)
- Publik kopia för Marcus och Tina: https://marcusgoransson81-oss.github.io/veckan/foraldrar-5592624a/ (index.html + skolrapport.pdf). Länka den inte från Hugos sidor.
- Varje gång rapporten på claude.ai (artifact be28c770-ced9-4f76-ba8d-6fa2465f8cf4) uppdateras: hämta dess index.html, lägg in `<meta name="robots" content="noindex,nofollow">`, PDF-länken i undertitelraden och raden `<div class="upd">Uppdaterad: Veckodag D/M kl HH:MM</div>` först i `<main>` (stil `.upd`), spara som foraldrar-5592624a/index.html, lägg den nya PDF:en som skolrapport.pdf och pusha till main.
