# Genesys DC Solutions — website

## Files

| File | What it is |
|---|---|
| `genesys-dc-solutions.html` | **The website.** One self-contained file — deploy it anywhere. |
| `genesys-deck-text.txt` | Text extracted from the source presentation. |
| `deck-pages/` | Every slide of the deck rendered to PNG (reference for the palette and content). |

## Deploying

It is a single static HTML file. Rename it `index.html` and drop it on any host:

- **Netlify / Vercel** — drag the folder onto the dashboard, or `vercel deploy`
- **GitHub Pages** — commit as `index.html` on a `gh-pages` branch
- **Any web server** — copy it into the document root

No build step, no dependencies, no framework. The only external request is
Google Fonts (Inter). Everything else — the globe, the data-hall model, the
charts, the icons — is drawn in the page itself.

## The enquiry form

Every CTA opens the enquiry dialog. Because a static page cannot open an SMTP
connection, the form **composes** the enquiry and hands it to the visitor's own
mail client, addressed to `hello@genesysdcs.com`.

To send server-side instead, find `form.addEventListener('submit', ...)` near the
bottom of the file. The `compose()` function above it already builds the payload.
Replace the mailto block with a POST to your own endpoint:

    var f = new FormData(form);
    fetch('/api/enquiry', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(Object.fromEntries(f))
    });

Then show the success panel on the response rather than before it, and change
the button label from "Compose enquiry" to "Send enquiry".

## Changing the contact address

One place: the `TO` constant in the enquiry script. The `mailto:` links in the
markup are the no-JavaScript fallback — update those too.

## Notes

- Brand colours are taken from the deck: black grounds, crimson `#8E0F1E`,
  signal red `#E4102A`. They are CSS custom properties in `:root`.
- The wordmark is live text, not an image — each `E` is three red bars, as in
  the deck.
- The page respects `prefers-reduced-motion`: the globe stops rotating, the
  scroll-driven hall sequence becomes buttons, and all tilt is disabled.
- No fabricated client names, project figures or track record appear anywhere.
  The dashboard and hall figures are labelled as illustrative.

## Photographs

Two photographs are referenced by the page. Save them into an `images/` folder
next to the HTML, with these exact filenames:

| File | Photograph | Appears in |
|---|---|---|
| `images/site-aerial.jpg` | Aerial of the campus under construction | High-Stakes Delivery (bleeds off the right edge) |
| `images/switchroom.jpg` | LV switchroom / switchgear lineup | MEP Engineering Excellence (bleeds off the left edge) |

JPEG, roughly 1600px on the long edge, quality ~80.

Until those files exist the page shows a neutral labelled plate in their place,
so nothing renders as a broken image. Both slots are graded into the dark palette
with a gradient scrim and carry a caption, following the deck's own treatment of
photography (image bleeding off one edge, copy on the opposite side).
