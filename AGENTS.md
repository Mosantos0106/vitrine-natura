# Working on this repo

## What this is

A single-file static landing page (`index.html`, ~160 KB) for an independent Natura beauty
consultant's ready-to-ship showcase. All markup, CSS and JavaScript are inline in that one
file. There is no build step, no package manager, no backend, no database and no external
service credentials.

The entry file arrived as `index (2).html` (a browser "save page" artifact); it was renamed
to `index.html` so it can be served as a site root. Nothing inside the file changed.

## Running it

```bash
docker compose -f docker-compose.base44.yml up -d --build
```

`web` is stock `nginx:alpine` serving host port 3000. Nginx has no watcher, so after editing
`index.html` the preview needs a page reload (`reload_preview`) — the file itself is
bind-mounted, so no rebuild or restart is required.

The compose file bind-mounts only `index.html`. If product photos are added under `img/`
(the path the `PRODUTOS` comments use), add a matching read-only bind mount
(`./img:/usr/share/nginx/html/img:ro`) so they are served.

## Editing content

Everything a consultant needs to change lives in two inline `<script>` blocks near the bottom
of `index.html`:

- `CONFIG` — consultant name, WhatsApp number, Instagram, service hours, address, official store link.
- `PRODUTOS` — the product list (`demo: true` marks placeholder items; `foto` is a path under `img/`).

Filter chips are generated from the categories of available products; with two or fewer
categories the filter row hides itself. "Tenho interesse" buttons open `wa.me` links in a new
tab, so they only work once `CONFIG.whatsapp` is filled in.

## Verifying

`curl -sf http://localhost:3000/ | grep -q 'Pronta-entrega'` — the served page is the source
file, unbuilt. The demo products show placeholder photo blocks until real photos are added.
