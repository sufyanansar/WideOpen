# sufyanansar.github.io

Personal portfolio, and the public host for the [Nintex DocGen Field
Tagger](https://github.com/sufyanansar/nintex-docgen-tagger) Word add-in.

- `index.html` — the portfolio page.
- `nintex-docgen-tagger/` — the built add-in (`manifest.xml` + `index.html` +
  `assets/`). Regenerated from the add-in repo via `npm run build:ghpages`;
  never hand-edit the files in here.

## Publishing this for the first time

Run from this directory:

```bash
gh repo create sufyanansar.github.io --public --source=. --push
```

`gh` creates the repo, sets the remote, and pushes in one step. If you'd
rather do it manually:

```bash
git init
git add -A
git commit -m "Initial site + Nintex DocGen Field Tagger"
git branch -M main
git remote add origin https://github.com/sufyanansar/sufyanansar.github.io.git
git push -u origin main
```

Then in the repo's Settings → Pages, set the source to the `main` branch,
root folder. GitHub serves user sites (`<username>.github.io`) at the domain
root automatically — no further config, and it's usually live within a
minute or two.

## Updating the add-in after a code change

From the add-in repo (`app-plugins`):

```bash
npm run build:ghpages
```

Then copy the output over:

```bash
cp -r dist/* ../sufyanansar.github.io/nintex-docgen-tagger/
cp manifest.production.xml ../sufyanansar.github.io/nintex-docgen-tagger/manifest.xml
```

Commit and push from this repo. Colleagues who already sideloaded the
manifest don't need to re-upload — Word re-fetches `index.html` and the
assets on each open. If the manifest itself changed (rare — it only changes
when the icon set or task pane title changes), they will need to re-upload.

## Colleague setup (already covered on the site's repo card)

1. Word → Insert → Add-ins → More Add-ins → Upload My Add-in.
2. Paste: `https://sufyanansar.github.io/nintex-docgen-tagger/manifest.xml`
3. In Salesforce: Developer Console → Debug → Open Execute Anonymous Window,
   run `System.debug(UserInfo.getSessionId());`, copy the ID from the log.
4. Paste it into their org URL after `?sid=`, then paste that full URL into
   the add-in's Connect field.

No Connected App, no OAuth callback, no CORS entry required for the sideload
itself — Salesforce CORS still needs `https://sufyanansar.github.io` added
under **Setup → Security → CORS** once, so the REST calls the add-in makes
aren't blocked. That's a one-time step for whoever administers the org.
