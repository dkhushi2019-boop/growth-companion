# Growth Companion

A family app for Veer (iPad, India) and Mumma (iPhone and laptop, Dubai): daily plan and
school timetable, tasks, study, reading, sports and screen-free time, mood check-ins,
iPad screen time, messages, points and family rewards.

It is a web app that installs on the Home Screen. It needs no App Store and no Mac.

## How it is built

| Part | Where it runs | What it does |
| --- | --- | --- |
| `web/` | GitHub Pages | The app itself (works offline, installs to the Home Screen) |
| `supabase/sql/schema.sql` | Supabase database | Family data and the access rules |
| `supabase/sql/cron.sql` | Supabase scheduler | Test reminders, evening nudge, weekly summary, sending notifications |
| `supabase/functions/push` | Supabase server function | Sends notifications to iPhone, iPad and laptop |
| `supabase/functions/child` | Supabase server function | Lets the parent create or reset the child's login |
| `.github/workflows/deploy.yml` | GitHub Actions | Sets everything up and publishes on every change |

Privacy: a mood note the child keeps private can only be read from the child's account.
The child cannot change goals, limits or rewards, or approve his own reward requests.
The app never reads location, browsing, messages or photos.

## One-time setup

1. Create a public GitHub repository named `growth-companion` and set
   Settings → Pages → Source to **GitHub Actions**.
2. Create a free Supabase project named `growth-companion` (region: Mumbai).
3. Create a Supabase access token (supabase.com/dashboard/account/tokens) and save it in
   GitHub as the Actions secret `SUPABASE_ACCESS_TOKEN`.
4. Push this code to `main`. The workflow sets up the database, scheduled jobs,
   notification keys and server functions, then publishes the app at
   `https://<github-username>.github.io/growth-companion/`.

No keys are stored in this repository. The notification keys and the scheduler secret are
created on the first run and kept inside the Supabase database.

## Tests

```
node tests/db.test.mjs <node_modules with @electric-sql/pglite>      # access rules and reminders
node --experimental-strip-types tests/push.test.mjs <node_modules with http_ece>   # notification encryption
python3 tests/app.test.py                                             # parent + child in two browser tabs
```
