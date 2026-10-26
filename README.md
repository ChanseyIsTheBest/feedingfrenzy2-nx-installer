<div align=center>

<img src="icons/icon-192.png" alt="Feeding Frenzy 2" width="20%">

</div>
<h1 align=center>feedingfrenzy2-nx installer</h1>

The installer page of [feedingfrenzy2-nx](https://github.com/ChanseyIsTheBest/feedingfrenzy2-nx), the native Nintendo Switch
port of Feeding Frenzy 2 (Xbox Live Arcade): **https://chanseyisthebest.github.io/feedingfrenzy2-nx-installer/**

You give it your own copy of the game, and it gives you a zip ready to extract into `sdmc:/switch/`. Everything runs in
your browser: the game files never leave your computer.

## What it does

1. **Finds your game** in what you chose (files or a whole folder): its content package
   `D4F5B380AAE1D2A5DDF4359A318CE7F4DDE35C9958` (title `5841088C`, content type `0x000D0000`, read in place:
   `lib/from_sa2/stfs_package.js`), or a folder it was extracted into (`default.xex` with `Resource/resources.xpr`).
   Feeding Frenzy 2 has no title update and no add-on.
2. **Checks it** against `release/manifest.json`: `default.xex` by its SHA-256, the game's other files by their sizes.
3. **Downloads the build's NRO** and checks its SHA-256.
4. **Writes the game pack** (`game_root.nxpack` + `.nxidx`, `lib/nxpack.js`) from the game's files, its folders
   included.
5. **Streams the zip to disk** through the service worker, so nothing large has to fit in memory.

The game compiles all of its 26 shaders while it runs, with the Xbox's shader compiler, so none is stored in its
files and the page has no shaders to make: the NRO carries them.

The zip holds `feedingfrenzy2-nx/feedingfrenzy2.nro` with its `LICENSES.txt` and `game_root.nxpack` + `.nxidx`. The update zip
holds the NRO and its licenses. Neither holds `feedingfrenzy2.toml`: the game writes it at its first start, so an update never
overwrites your settings.

## Checks

- `test/manifest_node.mjs`: writes the build's entry of `release/manifest.json` from the tested files.
- `test/package_node.mjs`: the whole zip in Node with the page's code.
- `test/texts_node.mjs`: the interface texts.

## Layout

| Path | Contents |
|---|---|
| `index.html`, `app.js` | The page and its interface (`lib/i18n.js`) |
| `worker.js` | Builds the package off the main thread |
| `sw.js` | Offline cache and the download stream |
| `lib/` | STFS package reader, asset pack and zip writers |
| `release/` | The list of builds and the licenses of the NRO; the deployment adds the NRO from the feedingfrenzy2-nx release |
| `test/` | Node scripts that run the same code as the page |

## License

GPL-3.0 (see [LICENSE](LICENSE)). Third-party components are listed in [NOTICES.md](NOTICES.md). The page is built on
[bejeweled3-nx-installer](https://github.com/ChanseyIsTheBest/bejeweled3-nx-installer), itself built on
[peggle1-nx-installer](https://github.com/ChanseyIsTheBest/peggle1-nx-installer),
[peggle2-nx-installer](https://github.com/ChanseyIsTheBest/peggle2-nx-installer),
[futari-nx-installer](https://github.com/ChanseyIsTheBest/futari-nx-installer),
[sa2-nx-installer](https://github.com/ChanseyIsTheBest/sa2-nx-installer) and
[nfsmw-nx-installer](https://github.com/StevensND/nfsmw-nx-installer) by StevensND.

Feeding Frenzy is a trademark of PopCap Games / Electronic Arts; the icon artwork is © PopCap Games. This project is not
affiliated with or endorsed by Sprout Games, PopCap Games, Electronic Arts, Nintendo or Microsoft.
