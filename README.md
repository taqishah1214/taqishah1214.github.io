# Taqi Shah · portfolio

A static site: one `index.html` (plus `resume.pdf` once the résumé links are switched back on). No build step and no server. three.js r128 loads from cdnjs and a few of its add-ons from jsDelivr; fonts come from Google Fonts. The music comes from Spotify's embedded player, with a lo-fi loop generated in the browser as the fallback, so there are no audio files either.

## What it is

- A short `dotnet test` run plays while the room builds, then you enter a 3D desk room at night.
- Drag (or swipe) to look around. Each prop opens a section:
  - the **monitor**: Projects. The AI platforms come first, then work and open source. Each project has an architecture diagram built from its real stack, plus a README-style write-up;
  - the **pinboard**: Experience, one lanyard ID badge per role that you flip to see what you did there, plus a timeline;
  - the **TV-head robot**: About. It sits at its own computer, typing. Click it and it stops, turns to you, and you can chat with it. The chat answers "do you have experience in X?" only from what is in the data block;
  - the **phone**: Contact, with copy buttons for email, GitHub and LinkedIn.
- On the desk: a large studio speaker with a "Music" marker over it that starts and stops the music (so does the Music button at the top right; browsers only play sound after a click), and a small Messi action figure that celebrates when clicked.
- Around the room: an African Grey parrot on its perch stand (click it), a couch, bookshelf, monstera, rug and wall art, and a window onto a city at dusk where people walk a dog, kids play catch, friends chat on a roof terrace and people work in a lit office.
- Background music from Spotify: the playlist in `ME.spotify` plays in Spotify's small player, which drops down under the Music button when you enter the room. The Music button and the speaker play and pause it, the × hides the player while it keeps playing, and the page remembers the choice. Spotify's rules: visitors signed in to Spotify in that browser hear full songs, everyone else 30-second previews, and Safari visitors press play in the player themselves.
  If Spotify can't load (a blocked network, or a preview page that doesn't allow other sites, like the claude.ai preview), the room plays its own lo-fi loop made in the browser and says so for a few seconds.
- The bottom bar jumps straight to any section. Links like `index.html#projects` open that section directly.
- Browsers without WebGL get the "flat version": every section on one page. Visitors can also switch to it from the corner link.

## Editing

Everything personal is in the block marked `EDIT ME` at the top of the main `<script>`:

- `ME`: name, roles, tagline, location, timezone, email, LinkedIn, Spotify playlist, résumé path, bio, skills, languages.
  `spotify` takes any Spotify playlist, album or track link (Share, then Copy link, in the Spotify app). It is Spotify's own "lofi beats" playlist for now; set it to `null` to use only the built-in loop.
  `resume` is `null` for now, which hides every Résumé link: the old PDF showed the phone number and listed Techverx as the current job. Put an updated `resume.pdf` next to `index.html` and set `resume: "resume.pdf"` to bring the links back.
- `PROJECTS`: what shows on the monitor. `kind` is "AI project", "Client work", "Open source" or the role you hold there; AI projects are grouped first. Set `repo` to link a GitHub repo.
- `MORE_REPOS`: other GitHub repos to mention (empty for now; the monitor's terminal shows the tagline instead of a repo list).
- `EXPERIENCE`: the ID badges on the pinboard, newest first.

The chat bot reads the same block, so adding a skill or a project there is enough for it to answer about it. It never claims anything that isn't listed.
`ph("...")` renders a highlighted placeholder if you want to mark something as unfinished.
Your phone number is left off the site on purpose; set `phone` in `ME` to show it on the phone card.

## Deploy

Any static host works. Easiest is GitHub Pages: push `index.html` (and `resume.pdf`, if you turn the links back on) to a repo named `taqishah1214.github.io` and turn on Pages in the repo settings. Vercel or Netlify also work by dragging the folder in.
