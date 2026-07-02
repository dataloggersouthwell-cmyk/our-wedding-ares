# Wedding AR Invitation — Setup Guide

This is a free, no-app-install AR experience. Guests open a link on their
phone, tap "Open Camera," point it at your printed invitation, and see
animated, tappable content float above it.

**Stack (all free):**
- [A-Frame](https://aframe.io/) — WebXR/3D scene framework
- [MindAR.js](https://hiukim.github.io/mind-ar-js-doc/) — image tracking (recognizes your actual invite artwork)
- [GitHub Pages](https://pages.github.com/) — free HTTPS hosting (camera access requires HTTPS)

---

## Test the camera first, before using your own invitation

`assets/targets/invitation.mind` currently contains MindAR's own example
target (a working test file), and `assets/targets/test-card.png` is the
matching image. **Before wiring up your real invitation:**

1. Push this as-is to GitHub Pages.
2. Open the site on your phone, tap "Open Camera."
3. Point your phone at `assets/targets/test-card.png` (open it on a second
   screen, or print it) — you should see the three hotspots appear.

If this works, your hosting/camera setup is fine and any future issue is
about your own `.mind` file. If it doesn't work, you'll now get a specific
on-screen error message (camera blocked, not HTTPS, etc.) instead of a
silent failure — screenshot that message if you need more help.

## Step 1 — Turn your invitation into a trackable target

MindAR needs a `.mind` file trained on your actual invitation design.

1. Go to the free compiler: https://hiukim.github.io/mind-ar-js-doc/tools/compile
2. Upload a flat, well-lit photo or the print-ready file of your invitation
   (front side — whatever design guests will point their camera at).
3. Click "Start," then "Download Compiled File." You'll get `targets.mind`.
4. Rename it `invitation.mind` and drop it into `assets/targets/`,
   replacing the placeholder path already referenced in `index.html`.

**Tips for a good target:**
- High contrast, lots of visual detail/texture (e.g. illustrations, patterns,
  varied typography) tracks far better than a plain card with just centered
  text on a solid background.
- Avoid glossy/laminated print if possible — glare confuses tracking.
- Test the compiler's built-in preview before downloading.

## Step 2 — Customize the content

Open `index.html` and edit:
- `#landing` section — your names and intro copy.
- The three `<a-entity class="tappable">` blocks — each has a
  `data-title` and `data-body` attribute. Change these to your actual
  story, ceremony details, and RSVP info. Add more hotspots by copying
  the pattern (each needs a unique `position`, and its own
  `data-title`/`data-body`).
- `position` values (`x y z`) place each hotspot relative to the center
  of your invitation image — nudge these once you test on your real
  invite, since layout differs per design.

## Step 3 — Host it for free

Easiest option: GitHub Pages.

1. Create a new GitHub repo, e.g. `our-wedding-ar`.
2. Upload this whole folder (`index.html`, `assets/`, `README.md`) to it.
3. In the repo, go to **Settings → Pages**, set source to the `main`
   branch, root folder.
4. GitHub gives you a URL like `https://yourname.github.io/our-wedding-ar/`
   — that's HTTPS by default, which is required for camera access.
5. Put that URL (or a shortened/QR version of it) on your printed
   invitation or wedding website.

## Step 4 (optional) — QR code shortcut

Generate a free QR code (e.g. via https://www.qr-code-generator.com) that
points to your GitHub Pages URL, and print it small in a corner of the
invitation, so guests who don't want to type a URL can just scan and go.

## Notes on "interactive, chained" content

Right now each hotspot is independent — tap one, see info, close, tap
another. If you want a *sequence* (tap A, it moves and reveals B, tap B to
reveal C), that's also doable: it just means hiding hotspot B until A has
been tapped, using a small state flag in the script (I can build that
version out once you confirm the actual content/order you want).

## What Python could add (optional, separate from the AR itself)

The AR/camera piece has to be JS — there's no way around that, it runs in
the browser. But if you want a live guest list or RSVP form that saves
responses somewhere, a small free Python backend (e.g. Flask on
[Render's free tier](https://render.com) or a Google Sheet via a Google
Apps Script) can sit behind an RSVP button on this same page. Happy to
build that next if useful.
