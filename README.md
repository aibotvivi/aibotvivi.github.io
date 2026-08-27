# aibotvivi.github.io

GitHub Pages user site. Each folder is served as a path.

| path | what it is |
|---|---|
| `/innerwaves/` | InnerWaves — static preview of the almanac interface |

## /innerwaves — read this before assuming it is broken

This is the **front end only**. The real app is an interface over a Node server that
computes the charts (four pillars from real solar terms, a tropical chart, 卦氣) and asks
a model to write the readings. GitHub Pages serves static files and cannot run that
server, so on this URL:

- the layout, typography, themes and every screen render correctly;
- sign-in, readings, the birth chart, the daily notes and payments **do not work** —
  they call an API that does not exist at this origin.

It is a design preview, not a working deployment. The working app runs on a private host
with the server behind it.

## Deliberate differences from source

This copy is not byte-identical to `index-almanac.html` upstream. Two changes, both made
because this is a public preview and neither backported:

1. The tailnet hostname was removed from a code comment.
2. The light/dark identity toggle is enabled here. Upstream it is fenced to localhost and
   the tailnet — it is an experimental A/B switch marked "remove before merge" — so on a
   normal host no button appears. The preview widens the guard to `*.github.io` so both
   identities can be compared. The dark theme's CSS was always present either way.
