# Tombola Ninety

A 90-ball bingo blower for a phone. Tap once and the machine does the rest:
the blower fires, the gate opens, a ball drops into the glass chute showing
only its colour, then lands and fills the screen with its number, a chime
pitched to its decade, and a burst of confetti that falls behind it.

**Live:** https://mekitoji92.github.io/Tombola90/

Drawn balls leave the pool. The game is kept in `localStorage`, so a refresh
mid-session does not lose the calls. Reset (top right) asks once before
returning all 90 to the drum.

## Colours

Ten balls per colour, nine decades, each ringing one step further up a major
pentatonic:

| 1–10 | 11–20 | 21–30 | 31–40 | 41–50 | 51–60 | 61–70 | 71–80 | 81–90 |
|---|---|---|---|---|---|---|---|---|
| red | orange | gold | lime | emerald | cyan | azure | violet | magenta |
| A4 | B4 | C♯5 | E5 | F♯5 | A5 | B5 | C♯6 | E6 |

Each number is announced with its traditional call — *Two fat ladies*,
*Clickety click*, *Top of the shop*. 6 and 9 are underlined, as on a real set.

## Files

| File | What it is |
|---|---|
| `tombola-ninety.html` | **The source.** Edit this. |
| `build.py` | Wraps the source in an HTML skeleton → `index.html`. |
| `index.html` | **Generated.** What GitHub Pages serves. Do not hand-edit. |
| `.nojekyll` | Tells Pages to serve the files as-is. |

`tombola-ninety.html` is authored as Claude Artifact *content*: it has no
`<!doctype>`, `<html>`, `<head>` or `<body>` of its own, because the Artifact
platform adds them at publish time. `build.py` adds an equivalent wrapper so
the identical page works as a plain static site.

The page is fully self-contained — one file, no build step beyond that
wrapper, no dependencies, no network calls except the Google Fonts
stylesheet.

## Working on it

```bash
python build.py
python -m http.server 8777
```

Then open <http://localhost:8777/>.

Commit both `tombola-ninety.html` and the rebuilt `index.html` — Pages serves
the generated file, so a source change that is not rebuilt will not ship.

## Notes

- **No external assets.** The page also runs as a Claude Artifact, whose CSP
  blocks everything but scripts from a short CDN allowlist and stylesheets
  from Google Fonts. The sound is synthesised with the Web Audio API; the
  machine and the confetti are canvas. Keep it that way and the two targets
  stay in sync.
- **The draw runs on the wall clock, not the render loop.** `seq.start` plus a
  `setTimeout` drive the reveal, so a throttled or backgrounded tab cannot
  strand a draw half-finished. The animation is cosmetic — never hang state
  off a frame firing.
- **`prefers-reduced-motion`** shortens the sequence and skips the confetti.
  The sound still plays.

## Related

`../bingo90.py` in the parent folder generates the printable ticket strips
this caller is played against. It is not part of this repository.
