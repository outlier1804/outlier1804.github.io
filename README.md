# outlier1804.github.io

The **user Pages site** for the `outlier1804` GitHub account. It exists for two reasons:

1. **It is a hub page** linking to every public tool on this account, so each of them has at least
   one inbound link from an indexable page.
2. **It owns the host root**, which is the only place a crawler will read `robots.txt` from.

## Why the host root matters

The five project sites are *project* Pages sites — they live at `outlier1804.github.io/<repo>/`. A
`robots.txt` committed inside one of those repos is served at
`outlier1804.github.io/<repo>/robots.txt`, returns HTTP 200, and is **read by nothing**: crawlers
only ever fetch `robots.txt` from the host root. Before this repo existed,
`https://outlier1804.github.io/robots.txt` was GitHub's "Site not found" page, so no `Sitemap:`
directive on this host was discoverable by anyone.

A repo named exactly `<username>.github.io` is served at the host root, which fixes that for every
site on the host at once.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The hub page. Plain static HTML, no build step, no dependencies. |
| `robots.txt` | Host-root robots. Declares the sitemaps for **all** sites on this host. |
| `sitemap.xml` | Host-root sitemap. A sitemap may list any URL on the same host, so this covers all five project sites. |
| `93b394e7519e402e9b016af81ceeb944.txt` | IndexNow key file (Bing / Yandex / Seznam). Do not rename or delete — the key inside must match the filename. |

## Maintenance

When a new page ships on any site on this host, add its `<loc>` to `sitemap.xml` here, and — if the
new site has its own sitemap — add a `Sitemap:` line to `robots.txt` here. Then re-ping IndexNow:

```sh
curl -sS -X POST https://api.indexnow.org/IndexNow \
  -H 'Content-Type: application/json' \
  -d '{"host":"outlier1804.github.io",
       "key":"93b394e7519e402e9b016af81ceeb944",
       "keyLocation":"https://outlier1804.github.io/93b394e7519e402e9b016af81ceeb944.txt",
       "urlList":["https://outlier1804.github.io/"]}'
```

A `200` or `202` means accepted. IndexNow needs no account and no API key registration — hosting the
key file at the URL above *is* the authentication.
