# Festival Slash

A fruit-slicing style browser game with festival themes: slice the festive items, dodge the one you shouldn't touch.

**Themes**
- Diwali: crackers, spare the diya
- Christmas: gifts, baubles, candy canes, dodge the coal
- Halloween: pumpkins, ghosts, candy corn, dodge the skull
- Holi: colour balloons, keep the white kurta clean
- Eid: lanterns, stars, sweets, dodge the storm cloud
- Lunar New Year: red packets, lanterns, dumplings, dodge the unlucky clock
- Easter: painted eggs, don't hurt the bunny
- 
## Play
Open `index.html` in any browser. No build step, no dependencies. Works with mouse and touch.

## Rules
- Swipe through items to slice them. Chain slices for a streak multiplier (up to x5).
- Missing an item or cutting the "avoid" item costs one of 3 lives.
- Difficulty ramps up gradually over about 3 minutes: faster, smaller, more items.
- A rare golden bonus item starts a slow-motion double-points frenzy.
- Pause with the pause button or the P key. The game also pauses when you switch tabs.
- Enter your name on the start screen. Each theme shows the top 5 scores and the score to beat.
- Sound can be switched off with the speaker button. Your theme, sound setting and best scores are remembered.

## Host on GitHub Pages
Settings, then Pages, then deploy from the `main` branch root.

## Install on your phone (works offline)
Open the live page in Safari on iPhone, tap Share, then Add to Home Screen. On Android Chrome, use the menu, then Install app. After the first visit the game works without a network. Updates are picked up automatically the next time you open it online.

Files needed in the repo root: `index.html`, `manifest.json`, `sw.js`, `icon-192.png`, `icon-512.png`, `apple-touch-icon.png`. Keep them in the same folder.

## Leaderboard
By default scores are saved in the browser, so the board is per device. To share one board between all players, use a free Supabase project:

1. Create a project at https://supabase.com and run this in the SQL editor:

```sql
create table scores (
  id bigint generated always as identity primary key,
  name text not null check (char_length(name) <= 12),
  theme text not null,
  score int not null check (score between 0 and 100000),
  created_at timestamptz default now()
);
alter table scores enable row level security;
create policy "read scores" on scores for select using (true);
create policy "add scores" on scores for insert with check (true);
```

2. In `index.html`, find `const SUPA={url:'',key:''};` and fill in your project URL and the public `anon` key (Project Settings, then API). Never use the `service_role` key here.
3. Commit and push. Note that anyone can submit a score from a browser, so treat the board as for fun, not competition-proof.
