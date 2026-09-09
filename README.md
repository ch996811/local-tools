# Local Tools

Four small web tools that do their work on your own machine. **Your file, your
screenshot and your bank details are never uploaded to anything.**

Each one is a single HTML file with no build step, no framework, no bundler and
no backend. Open it from disk and it works.

| Tool | What it does | Live |
|---|---|---|
| [`iban-checker.html`](iban-checker.html) | Validates an IBAN against the official ISO 13616 mod-97 checksum, shows the country, bank code and account number, and explains exactly which check a bad IBAN failed. Knows the length and field layout of 28 countries. | [try it](https://snip2field.netlify.app/iban-checker/) |
| [`pdf-to-text.html`](pdf-to-text.html) | Extracts the text from a PDF. Pages that carry a real text layer are read out exactly, with no recognition step; only image-only scanned pages go through OCR, and the scan is lifted from the file at its original resolution rather than re-rendered. | [try it](https://snip2field.netlify.app/pdf-ocr/) |
| [`snip2text.html`](snip2text.html) | Paste or drop a screenshot and get the text out of it. | [try it](https://snip2field.netlify.app/snip2text/) |
| [`salary-clock.html`](salary-clock.html) | Shows what you are earning second by second, and what a break or a meeting cost. Settings stay in `localStorage`. | [try it](https://snip2field.netlify.app/salary-clock/) |

## How the privacy claim actually holds

There is no server to send anything to. All four tools are static pages:

- **Nothing is uploaded.** No `fetch` or `XMLHttpRequest` anywhere in these files
  carries your document, image or IBAN. You can confirm this by reading the
  source, or by opening the network tab and using the tool.
- **Nothing is stored server-side.** Salary Clock writes your salary to
  `localStorage` on your own machine; the others store nothing at all.
- **No accounts, no cookies, no consent banner.**
- **They work offline.** Once the page and its recognition engine are cached,
  disconnect and they keep working.

### Being precise about the two network requests that do exist

Honesty is the point of the list this belongs on, so:

1. **Recognition engines are fetched from a public CDN on first use.**
   `snip2text.html` and `pdf-to-text.html` load [tesseract.js](https://github.com/naptha/tesseract.js)
   from jsDelivr, and `pdf-to-text.html` also loads [pdf.js](https://github.com/mozilla/pdf.js).
   That is code coming *to* your browser. Your document does not go the other
   way. Vendor these two files locally if you want zero third-party requests.
2. **The hosted copies count anonymous page views** with Cloudflare Web
   Analytics (cookieless, no fingerprinting). It is one `<script>` tag near the
   bottom of each file — delete it and everything still works. It is not present
   in any way that touches your content.

## Running them

Open any of the HTML files in a browser. That is the whole setup.

To serve them instead:

```sh
python -m http.server 8000
```

`ledger.css` holds the shared styling and must sit next to the HTML files.

## Browser support

Anything current. `pdf-to-text.html` needs `ImageBitmap` and canvas, which is
every modern browser.

## Related

These grew out of [Snip2Field](https://snip2field.netlify.app), a paid Windows
desktop tool that snips a field off any invoice on screen and checksum-verifies
it before copying. **That application is closed source and is not part of this
repository** — only the free browser tools here are MIT licensed.

## Contributing

Issues and pull requests are welcome. The rule these follow is simple: a tool
here may not send the user's content anywhere. Anything that would need a server
does not belong in this repo.

## License

[MIT](LICENSE)
