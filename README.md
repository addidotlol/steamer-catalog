# steamer-catalog

The list of apps [steamer](https://github.com/addidotlol/steamer) offers in its Catalog. steamer fetches `catalog.json` from the `main` branch at startup and keeps the last copy it got, so a bad push only shows a warning until it's fixed.

## Adding an app

Add an entry to `items`:

```json
{
  "id": "moonlight",
  "name": "Moonlight",
  "description": "Stream games from your PC.",
  "homepage": "https://moonlight-stream.org",
  "tags": ["streaming"],
  "launchOptions": "",
  "source": {
    "type": "github",
    "repo": "moonlight-stream/moonlight-qt",
    "asset": "^Moonlight-.*-x86_64\\.AppImage$"
  }
}
```

- `id` is permanent. steamer records it on the handheld to know what's installed, so renaming it makes existing installs look like custom ones.
- `name` becomes the Steam shortcut's name and the AppImage's filename, so changing it later creates a second shortcut on the next update.
- `sgdb` is the SteamGridDB game id to pull artwork from (the number in `steamgriddb.com/game/<id>`). Without it steamer takes the first search result for `name`, which is usually right for well-known apps and sometimes picks a game with the same name.
- `launchOptions` are passed to Steam as-is. steamer adds `APPIMAGE_EXTRACT_AND_RUN=1 %command%` by itself on handhelds without FUSE 2.

Two source types are supported:

- `github` picks the first asset in the repo's latest release whose name matches the `asset` regular expression, and uses the release tag as the version. GitHub publishes a SHA-256 for each asset, and steamer verifies the download against it.
- `url` downloads a fixed URL. Add `sha256` and `version` so steamer can verify the file and show what's installed.

## Decky plugins

Decky plugins come straight from the Decky plugin store. `featuredDecky` lists store plugin names to pin at the top of the Decky tab, in that order.

## Testing changes

In steamer's Settings, set the catalog URL to `file:///path/to/steamer-catalog/catalog.json`, save, and the Catalog page reloads from your working copy. steamer rejects the whole file if any entry is invalid, and shows why.
