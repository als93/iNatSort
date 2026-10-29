<p align="center">
  <img src="assets/icon.png" width="96" alt="iNatSort icon">
</p>

<h1 align="center">iNatSort</h1>

<p align="center">
  <b>A keyboard-driven tool for clearing out the iNaturalist “Unknown” pile.</b><br>
  One HTML file. No server, no build, nothing to install.
</p>

<p align="center">
  <img alt="License: GPL v3" src="https://img.shields.io/badge/license-GPLv3-blue">
  <img alt="Single file" src="https://img.shields.io/badge/app-single%20HTML%20file-green">
  <img alt="No backend" src="https://img.shields.io/badge/backend-none-success">
</p>

<p align="center"><a href="https://als93.github.io/iNatSort/"><b>Open the live version</b></a></p>

---

## Why

Thousands of iNaturalist observations sit at “Unknown” because nobody has given them even a broad identification, so the specialists who could name the species never see them. iNatSort shows you Unknown observations one at a time and lets you file each under a broad group (plants, insects, birds, fungi…) with a single keypress, so the right people can find it.

You make every identification yourself. The tool never suggests one, and it posts only when you press a key or click.

## Getting started

You can browse without signing in, but you need an API token to post.

1. While signed in to iNaturalist, open [inaturalist.org/users/api_token](https://www.inaturalist.org/users/api_token) and copy the token (bare token or the whole JSON both work).
2. Open **Settings** in iNatSort and paste it in. Tokens expire after 24 hours and stay on your device.
3. Read and accept the short usage agreement. It asks again if the wording changes materially.

Then: look at the photo (or listen to the sound), press a number key to post a broad ID such as “Insects”, and optionally press a letter first to add an annotation. A six-second countdown lets you undo; after that the ID is sent in the background and verified.

## Keys

| Key | Action |
| --- | --- |
| `1`–`9`, `0` | Open or post a group (Plants, Insects, Birds, Fungi, Mammals, Reptiles & Amphibians, Fish, Arachnids, Mollusks, Other) |
| `S` | Skip |
| `Z` | Undo during the countdown |
| `C` | Search any taxon by name |
| `N` | Data quality vote: no evidence of an organism |
| `P` | Data quality vote: captive / cultivated |
| `L` `D` | Annotate alive / dead |
| `M` `F` | Annotate male / female |
| `A` `J` `E` `V` | Annotate adult / juvenile / egg / larva |

Most groups open a sub-menu (beetles, owls…) for when you can be more specific. Taxa you pick through search at genus rank or above are added to the matching sub-menu, with a ✕ to remove them again.

## Choosing what to sort

All in **Settings**:

- **Quality grade:** Needs ID, Research, or Any.
- **Skip uploads newer than (days):** leaves fresh uploads for the observer and friends. Default 7; 0 includes everything.
- **Places:** empty means anywhere. Type a name or ID, or pick a common region. Multiple places allowed. Presets are verified against iNaturalist before they’re offered.
- **Oldest first:** sort by observed date instead of newest uploads.
- **Set classification mode:** show observations of a chosen taxon (and everything inside it) instead of Unknowns, to refine existing IDs.

Only verifiable observations you haven’t already reviewed are loaded, 50 at a time.

## Safety

This tool posts under your account, so it’s built to be cautious.

- **Undo window:** every post waits six seconds before sending.
- **Outbox:** posts are saved in your browser and survive closed tabs and dropped connections. Failed sends retry with backoff and respect rate limits.
- **No duplicates:** before retrying an uncertain send, it checks whether the ID already exists.
- **Verification:** after posting, it checks that iNaturalist recorded the taxon you chose (`✓ Verified` or `⚠ Check`). Group IDs not yet confirmed are checked once more just before sending.
- **Human-pace guard:** you must look at an observation for a moment first, and posts are capped at 30 a minute.
- **Recent posts:** “View recent posts & IDs” compares your latest identifications with the live iNaturalist record.

Please check your first few posts after any update, and correct or withdraw anything that doesn’t match what you chose.

## iNaturalist’s rules

iNaturalist allows tools to help you work out what something is, as long as you personally decide and post each identification. It treats unattended or machine-generated content as a suspendable offense. Don’t point scripts, bots or AI agents at this tool. The in-app agreement links to iNaturalist’s [Community Guidelines](https://www.inaturalist.org/pages/community+guidelines), [Machine Generated Content policy](https://www.inaturalist.org/pages/machine_generated_content) and [Terms of Use](https://www.inaturalist.org/pages/terms), which are the authority and can change.

## Privacy and data

Everything goes straight from your browser to public services. There is no backend and nothing is collected. A strict content security policy limits the page to these hosts:

| Service | Used for |
| --- | --- |
| [iNaturalist API](https://api.inaturalist.org/v1/docs/) and photo hosts | Observations, taxa, places, posting IDs, annotations and votes |
| OpenStreetMap, Esri, OpenTopoMap | Map tiles for the small location map |
| [Leaflet](https://leafletjs.com/) (unpkg, integrity-pinned) | The map (optional; the tool works without it) |

Stored in `localStorage`: your token and settings, the last 5,000 observation IDs you’ve handled, a log of your last 200 posts, the outbox, a 7-day cached copy of the current batch (used only if iNaturalist is unreachable), cached taxon/place lookups, groups you added, and your agreement acceptance. **Clear history** in Settings resets the handled-observation list.

## Running it

Open `index.html` in a browser, or use the live version. You need a connection to load observations and post; if it drops mid-batch, queued posts are kept and sent when you’re back.

## Known limitations

- You need a fresh API token each day to post.
- iNaturalist obscures the location of sensitive species, so the map may show an approximate area.
- Choices are limited to the built-in groups plus anything you add through search.
- With two tabs open, only one sends the outbox at a time.
- The tool can’t tell you whether an identification is right. That part is up to you.

## Contributing

Bug reports and pull requests are welcome. Please mention what you pressed, what happened, and what you expected. For wrong posts, include the observation number.

## Credits

Observation data belongs to iNaturalist and its contributors; each observation keeps its own license. Icons: [Font Awesome Free](https://fontawesome.com/license/free) (CC BY 4.0) and Material Design Icons (Apache 2.0). Map tiles © OpenStreetMap contributors, Esri, OpenTopoMap.

## License

[GNU General Public License v3.0](LICENSE).
