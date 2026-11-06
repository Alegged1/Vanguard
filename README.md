# Vanguard — team showcase

The public roster site for **Vanguard**, a team in the Legions Minecraft league.
It's a single static page (`index.html` plus `logo.png`) hosted free on GitHub Pages.

- **Roster tab:** a card for every player. Each card has a live 3D model of the player's Minecraft skin, which spins slowly and can be dragged to turn it.
  The owner is always first, then managers, then everyone else by Quips rating (highest first). Players with the same rating are sorted alphabetically.
- **Players tab:** a full-screen profile for each player that you scroll through one by one. It shows a big 3D model, their rating and quote, and an **About** section.
  It can also show when they joined, their playstyle, achievements and links. Clicking a card on the roster jumps straight to that player.
  On a phone, the first tap on a card shows its quote and the second tap opens the profile.
- Skins load by Minecraft username, so a card updates on its own when that player changes skin.
  If a username has no skin, you can upload the skin file instead.
- Optional extras: a **Join our Discord** button and a **welcome message** under the header.

---

## How to update the site

### The easy way: edit mode

1. Open your site with `?edit` added to the end of the address:
   `https://alegged1.github.io/Vanguard/?edit`
   If you've set a **secret edit word**, use `?edit=YOURWORD` instead. Plain `?edit` then does nothing.
2. An **Edit site** panel opens. The page changes as you type:
   - **Team:** team name, tagline, welcome message (and who it's signed by), Discord invite link and secret edit word.
     Leave the Discord link or the welcome message empty to hide it.
   - **Members:** click a card or profile, or a name in the list, to open that member's details:
     - **Name on card**, **Minecraft username (skin)**, **Role** (Member / Manager / Owner), **Title**, **Quips rating** (0.1–2.0, leave it blank for none) and **Card quote**.
     - **Profile page:** **About** (each new line starts a new paragraph), **Joined**, **Playstyle**, **Achievements** (one per line) and **Links** (one web address per line). Links to YouTube, Twitch, TikTok and similar sites are labelled automatically.
     - **⬆ Upload skin file:** use this when a player's skin won't load from their username, for example because they use a different account. Pick their 64×64 skin `.png`.
       An uploaded skin is saved inside the site, but it won't update when they change skin in-game.
     - **View profile** previews that player's full-screen page. **Remove member** deletes them.
   - **+ Add member** adds a new player.
   - Under each member, a note says where their skin came from. "Skin not found" means you should check the spelling or upload the file.
3. Your edits are saved in this browser as you go, so a refresh won't lose them. They are **not public yet**.
4. Click **⬇ Download updated site**. This saves a new `index.html` with your changes built in.
5. Upload that file to GitHub:
   1. Go to your repository on github.com.
   2. Click **Add file → Upload files**.
   3. Drag the downloaded `index.html` into the box. It replaces the old one.
   4. Click **Commit changes**.
6. Wait about 1 minute, then refresh your site. You may need a hard refresh: Ctrl+F5, or Cmd+Shift+R on a Mac.

After your upload, when you open edit mode again the saved draft matches the live site and is cleared on its own.
To throw away edits you haven't published, click **Discard my edits** in the editor.

**Forgot your secret edit word?** On GitHub, open `index.html` and look near the top for `editKey: "..."`.
The word is between the quotes. You can also change it there with the ✏️ pencil, or clear it to go back to plain `?edit`.
The secret word only hides the editor. It can't publish anything, because the live site only changes when someone uploads to your GitHub repository.

### The manual way: edit the file on GitHub

All team data sits in one clearly labelled block at the very top of `index.html`, between
`VANGUARD DATA START` and `VANGUARD DATA END`. On github.com, open `index.html` and click the ✏️ pencil to edit it.
Each member looks like this:

```js
{
  name: "Pillarger", skin: "Pillarger", role: "member", rating: 1.8,
  title: "The Lion", bio: "First in, last out.",
  about: "Pillarger has been with Vanguard since day one.",
  joined: "March 2025", playstyle: "Sword main",
  achievements: ["Won Legions Season 2", "Top 3 in kills"],
  links: ["https://youtube.com/@pillarger"]
},
```

| Field          | What it does |
|----------------|--------------|
| `name`         | Name shown on the card |
| `skin`         | Minecraft username the skin is loaded from |
| `role`         | `"owner"`, `"manager"` or `"member"` |
| `rating`       | Quips rating from 0.1 to 2.0. Use `null` for no rating |
| `title`        | Text at the bottom of the card |
| `bio`          | Short quote shown on the card (hover on a computer, tap on a phone) |
| `about`        | Longer text on their profile page. Use `\n` for a new paragraph |
| `joined`, `playstyle` | Short facts shown on their profile page |
| `achievements` | List of wins or moments, each in quotes, separated by commas |
| `links`        | List of web addresses |

Keep the quotes `"` and the commas. Order doesn't matter, because the site sorts itself.
Click **Commit changes** when you're done.

---

## How it works (for the curious)

- **3D skins**: [skinview3d](https://github.com/bs-community/skinview3d) 3.4.2, loaded from jsDelivr with unpkg as a backup.
  The page uses **one** WebGL context for every model, on the roster cards and the profiles alike. A single hidden renderer draws each visible model in turn and copies the image onto that model's own canvas.
  Models that are off-screen aren't drawn, and slow devices automatically draw less often. This keeps phones smooth and stays well under browser WebGL limits.
- **Skin images** come from `mc-heads.net/skin/<username>`, with `minotar.net/skin/<username>` as a backup, unless you've uploaded a skin file for that player.
  If no skin is found, the card shows a default Steve-style skin instead of breaking.
  If 3D isn't available at all, each model is replaced by a flat 2D picture of the skin.
- The tabs are plain links (`#roster`, `#players`, `#players/<name>`), so you can share a link straight to one player's profile, for example
  `https://alegged1.github.io/Vanguard/#players/pillarger`.
- Visitors who turn on "reduce motion" on their device see still models. They can still drag them.
