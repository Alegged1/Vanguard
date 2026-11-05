# Vanguard — team showcase

The public roster site for **Vanguard**, a team in the Legions Minecraft PvP community.
It's a single static page (`index.html` plus `logo.png`) hosted free on GitHub Pages.

- Each player card shows a live 3D model of the player's Minecraft skin. It spins slowly, and visitors can drag it to turn it.
- Skins load by Minecraft username, so a card updates on its own when that player changes skin.
- The owner is always shown first. Everyone else is sorted by Quips rating, highest first. Players with the same rating are sorted alphabetically.

---

## How to update the site

### The easy way: edit mode

1. Open your site with `?edit` added to the end of the address, for example:
   `https://YOUR-USERNAME.github.io/Vanguard/?edit`
   (Visitors never see this. Without `?edit` the page is the normal public site.)
2. An **Edit roster** panel opens. The cards change as you type:
   - Click a card, or a name in the list, to open that member's details.
   - Change **Name on card**, **Minecraft username (skin)**, **Title**, **Quips rating** (0.1–2.0, leave it blank for none) or **Bio / quote**.
   - **+ Add member** adds a new card. **Remove member** deletes one.
   - Tick **Owner** to give someone the crown badge and put them first.
   - Under each member, a note says whether their skin loaded. If it says "Skin not found", check the username spelling.
3. Your edits are saved in this browser as you go, so a refresh won't lose them. They are **not public yet**.
4. Click **⬇ Download updated site**. This saves a new `index.html` with your changes built in.
5. Upload that file to GitHub:
   1. Go to your repository on github.com.
   2. Click **Add file → Upload files**.
   3. Drag the downloaded `index.html` into the box. It replaces the old one.
   4. Click **Commit changes**.
6. Wait about 1 minute, then refresh your site. You may need a hard refresh: Ctrl+F5, or Cmd+Shift+R on a Mac.

After your upload, when you open `?edit` again the saved draft matches the live site and is cleared on its own.
If you want to throw away edits you haven't published, click **Discard my edits** in the editor.

### The manual way: edit the file on GitHub

All team data sits in one clearly labelled block at the very top of `index.html`, between
`VANGUARD DATA START` and `VANGUARD DATA END`. On github.com, open `index.html` and click the ✏️ pencil to edit it.
Each member is one line:

```js
{ name: "Pillarger", skin: "Pillarger", title: "The Lion", rating: 1.8, bio: "First in, last out." },
```

| Field    | What it does |
|----------|--------------|
| `name`   | Name shown on the card |
| `skin`   | Minecraft username the skin is loaded from |
| `title`  | Text at the bottom of the card |
| `rating` | Quips rating from 0.1 to 2.0. Use `null` for no rating |
| `owner`  | Add `owner: true` for the crown badge and first place |
| `bio`    | Short bio or quote shown on hover (computer) or tap (phone) |

Keep the quotes `"` and the commas. Order doesn't matter, because the site sorts the cards itself.
Click **Commit changes** when you're done.

---

## How it works (for the curious)

- **3D skins**: [skinview3d](https://github.com/bs-community/skinview3d) 3.4.2, loaded from jsDelivr with unpkg as a backup.
  The page uses **one** WebGL context for all cards. A single hidden renderer draws each visible card in turn and copies the image onto that card.
  Cards that are off-screen aren't drawn at all, and slow devices automatically draw less often. This keeps phones smooth and stays well under browser WebGL limits.
- **Skin images** come from `mc-heads.net/skin/<username>`, with `minotar.net/skin/<username>` as a backup.
  If neither has the skin, the card shows a default Steve-style skin instead of breaking.
  If 3D isn't available at all (very old browser, or the CDN is blocked), each card shows a flat 2D render of the skin.
- Visitors who turn on "reduce motion" on their device see still models. They can still drag them.
