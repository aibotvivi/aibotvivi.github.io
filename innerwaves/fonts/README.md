# Self-hosted Hong Kong typefaces

Both families are by Tamcy, under the SIL Open Font License 1.1. `LICENSE.md` sits beside
each one and must stay there — the OFL requires the licence to travel with the fonts.

| Directory | Family (CSS name) | Upstream | Version |
|---|---|---|---|
| `chiron-sung/` | `Chiron Sung HK WS` | [chiron-fonts/chiron-sung-hk](https://github.com/chiron-fonts/chiron-sung-hk) | v1.024 |
| `chiron-hei/` | `Chiron Hei HK WS` | [chiron-fonts/chiron-hei-hk](https://github.com/chiron-fonts/chiron-hei-hk) | v2.609 |

## Why self-host at all

Noto Serif TC and Noto Sans TC — the Google-hosted faces this app used — draw to the
mainland/Taiwan standard. Chiron draws to **香港字形**, the shapes a Hong Kong reader
recognises as correct. The differences are small per character and constant across a
page: `兌` rather than `兑` inside `閱`, the inner stroke of `骨` turning left, `為`
keeping its four dots. For a Cantonese-first app it is the difference between type that
looks right and type that looks slightly foreign in a way most readers feel without
being able to name.

Google Fonts does not serve either family, so first-party hosting is the only route.

## Why this is only 20 MB, and why the browser downloads far less

A full CJK face is 30–90 MB. Upstream ships a **unicode-range subset build** — the same
mechanism Google Fonts uses for CJK — splitting each face into ~110 WOFF2 files of about
100 KB, each declared with the exact `unicode-range` it covers. A browser fetches only
the subsets containing glyphs the page actually paints, so a reading costs roughly
1–2 MB, not the whole family. Every face is `font-display: swap`, so text renders in the
fallback immediately and reflows when Chiron arrives.

Vendored here rather than fetched at build time because there is no build step.

## What was taken, and how to update

Upstream distributes a source repository, not a release archive — the Sung zipball is
**1.3 GB** and the Hei zipball **891 MB**, almost all of it design sources and static
instances this app never serves. Only the roman variable-weight WOFF2 build is copied:

```sh
V=1.024   # check upstream for the current tag
curl -L -o /tmp/sung.zip https://codeload.github.com/chiron-fonts/chiron-sung-hk/zip/refs/tags/v$V
unzip -q /tmp/sung.zip "chiron-sung-hk-$V/WOFF2_OTF/woff2/vf/t0/*" \
                       "chiron-sung-hk-$V/WOFF2_OTF/css/vf.t0.css" \
                       "chiron-sung-hk-$V/LICENSE.md" -d /tmp/sungx
cp -R /tmp/sungx/chiron-sung-hk-$V/WOFF2_OTF/{css,woff2} /tmp/sungx/chiron-sung-hk-$V/LICENSE.md \
      fonts/chiron-sung/
```

Chiron Hei is identical with `chiron-hei-hk` and its own tag. Do not commit the zip.

Two things to keep straight when picking directories out of the archive:

- **`vf`, not `vf-italic`.** The italic build is a second complete family. Nothing here
  sets italic Chinese, so it would double the weight to serve nothing.
- **`t0`, not `t1`.** These are orthography variants; `t0` is the Hong Kong shape set,
  which is the entire reason for using this font.

`css/vf.t0.css` references its WOFF2 files as `../woff2/vf/t0/…`, so the `css/` and
`woff2/` directories must stay siblings.

## How it is wired

`index-almanac.html` imports both stylesheets and puts Chiron **first** in every serif
and sans stack, keeping the Noto faces behind it as fallback for anything Chiron does not
cover. Thirteen headings in that file set their family in a `style=""` attribute and
cannot be edited — the file's markup is held byte-identical to `index.html` — so they are
reached with `html [style*="Noto Serif TC"]` and `!important`, the only thing that
outranks an inline declaration.

`server/static.mjs` already maps `.woff2` to `font/woff2` and caches non-HTML assets for
a day.
