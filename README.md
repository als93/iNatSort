# iNatSort

A keyboard-driven tool for clearing out the iNaturalist "Unknown" pile.

Thousands of iNaturalist observations sit at "Unknown" because nobody has given them even a broad identification. Specialists who could name the species never see them, because they're buried in a feed nobody filters for. iNatSort shows you Unknown observations one at a time and lets you file each one under a broad group (plants, insects, birds, fungi, and so on) with a single keypress, so the right specialists can find it.

It's a single HTML file. There is no server, no build step, and nothing to install.

**Live version:** `https://als93.github.io/iNatSort/`

## What it does

1. Paste your iNaturalist API token in Settings.
2. Choose which observations you want: a place, a quality grade, how old they should be.
3. Look at the photo (or listen to the sound), then press a number key to post a broad identification such as "Insects" or "Birds". Press a letter key first if you also want to add an annotation such as life stage or sex.
4. A six-second countdown lets you undo. After that the identification is sent to iNaturalist in the background and checked.
5. Open "View recent posts & IDs" at any time to compare what you posted against what iNaturalist recorded.

You make every identification yourself. The tool never suggests one, and it posts only when you press a key or click.

## Getting started

You can browse without signing in, but you need an API token to post.

1. Open [inaturalist.org/users/api_token](https://www.inaturalist.org/users/api_token) while signed in to iNaturalist and copy the token. You can paste either the bare token or the whole JSON text.
2. Open Settings in iNatSort and paste it in.
3. Read and accept the short usage agreement the first time. It asks again whenever the wording changes materially.

Tokens expire after 24 hours, so you'll need a fresh one each day. The token stays on your device.

## Choosing what to sort

All of this is in Settings.

- **Quality grade.** Needs ID, Research, or Any.
- **Skip uploads newer than (days).** Leaves recent uploads for the observer and their friends to identify first. The default is 7 days; set it to 0 to include everything.
- **Places.** Leave empty for observations from anywhere. Otherwise type a place name or ID, or pick from the list of common regions. You can add more than one place. The common places are checked against iNaturalist before they're offered, and a preset only appears if a place with exactly that name exists.
- **Oldest first.** Sorts by observed date. When off, the newest uploads come first.
- **Set classification mode.** Normally the tool shows Unknowns only. With this on, it shows observations of a taxon you choose (and everything inside it) instead, so you can refine existing identifications rather than add the first one. Your quality grade setting still applies.

The tool only loads observations that are verifiable (date, location, and a photo or sound) and that you haven't already reviewed on iNaturalist. Observations are fetched 50 at a time.

## Posting identifications

| Key | Action |
| --- | --- |
| `1`–`9`, `0` | Open or post a group (see below) |
| `S` | Skip this observation |
| `Z` | Undo the last post (during the countdown) |
| `C` | Search for any taxon by name |
| `N` | Data quality vote: no evidence of an organism |
| `P` | Data quality vote: captive / cultivated |

**Groups.** Each number key is a group: Plants & Algae, Insects, Birds, Fungi & Slime Molds, Mammals, Reptiles & Amphibians, Fish, Arachnids, Mollusks, and Other. Most groups open a sub-menu (beetles, butterflies and moths, owls, and so on) so you can be more specific than "Insects" when you're sure.

**Searching.** Press `C` to find any taxon by name. If you pick something at genus rank or above, it's added to the matching group's sub-menu so it's one click away next time. Added entries have a small ✕ to remove them again.

**Annotations (optional).** Press a letter before you choose a group and it's posted together with the identification: `L` alive, `D` dead, `M` male, `F` female, `A` adult, `J` juvenile, `E` egg, `V` larva.

**Data quality votes.** `N` and `P` post a vote on the observation's data quality rather than an identification.

## Keeping your posts safe

This tool posts under your account, so it's built to be cautious.

- **Undo.** Every post waits six seconds before it's sent.
- **A queue that survives problems.** After the countdown, the post goes into an outbox saved in your browser. If you close the tab or lose your connection, it's still there and sends when you're back online. Failed sends retry with increasing delays, and rate-limit responses are respected.
- **No duplicates.** Before retrying a send it might already have made, the tool checks whether the identification already exists on iNaturalist.
- **Verification.** After a post goes through, the tool checks that iNaturalist recorded the taxon you chose. The header briefly shows `✓ Verified #123456` and the posted count pulses. If something doesn't match, it shows `⚠ Check #123456` and the entry is marked in Recent posts.
- **Unconfirmed taxon IDs are checked before sending.** Group IDs are looked up from iNaturalist and cached. Any built-in ID that hasn't been confirmed yet is verified once, just before it's posted, and corrected or held back if it's wrong.
- **A human-pace guard.** The tool won't accept a post faster than about one per second or more than 30 a minute. iNaturalist asks that tools not add content faster than a person reasonably could.
- **Recent posts.** "View recent posts & IDs" lists your latest identifications. Press "Check against iNaturalist" to compare each with the live record. A red "to check" count appears on the button if anything differs. A very recent post can take a minute to appear.

Please check your first few posts after any update, and correct or withdraw anything that doesn't match what you chose.

## iNaturalist's rules

iNaturalist allows tools to help you work out what something is, as long as you personally decide and post each identification. It treats unattended or machine-generated content as a suspendable offense. Don't point scripts, bots, or AI agents at this tool. The usage agreement in the app covers this in more detail and links to iNaturalist's Community Guidelines, Machine Generated Content policy, and Terms of Use, which are the authority and can change.

## Where the data comes from

All requests go straight from your browser to public services. There is no backend and nothing is collected.

| Service | Used for |
| --- | --- |
| [iNaturalist API](https://api.inaturalist.org/v1/docs/) | Observations, taxon and place search, annotation labels, posting identifications, annotations, and votes |
| iNaturalist photo hosts | Observation photos, taxon thumbnails |
| OpenStreetMap, Esri, OpenTopoMap | Map tiles for the small location map |
| [Leaflet](https://leafletjs.com/) | The map (loaded from unpkg with an integrity hash) |

The map is optional. If it can't load, the tool still works.

## Privacy and storage

Your token and settings are kept in your browser's `localStorage`, along with:

- the IDs of observations you've already sorted (the most recent 5,000), so they don't come back;
- a log of your recent posts (the last 200);
- the outbox of posts waiting to be sent;
- a saved copy of the current batch of observations (up to 7 days), used only if iNaturalist can't be reached;
- cached taxon and place lookups, and any groups you've added;
- whether you've accepted the usage agreement.

"Clear history" in Settings resets the list of sorted observations. Nothing is sent anywhere except the iNaturalist lookups and posts above.

A strict content security policy limits the page to the hosts listed above.

## Running it

Open `index.html` in a browser, or use the live version.

You need an internet connection to load observations and post. If you lose it partway through a batch, the tool keeps your queued posts and any observations already loaded, and sends the queue when you reconnect.

## Known limitations

- You need to be signed in to iNaturalist and paste a fresh API token each day to post.
- iNaturalist hides the exact location of sensitive species, so the map shows an approximate area for some records.
- Observations with no photo or sound, or with no date or location, aren't loaded.
- Choices are limited to the groups in the app, plus anything you add through search. Rarely used taxa need a search.
- The tool can't tell you whether an identification is right. That part is up to you.
- If two browser tabs are open, only one sends the outbox at a time.

## Credits

Observation data belongs to iNaturalist and its contributors, and each observation keeps its own license. Photos and sounds are shown for identification and linked back to the original record. Map tiles are © OpenStreetMap contributors, Esri, and OpenTopoMap.

## License

This project is licensed under the GNU General Public License v3.0 (GNU GPLv3). See the LICENSE file for full details.
