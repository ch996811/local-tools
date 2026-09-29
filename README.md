# Local Tools

Six small web tools that do their work on your own machine. **Your file, your
screenshot and your bank details are never uploaded to anything.**

Each one is a single HTML file with no build step, no framework, no bundler and
no backend. Open it from disk and it works.

| Tool | What it does | Live |
|---|---|---|
| [`iban-checker.html`](iban-checker.html) | Validates an IBAN against the official ISO 13616 mod-97 checksum, shows the country, bank code and account number, and explains exactly which check a bad IBAN failed. Knows the length and field layout of 28 countries. | [try it](https://snip2field.netlify.app/iban-checker/) |
| [`pdf-to-text.html`](pdf-to-text.html) | Extracts the text from a PDF. Pages that carry a real text layer are read out exactly, with no recognition step; only image-only scanned pages go through OCR, and the scan is lifted from the file at its original resolution rather than re-rendered. | [try it](https://snip2field.netlify.app/pdf-ocr/) |
| [`snip2text.html`](snip2text.html) | Paste or drop a screenshot and get the text out of it. | [try it](https://snip2field.netlify.app/snip2text/) |
| [`statement-to-csv.html`](statement-to-csv.html) | Turns a bank statement PDF into CSV rows, then checks every row against the statement's own running balance: a row's amount must equal the change in balance from the row above, so a misread is flagged red instead of exported. Works out the date order (MM/DD vs DD/MM) and decimal convention (1,234.56 vs 1.234,56) from the document. Cells are editable and re-checked as you type. `sample-statement.pdf` has one row altered on purpose to show a flag. | [try it](https://snip2field.netlify.app/statement-to-csv/) |
| [`screenshot-studio.html`](screenshot-studio.html) | Background, padding, rounded corners, shadow and aspect ratio for a screenshot, plus arrows, boxes, pen, text and a pixelate tool that is baked into the exported PNG. No libraries at all. | [try it](https://snip2field.netlify.app/screenshot-studio/) |
| [`salary-clock.html`](salary-clock.html) | Shows what you are earning second by second, and what a break or a meeting cost. Settings stay in `localStorage`. | [try it](https://snip2field.netlify.app/salary-clock/) |

## How the privacy claim actually holds

There is no server to send anything to. All six tools are static pages:

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
   from jsDelivr, and `pdf-to-text.html` and `statement-to-csv.html` load
   [pdf.js](https://github.com/mozilla/pdf.js). `iban-checker.html`,
   `screenshot-studio.html` and `salary-clock.html` load nothing at all.
   That is code coming *to* your browser. Your document does not go the other
   way. Vendor these two files locally if you want zero third-party requests.
2. **The files in this repository contain no analytics at all.** The hosted
   copies at snip2field.netlify.app add one Cloudflare Web Analytics script line
   (anonymous, cookieless page views) at deploy time — that is a property of the
   hosting, not of these tools, and it never touches your content.

## Running them

Open any of the HTML files in a browser. That is the whole setup.

To serve them instead:

```sh
python -m http.server 8000
```

`ledger.css` holds the shared styling and must sit next to the HTML files, and
`sample-statement.pdf` next to `statement-to-csv.html` for its sample button.
Links in the page chrome point at the hosted site.

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
