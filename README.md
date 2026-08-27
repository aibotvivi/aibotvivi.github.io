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
