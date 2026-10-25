# Ruby Achievements — community packs

[**English**](README.md) · [Português](README.pt-BR.md)

Achievement packs (`.rubydb`) for [Ruby Achievements](https://github.com/andrademats/ruby-achievements),
listed in the catalog that the app downloads on the Nintendo Switch.

In the app: open a game > **Achievements** > **Find community packs** (or **Manage / download other
packs**). The app filters the catalog by the game's Title ID, shows a compatibility badge and installs
the pack as `/switch/ruby_achievements/achievements/<TitleID>.<pack_id>.rubydb`.

| Badge | Meaning |
|---|---|
| Green — Compatible | the game's build is in `supported_build_ids` (or the pack has no list) |
| Yellow — Warning | same game, different build, or the build was not read yet (open the game once) |
| Red — Incompatible | installed game version below `game_version_min` |

## Layout

```
index.json                         catalog read by the app (raw.githubusercontent.com, branch main)
packs/<TitleID>.<pack_id>.rubydb   the packs
```

## Adding a pack

1. Make the `.rubydb` with the AchieveSet Studio or with `tools/json_to_rubydb.py` from the main
   repository. Packs with memory conditions must carry `supported_build_ids`.
2. Copy it to `packs/<TitleID>.<pack_id>.rubydb` (`pack_id`: `a-z`, `0-9`, `_`, `-`, up to 48).
3. Add or update the catalog entry (title, author, version, Title ID, build ids, achievement count and
   SHA-256 come from the pack itself):

   ```
   python -I tools/rubydb_index.py --index index.json --pack packs/<file>.rubydb \
       --url https://raw.githubusercontent.com/andrademats/ruby-achievements-packs/main/packs/<file>.rubydb \
       [--version-min 1.6.0]
   ```
4. Open a pull request. Any change to a pack needs the entry regenerated: the app refuses a file whose
   SHA-256 differs from the catalog.

## Rules

- Packs only: no game files, keys (`prod.keys`/`title.keys`), dumps, NSP/NSZ or executables.
- Text up to 64 KB per pack, icons PNG up to 256 KB and 512×512 (checked by the tools and by the app).
- One pull request per pack; describe what the achievements need and which game version you tested.
