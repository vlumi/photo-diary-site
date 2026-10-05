# photodiary.misaki.fi

The website for **Photo Diary Companion**, the iPhone companion for a
self-hosted [Photo Diary](https://github.com/vlumi/photo-diary): landing,
screenshots, support and privacy pages, and the landing spot for pairing
links.

The app itself is open source at
[github.com/vlumi/photo-diary-ios](https://github.com/vlumi/photo-diary-ios).

## Build

Static site, built with [Hugo Extended](https://gohugo.io/). No JS, no external
assets — plain HTML/CSS, in the app icon's palette.

The Hugo version is **pinned** in [`.hugoversion`](.hugoversion). Hugo is a
build-time tool, not a runtime — there's no security reason to chase updates,
and a bump is the thing most likely to *break* the build. So pin it and update
**deliberately**: bump `.hugoversion`, run a local build, and commit only if
it's clean.

```sh
hugo server        # local preview at http://localhost:1313
hugo               # one-off build into ./public
./deploy.sh        # on the host: pull, build with the pinned Hugo, publish
```

`nginx.conf.example` is the server block; `deploy.sh` publishes to the web
root it names.

## Pairing links (Universal Links)

`static/.well-known/apple-app-site-association` lets the app claim
`https://photodiary.misaki.fi/pair` (`U84DT7P75P.fi.misaki.photodiary`). A
pairing link carries its server and one-time code after the `#`
(`/pair#host=…&token=…`), so the code never reaches this site's logs. With
the app installed, iOS opens the app; without it, [`content/pair.md`](content/pair.md)
explains what to do. iOS fetches the association file over HTTPS without
redirects, served as JSON (see the nginx example).

## Screenshots

The gallery reads its pictures from `assets/img/shots/<name>.png` and resizes
them at build time; a name without a file shows a labeled placeholder. The
names are those of the app's `Scripts/asc/shots.json` (`map`, `calendar`,
`photo`, `todo`, `todo-list`, `front`): copy `shots/iphone/en/<name>-iphone.png`
from the app repo under those names and rebuild.

## Privacy page

[`content/privacy.md`](content/privacy.md) is the app repository's
`PRIVACY.md` under a front matter; when that changes, copy it over again rather
than editing it here.
