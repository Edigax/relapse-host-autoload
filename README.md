# relapse-host-autoload

PS5 exploit host by **Edigax**, built on [Relapse](https://github.com/soniciso1/relapse) — firmware **7.00 – 13.60** (10.60 and 11.40 excluded: those firmware images are missing the modules the offsets are read from).

One page, one button, one outcome:

```
Jailbreak  ->  relapse chain (WebKit bug + kernel exploit + elfldr on 127.0.0.1:9021)
           ->  automatically sends the PS5 WebKit Autoloader installer to 9021
           ->  done. nothing else happens
```

The autoloader installer is [itsPLK's PS5 WebKit Autoloader](https://github.com/itsPLK/ps5-webkit-autoloader). It opens the browser to cache the autoloader page and creates a **WebKit Autoloader** app on the homescreen — after that you launch the jailbreak from the homescreen shortcut and no longer need this page: **reboot once after a successful install, then launch WebKit Autoloader from the homescreen.**

## Hosting

Static site — hosted free on GitHub Pages.

**Live page:** <https://edigax.github.io/relapse-host-autoload/>
**Short URL:** <http://tiny.cc/relapse>

### Use it on a PS5 (quick steps)

1. On the PS5, connect Wi-Fi/LAN (relapse needs a network on every firmware).
2. Point the console at this page: set your network's DNS manually to a redirect service — or a self-hosted DNS server — that resolves the **User's Guide** (`manuals.playstation.net`) to this URL.
3. Open **Settings → User's Guide** → this page loads and shows your firmware version.
4. Press **Jailbreak** and don't touch the console. The log ends with: `WebKit Autoloader v0.6.2 installer sent` — the installer then creates the **WebKit Autoloader** app on the homescreen. Reboot once and launch it from there.
5. Firmwares 10.60 and 11.40 are not supported by relapse and are flagged on the landing page.

## Files

| path | purpose |
| --- | --- |
| `index.html` | landing page: firmware readout + jailbreak button |
| `relapse.html` | run page: relapse chain, then auto-sends the autoloader installer |
| `offsets/` | per-firmware offset tables (upstream, untouched) |
| `payloads/elfldr-ps5-1360.elf` | ELF loader the kernel handoff brings up on port 9021 |
| `payloads/kexp_2026_05_25.bin` | kernel shellcode for the kexp handoff |
| `payloads/wkal-installer.elf` | vendored WebKit Autoloader installer (stable filename) |
| `payloads/wkal-manifest.json` | version + sha256 of the vendored installer |
| `LICENSE` | GPL-3.0 — license of the vendored WebKit Autoloader installer, kept so redistribution is compliant |
| `.github/workflows/update-autoloader.yml` | daily cron: pulls the latest **stable** WebKit Autoloader release from itsPLK and bumps the vendored ELF + manifest automatically |

## Notes

- The `main.js` exploit file is unchanged from upstream to protect the proven chain; the only behavioural change lives in `relapse.html` (auto-send instead of payload menu).
- The workflow tracks stable releases only — pre-releases (e.g. `v0.7.0-prerelease-2`) are ignored. Manual trigger available (`workflow_dispatch`).
- For questions about the autoloader's own options (`autoload.txt`, Payload Manager), see the [upstream readme](https://github.com/itsPLK/ps5-webkit-autoloader#how-to-use).

## Credits

- **Host by [Edigax](https://github.com/Edigax)** — this hosted build (gradient UI, autoloader auto-send)
- **[soniciso1](https://github.com/soniciso1)** & contributors — [Relapse](https://github.com/soniciso1/relapse)
- **[itsPLK](https://github.com/itsPLK)** — [PS5 WebKit Autoloader](https://github.com/itsPLK/ps5-webkit-autoloader) (GPL-3.0; the vendored installer ELF is distributed with source link + license notice)
- **[jordyidk](https://github.com/jordyidk)** — [slopkit / Poops](https://github.com/jordyidk/slopkit)
- **[john-tornblom](https://github.com/john-tornblom)** — [ps5-payload-sdk](https://github.com/ps5-payload-dev/sdk) and [elfldr](https://github.com/ps5-payload-dev/elfldr)
- Full scene credit list on the landing page.

## Disclaimer

For research and development purposes only, on consoles you own. The developers are not responsible for any damage, data loss, or consequences resulting from the use of this software.
