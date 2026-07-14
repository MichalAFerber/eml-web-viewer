# EML Viewer

A fast, mobile-first, **single-file** email viewer. Drag & drop an `.eml` and
read the message — **headers, body, and attachments** — with an **SPF / DKIM /
DMARC** authentication readout up top. It's a **viewer, not a client** — no
accounts, no sending, no uploads. Everything runs locally in your browser.

🔗 **Live:** <https://eml-viewer.us/>

![Single file](https://img.shields.io/badge/build-single%20HTML%20file-success) ![No build step](https://img.shields.io/badge/build%20step-none-success) ![License](https://img.shields.io/badge/license-MIT-blue)

> Part of the **[File Viewer](https://file-viewer.us/) family** — HTML, Markdown,
> ePUB, PDF, Data, DOCX, Sheets, and EML each have their own dedicated viewer.
> Use the **☰ menu** in the header to jump between them.

## Features

- 🔐 **SPF / DKIM / DMARC readout** — parsed from the message's
  `Authentication-Results`, `Received-SPF`, and `DKIM-Signature` headers and
  shown as clear **pass / fail / none** badges with the signing domain and
  selector. (Reads what the receiving server recorded — no live DNS lookups.)
- 📇 **Header card** — From, To, Cc, Subject, Date, Reply-To, cleanly formatted
  (RFC 2047 encoded-words decoded, including emoji).
- 📄 **Body** — HTML parts render in a **sandboxed iframe** with **remote images
  blocked for privacy** (no tracking pixels phone home); plain-text parts render
  with clickable links.
- 📎 **Attachments** — listed with type and size, each downloadable (decoded
  locally to a `data:` URL — nothing leaves your device).
- `</>` **Source view** — one tap shows the raw `.eml`.
- ☰ **Family menu** · 🫥 **auto-hiding header** · 🎨 **pick any background color**.
- 🪶 **One file, no build** — `index.html` is self-contained and works offline,
  even from `file://`.
- 📊 **Privacy-friendly analytics** — self-hosted, cookieless
  [Plausible](https://plausible.io/); your messages never leave your device.

## Supported file types

`.eml` `.mbox` `.emlx`

Outlook **`.msg`** (compound-binary) isn't parsed yet — the viewer detects it and
asks you to **Save As → .eml** first. (Planned.)

## Quick start

**Just open it.** Download [`index.html`](index.html), double-click it — no
server, no build, no internet needed.

```sh
python3 -m http.server 8080   # then open http://localhost:8080
```

## Deploy to Cloudflare Pages

Connect the repo (**Workers & Pages → Create → Pages → Connect to Git**),
framework preset **None**, build command blank, output directory `/`. The
[`_headers`](_headers) file applies a strict CSP automatically. Add the custom
domain **eml-viewer.us** under the project's Custom domains tab.

## How it works

Everything is in [`index.html`](index.html): a small, dependency-free MIME parser
(headers, multipart, base64 / quoted-printable, charset via `TextDecoder`,
RFC 2047 encoded-words) plus the auth-header analysis, both written for this
project. [highlight.js](https://github.com/highlightjs/highlight.js) is inlined
only to colorize the raw-source view.

HTML message bodies render inside a `sandbox`ed `<iframe>` whose own
`Content-Security-Policy` allows **only** inline styles and `data:` images — so
scripts never run and **remote images/trackers are blocked**. Attachments are
decoded in memory and offered as `data:` downloads. The viewer's own CSP stays
strict (`default-src 'none'`, no external assets).

**Note:** SPF/DKIM/DMARC results are **read from the headers the receiving mail
server added** — this is a viewer, not a validator, and performs no live DNS or
cryptographic verification.

## Credits

| Component | Version | License |
| --- | --- | --- |
| [highlight.js](https://github.com/highlightjs/highlight.js) | 11.9.0 | BSD-3-Clause |

The MIME parser and auth analysis are original to this project. Envelope icon
by the author. Analytics by [Plausible](https://plausible.io/).

## License

[MIT](LICENSE) © 2026 Michal Ferber, aka **TechGuyWithABeard**. Bundled
components retain their own licenses — see [`LICENSE`](LICENSE) and
[Credits](#credits).
