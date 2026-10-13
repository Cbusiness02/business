# Notities voor Claude

## Context
- Marktplaats-monitors (PS5, iPhone, MacBook, Switch 2, Horloges) in GitHub
  Actions; nieuwe advertenties gaan naar Discord (secret `DISCORD_WEBHOOK_URL`).
- Repo verhuisd van `gitdev0202/business` naar `Cbusiness02/business`
  (13-10-2026) vanwege het schedule-probleem hieronder. Start sessies alleen
  met `Cbusiness02/business` als repo: twee repo's met de naam "business" in
  één sessie gaan niet samen.

## Schedule-probleem (cron) — status 13-10-2026
- GitHub sloeg geplande runs (event `schedule`) structureel over: urenlange
  gaten, geen foutmelding. Hernoemen naar `_v2` hielp niet; handmatige runs
  (`workflow_dispatch`) werkten wel altijd.
- In de nieuwe repo werkt de planning weer automatisch, maar nog met gaten:
  geplande runs om 23:07 (PS5), 23:33 (iPhone, gepland :11) en 23:36 (PS5),
  terwijl PS5 op `*/5` staat. Daarom is daarna de lus hieronder aangezet.
- Oplossing (actief sinds 13-10-2026, op verzoek van de gebruiker):
  `monitor_loop.yml`. Eén job die ~5u40m zelf elke 5 min de monitors draait
  (PS5 elke ronde, de andere 4 elk uur gespreid), de state na elke ronde
  pusht en zichzelf daarna opnieuw start via `workflow_dispatch`. Uurlijkse
  cron is alleen vangnet; concurrency-groep `monitor-loop` voorkomt dubbele
  lussen. De `_v2`-workflows hebben geen eigen cron meer (alleen handmatig).

## Controleren
- Lus controleren: GitHub-API `list_workflow_runs` voor `monitor_loop.yml`
  op `Cbusiness02/Business`. Er hoort altijd één run `in_progress` te zijn;
  elke ~5u40m een nieuwe met event `workflow_dispatch`.
