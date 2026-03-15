# Word Root Field Guide — Database Migrations

This file documents every database change made between versions.
Run the relevant script(s) in your **Supabase SQL Editor**.

All scripts use `if not exists` — they are safe to run even if you
are unsure which version you are currently on. Duplicate runs will
not cause errors.

---

## Fresh Install (any version)

If you are setting up for the first time, run this complete script.
It creates all tables and seeds the default teacher password.

```sql
-- Settings table (stores teacher password hash and app version)
create table settings (
  key text primary key,
  value text
);
-- Default password is "pleasechangethis" stored as SHA-256 hash
-- Change it immediately after first login via the Teacher Dashboard
insert into settings (key, value) values ('teacher_pwd', 'bb03304f366bc27d6ca9f3e14b5d1f67dc94d123c519ca81b58aa1226a6ee224');
insert into settings (key, value) values ('app_version', '1.3.2');

-- Roots table (the master word part list)
create table roots (
  id text primary key,
  part text not null,
  origin text not null,
  meaning text not null,
  example text not null,
  example_note text,
  is_prefix boolean default false,
  is_root boolean default false,
  is_suffix boolean default false,
  source_lang text,
  created_at timestamptz default now()
);

-- Users table (students)
create table users (
  username text primary key,
  pin text not null,
  created_at timestamptz default now()
);

-- Logs table (student word finds)
create table logs (
  id uuid primary key default gen_random_uuid(),
  username text references users(username) on delete cascade,
  root_id text references roots(id) on delete cascade,
  word text,
  definition text,
  found_in text,
  date_found text,
  source_lang text,
  source_lang_other text,
  created_at timestamptz default now(),
  unique(username, root_id)
);

-- Disable RLS for all tables (app handles auth)
alter table settings disable row level security;
alter table roots disable row level security;
alter table users disable row level security;
alter table logs disable row level security;
```

---

## Upgrading from v1.1 → v1.2

Adds source language tracking to roots and student logs.

```sql
alter table roots add column if not exists source_lang text;
alter table logs add column if not exists source_lang text;
alter table logs add column if not exists source_lang_other text;
```

---

## Upgrading from v1.2 → v1.3

No database changes required.
Download the new HTML file and replace your existing one.

---

## Upgrading from v1.3.1 → v1.3.2

Teacher password is now stored as a SHA-256 hash for security.
No structural database changes required — the app will automatically
upgrade your plain-text password to a hash the next time you log in.

After logging in, update the version:
```sql
update settings set value = '1.3.2' where key = 'app_version';
```

---

## Upgrading from v1.3 → v1.3.1

No database changes required.
Download the new HTML file and replace your existing one.

If you want to adopt the new default password behavior for future
installs, note that the seed password is now `pleasechangethis`
instead of `LittleGoldenBook699`. Your existing password is
unaffected — no action needed.

---

## Notes for future versions

When a new version requires database changes, a new section will
be added here with the migration script. Always check this file
before upgrading the HTML.

If you are ever unsure what version your database is on, run:
```sql
select value from settings where key = 'app_version';
```

After running any migration script, update the version in your database:
```sql
update settings set value = 'X.X.X' where key = 'app_version';
```
(Replace X.X.X with the version you just upgraded to.)

If you are ever unsure what version your database is on, it is
safe to run all migration scripts from the top — the
`if not exists` guards will skip anything already in place.
Then update the app_version to the latest.
