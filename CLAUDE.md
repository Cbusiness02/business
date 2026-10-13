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
  terwijl PS5 op `*/5` staat. De gebruiker vindt het werken zo voorlopig goed.
- Reservefix klaar maar NIET op main: `monitor_loop.yml` op branch
  `claude/clever-noether-630z4d` (commit `6b974e0`). Eén job die ~5u40m
  zelf elke 5 min de monitors draait en zichzelf daarna opnieuw start; haalt
  de cron uit de `_v2`-workflows. Alleen naar main zetten als de gebruiker
  dat vraagt, bv. als de planning weer urenlang wegvalt.

## Controleren
- Geplande runs opvragen: GitHub-API `list_workflow_runs` met event
  `schedule` op `Cbusiness02/Business`.
