# aoughwl store

The official store for [Jester](https://aoughwl.com): the apps and mods the
Jester shell offers under its **Store** tab. The shell reads `store.json` from
this repository's `main` branch.

Anyone can run a store of their own. It is one JSON file at any address a
browser could fetch, and players add it in the shell under **Store > Stores**
(paste the address of the `store.json`, or `github.com/owner/repo` for a
repository that keeps one on its main branch). Stores are independent: this
one is only the default.

## `store.json`

```json
{
  "format": 1,
  "name": "Aoughwl",
  "apps": [
    {
      "id": "minecraft",
      "name": "Minecraft",
      "detail": "One line about it.",
      "version": "0.1.0",
      "source": "https://raw.githubusercontent.com/<owner>/<repo>/main/content.json",
      "mods": ["aoughwl.ui", "aoughwl.voxel"]
    }
  ],
  "mods": [
    {
      "id": "aoughwl.voxel",
      "name": "Voxel",
      "detail": "One line about it.",
      "version": "0.1.0",
      "source": "https://raw.githubusercontent.com/<owner>/<repo>/main/content.json",
      "dependencies": [
        { "id": "aoughwl.ui", "version": "^0.1.0" },
        { "id": "aoughwl.sky", "version": "*", "optional": true }
      ]
    }
  ]
}
```

| field | meaning |
| --- | --- |
| `format` | always `1` |
| `name` | what the shell calls this store |
| `apps[]` | things a player opens: `id`, `name`, `detail`, `body`, `icon`, `image`, `version`, `source`, and `mods` (the mod ids it is made of) |
| `mods[]` | building blocks: the same fields, plus `dependencies` |
| `source` | the `content.json` that installs the item: the same committed manifest `tools/release.exe content` writes, listing every file at its real path. A store lists what exists; it never holds files itself. Several mods may share one `source` and are installed once. An item with no `source` is listed but cannot be installed. |
| `dependencies[]` | `id`, a `version` range (`*`, `^1.2.0`, `~1.2.0`, `>=1.2.0`, or an exact `1.2.3`), and `optional` |

When a player picks something, the shell works out everything it needs,
dependencies first, checks each version range against what the stores offer and
what is installed, and refuses (with the reason) rather than half-installing.

The repository must be **public** for the shell to read it without credentials.

## Featured entries

Any app or mod entry may carry `"featured": true`. The shell then marks it in
the Store with a gold star and a faint gold highlight (in the list and the grid).
Leave the field out, or set it to `false`, for an ordinary entry; a store with
no featured entries looks exactly as before.

## Mods made for particular apps

A mod entry may carry `"recommendedFor": ["Minecraft"]`. The shell then
recommends it only while an app whose name contains one of those words (either
way round, case aside) is being built or edited, under **Recommendations**;
for any other app it is left out of the list unless it is searched for by name.
Names are hints, not identities: nothing needs to be unique.

## Items that live on one machine

An item may carry `"local": "{data}/apps/mc"` instead of a `source`: it is listed but
not hosted, and nothing is downloaded for it. `{data}` is the player's `jester_data`
folder, which they can set under the cogwheel tab of the shell, so the same file works
on any machine that has the item.

Each entry has two descriptions. `detail` is the short one (a line or two, shown in the list). `body` is the long one, written in Markdown: headings (`#`, `##`, `###`), `-` bullets, numbered lines, `>` quotes, fenced code, `---` rules, and `**bold**`, `*italic*`, `` `code` `` and `[text](address)` inside a line. It shows under "ABOUT" in the side panel.

`icon` is a small picture for the entry: a `data:image/png;base64,...` address, or empty for none. A mod with no icon shows none; an app with none shows a letter on a colour. A player can pick a colour and a letter or symbol for either, from the star button beside Install, unless the store gives a picture. `image` is a larger cover picture, in the same format, shown under the short description.
