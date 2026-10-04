# baobao: Learn Chinese (offline PWA)

A free, Duolingo-style Mandarin course that runs entirely in the browser and works offline.
No accounts, no servers, no tracking. Progress is saved on the device.

## Course

10 sections, 60 units, 730 words, 412 sentences (roughly HSK 1 to HSK 4).

| Section | Level | Units |
|---|---|---|
| 1. Foundations 基础 | HSK 1 | Greetings, Numbers, People, Food & drink, Daily life, Animals & colors |
| 2. Getting around 出行 | HSK 1–2 | Time, Places, Transport, Shopping, Weather, Phones & words |
| 3. Everyday talk 日常 | HSK 2 | Health, Routine, Hobbies, Feelings, At home, Asking |
| 4. Life & work 生活 | HSK 2–3 | Work, Restaurant, Travel, Clothes, Family, Plans |
| 5. Stories & ideas 故事 | HSK 3 | Comparing, Nature, Studying, Online, Storytelling, Culture |
| 6. City life 城市 | HSK 3 | Little words, Neighborhood, Directions, Errands, Housing, When & how often |
| 7. People 人际 | HSK 3 | Looks, Personality, Relationships, Conversation, Invitations, Moods |
| 8. Work & money 经济 | HSK 3–4 | Job hunting, At the office, Money matters, Business, Technology, On the move |
| 9. Society 社会 | HSK 4 | Environment, In the news, Rules & safety, At the clinic, Knowledge, Arts |
| 10. Expressing ideas 表达 | HSK 4 | Opinions, Linking ideas, How much, Dreams & goals, Idioms, Traditions |

**Jump here:** any locked section can be unlocked with a 15-question test on everything before it.
Two mistakes are allowed and the third ends the test. Regular hearts aren't used, and XP counts even on a fail.
New learners who pick "I already know some Chinese" during setup land on the sections screen.

## What's inside

    index.html             the whole app (HTML, CSS, JS and course content in one file)
    manifest.webmanifest   install metadata (name, icons, colors)
    sw.js                  service worker: caches the app for offline use
    icons/                 home-screen icons (regular + maskable + Apple)

## God mode (for testing)

Profile → Settings → **God mode**. While it's on:

- Every lesson, chest, trophy and section is open, and hearts are unlimited (shown as ∞, with a ⚡ badge on the level ring).
- Each exercise shows **✓ Solve**, **✗ Miss** and **⏭ Finish** buttons. Keyboard: `S` solve, `X` miss, `F` finish.
- Listening exercises appear even without a Chinese voice installed, so every exercise type can be checked.
- Tapping a path node adds **Mark done up to here** or **Set progress back to here**.
- Locked sections get an **Open section** button next to Jump here.
- A **Testing tools** panel in Profile adds +500 XP, +500 jade, refills hearts, adds a streak day,
  marks every lesson done, or resets the path.

Jump tests keep their 3-mistake rule so the fail screen can still be tested (use Miss three times).
Everything done in God mode counts as real progress. Use Reset path or Reset progress to start clean.

## Put it online (needed for "install" and offline mode)

Service workers only run over HTTPS (or localhost), so host the folder on any static host:

- **Netlify Drop**: drag this folder onto https://app.netlify.com/drop
- **GitHub Pages**: push the folder to a repo, then Settings → Pages → deploy from branch
- **Cloudflare Pages / Vercel / any web server**: upload the folder as-is

Open the URL once while online. After that it works with no connection.
Chrome/Edge/Android show an **Install** button in the app (Profile tab, or the right-hand panel on desktop).
On iPhone/iPad: Safari → Share → **Add to Home Screen**.

To test locally: `python3 -m http.server 8080` in this folder, then open http://localhost:8080

## Pronunciation audio

Audio uses the device's built-in text-to-speech, so it works offline once a Chinese voice is installed:

- **iPhone/iPad**: Settings → Accessibility → Spoken Content → Voices → Chinese
- **Android**: Settings → System → Languages → Text-to-speech → Google engine → install Chinese (Mandarin)
- **Windows**: Settings → Time & language → Speech → Add voices → Chinese (Simplified)
- **macOS**: System Settings → Accessibility → Spoken Content → System voice → Manage voices → Chinese

Without a Chinese voice, listening drills are hidden and everything else still works.

## Scoring

| Event | XP |
|---|---|
| Correct answer | +10 |
| Combo (3+ right in a row) | +2 to +10 extra per answer |
| Lesson complete | +20 |
| Perfect lesson | +20 |
| XP boost (shop) | ×2 for 15 minutes |

Hearts: 5 max, a mistake in a lesson costs one, one refills every 30 minutes. Practice never costs hearts.

## Adding content

Course data lives near the top of the `<script>` in `index.html`. Section 1 is in the `UNITS` array;
sections 2 to 10 are added with `UNITS.push(U(...))` in a compact format:

    U('u61', 'Title', '中文', 'Short description',
      [ ['汉字', 'hàn zì', 'Chinese character', '🈶'], ... ],          // hanzi, pinyin, English, emoji
      [ ['我 会 写 汉字', 'wǒ huì xiě hàn zì', 'I can write Chinese characters'], ... ])  // tokens split by spaces

Rules: pinyin has one syllable per character, every sentence token must be a word from that unit or an earlier one,
and English meanings should be unique. Every 6 units form a section; add an entry to `SECTIONS` for each new group of 6.
After editing, bump `VERSION` in `sw.js` so installed copies pick up the change.
