# Word Root Field Guide 🪶

An English word root tracker for students in secondary ELA classrooms. Students build a personal running list of word roots and migratory (loan) words they encounter in the wild (reading done in class or elsewhere), logging where they found them and what they mean, and tracking their collection over time.

Inspired by the [ABA bird checklist](https://checklist.aba.org/). This was built with the idea that a good field guide is something you return to, add to, and feel a sense of accomplishment and ownership  over.

---

## What it does

**For students:**
- Browse a teacher-curated master list of word roots organized by origin language (Greek, Latin, Old English, French, Arabic, Japanese, and more)
- Log encounters with roots in the wild — what word they found, where they found it, what it means
- Add new Migratory Words (whole foreign words borrowed into English) to the shared list
- See their personal running collection grow over time from any device
- Optionally check the scoreboard to see how their collection compares

**For teachers:**
- Add and remove roots from the master list
- Pre-load each root with an example word and breakdown explanation
- View all students' entries in a dashboard
- Reset student PINs if needed
- Export all student data as a CSV
- Reset the database at the end of the year (roots are preserved)

---

## Origin categories

Greek · Latin · Old English/Anglo-Saxon · German · Norse · French · Spanish · Italian · Portuguese · Dutch/Flemish · Arabic · Hebrew · Sanskrit/Hindi · Japanese · Exotics · Migratory Word

---

## Technical overview

- **Single HTML file** — no frameworks, no build tools, no dependencies
- **Vanilla JavaScript** — runs in any modern browser
- **Supabase** — free hosted Postgres database handles all data storage
- **No installation required** — open the file in a browser and it works

---

## Setting up your own classroom instance

### 1. Create a Supabase project
- Go to [supabase.com](https://supabase.com) and create a free account
- Create a new project
- Go to **Settings → API** and copy your **Project URL** and **anon public key**

### 2. Set up the database
- In your Supabase dashboard, go to **SQL Editor**
- Run the full setup script from [`MIGRATIONS.md`](MIGRATIONS.md)

### 3. Configure the app
- Download `index.html`
- Open it in a text editor and find the **CONFIGURATION** block near the bottom
- Replace the two placeholder lines with your credentials:

```javascript
const SUPABASE_URL = 'your-project-url';
const SUPABASE_KEY = 'your-anon-public-key';
```

### 4. Open and log in
- Open `index.html` in any browser
- Log in as `teacher` with the password `pleasechangethis`
- You will be prompted to change your password immediately
- Your default root list is seeded automatically on first login

### 5. Optional: host it online
- Drag your configured `index.html` onto [Netlify Drop](https://app.netlify.com/drop) for an instant URL
- Or host it anywhere that serves static HTML files

---

## Sharing with students

Once your file is configured and hosted (or just saved locally), students:
1. Open the file or URL in any browser
2. Click **New student? Register here**
3. Choose a username and 4-digit PIN
4. Their data is saved to your database and accessible from any device

---

## Upgrading

When a new version is released, check [`MIGRATIONS.md`](MIGRATIONS.md) to see whether any database changes are required. Most updates are drop-in replacements — download the new file, add your credentials, done.

---

## Demo

A public demo instance is available at [word-roots-demo link here]. Student accounts in the demo are limited to 30 and wiped nightly. Roots are preserved. It is safe to try out but not intended for real classroom use.

---

## Version history

| Version | Changes |
|---------|---------|
| v1.0 | Single-file localStorage version |
| v1.1 | Supabase backend, single-classroom install |
| v1.2 | Source language fields, PIN complexity rules, Migratory Word rename |
| v1.3 | Migratory Words restructured; students can add migratory words; scoreboard; EtymOnline links; CSV export; end-of-year wipe |
| v1.3.1 | Default password enforcement, warning banner, password validation |
| v1.3.2 | Required fields on log form; "Your Word's Meaning" label; teacher password hashed with SHA-256 |

---

## License

MIT License — see [`LICENSE`](LICENSE) for details.

---

## Credits

Designed by Kristin Nielsen for high school level ELA.
Built with the assistance of [Claude](https://claude.ai) (Anthropic).
The pedagogical framework, feature decisions, and instructional design are the author's own.

---

## Contributing

If you use this in your classroom and improve it, pull requests are welcome. If you find a bug, open an issue. If you just want to say it worked for your students, that's welcome too.
