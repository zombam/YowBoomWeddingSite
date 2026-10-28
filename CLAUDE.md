# Yow & Boom wedding e-card

A one-page wedding invitation website for โย๋ (นางสาวปุณณภา บุญประคอง) and บูม (นายณัฐพงศ์ วัชระวิทยากุล).
Saturday 13 March 2570 (2027), พิพิธภัณฑ์ตุ๊กตาเชียงใหม่ (Chiang Mai Doll Museum). Hosted on Netlify (drag-and-drop deploy of this folder).

The site was designed with the owner (a web/UX designer) in a long Claude chat. This file carries over every decision from that chat. Keep to it unless the owner asks otherwise.

## Files

- `index.html`: the whole site. All CSS and JS are inline; there is no build step.
- `img/`: every image the page uses, plus `song.mp3`. Some older files in it are no longer referenced. Nothing else is needed to deploy.
- `.claude/launch.json`: a local server (`python3 -m http.server 8737`) for previewing. Opening `index.html` as a file doesn't load the images in Claude's browser pane. Netlify ignores it.
- Fonts come from Google Fonts: **Charmonman** (decorative: titles, names, times, headers), **Trirong** (body), **Charm** (fallback).
  The printed card uses *TH Charmonman*, which isn't on Google Fonts. If the owner supplies the font file, self-host it with `@font-face` and put it first in `--f-script`.

## Deploying

Drag the whole folder onto Netlify Drop, or onto the existing Netlify project's Deploys page.
The "Powered by Netlify" badge can be switched off in Project configuration › General › Powered by Netlify badge.
Before each deploy, check that every `img/...` path referenced in `index.html` exists in `img/` (one zip once shipped without `wedof.png`).

## Page structure, top to bottom

1. **Entrance (`.gate`)**: a warm peach-to-paper linen background. A **dark green envelope** is drawn with SVG shapes, *not* CSS clip-path; clip-path made the front pocket flash as a rectangle over the letter mid-animation. It has an ivory wax seal holding `img/monogram.svg`.
   Tapping the seal opens the flap, slides the letter up, brings the letter forward while the envelope fades, then reveals the site (about 3.3 s).
   The greeting reads "เรียน แขกคนพิเศษ". A personal name comes from `#g01`-style links via the `GUESTS` map in the script, or from `?to=ชื่อ` when self-hosted.
   A "ข้ามไปที่การ์ด" (skip to card) link is top right.
2. **Hero**: `img/frame.svg` is the owner's complete frame artwork, with sunflowers, vines and swirls baked in. It is used as **one fixed-ratio image** (339.53 × 411.46); don't stretch it. Its container `.hf` uses `container-type:inline-size`, so every position is in `cqw` units.
   - The gold mandala (`img/mandala.svg`) sits behind the frame, clipped along the frame's top green lines (`.hf-crown` clip-path). It rises when the site opens, then spins slowly (120 s per turn).
   - Content inside the frame matches the printed card: "The wedding of" (`wedof.png`), the logo (`logo.png`), a divider (knot **above** the line), the date, the venue, then a divider flipped with `.flip-y` (knot **below** the line).
   - The couple is clipped to the inside of the frame's green lines. The point where they meet sits **just above the bottom knot**, as on the printed card.
     The page uses `couple-fit.png`, a copy of the owner's cut-out `couple-full.png` (kept unchanged) with the bottom of each jacket stretched down past the green V line. The original jackets ended above the line, leaving a gap under the groom that moving the image couldn't close. The owner wants **no gap at all** there.
     If the owner sends new couple artwork, the jackets need to reach below the line (≈ `118.2 - 0.61·|x-50|` cqw) on both sides. The space between the two figures above the knot stays open, as on the card.
   - Sparkles twinkle (`.spk`), paper lanterns drift up, and there's a "ตอบรับคำเชิญ" (RSVP) button below.
3. **Countdown**: minimal, just numbers with small gold dots between them and no boxes. It counts down to 07:09 on 13 March 2027 Chiang Mai time (+07:00), with an add-to-Google-Calendar link.
4. **Invitation**: the parents' names are grouped **by family** (`.fam`). On desktop the bride's parents are on the left, "และ" (and) in the middle and the groom's parents on the right. On phones they stack: นายยุทธนา บุญประคอง + นางสายพิณ ศรีวิชัย, และ, นายทักษิณ วัชระวิทยากุล + นางรัชนีกร วัชระวิทยากุล.
   Bride and groom names: **side by side on desktop**, in a row wider than the text column so each full name stays on one line. On phones (≤600px) they stack: bride, gold mark, groom. The bride's nickname is **โย๋** (not โย่).
5. **Photos (`.pics`)**: four polaroids "taped to the wall", between the couple's names and the schedule.
   - Each image is a whole drawn polaroid (frame, leaves and roots) on a **transparent background**. The owner wants it used as is, **not cropped**, with **no tape and no drop shadow** (both were tried and removed).
   - The owner's four drawings are `img/photo-1.webp` to `photo-4.webp`, trimmed to the drawing and resized to 800px. They have different shapes: 1 and 3 are landscape, 2 is portrait, 4 is near-square.
   - Phones: two loose columns (`.wcol`), photos 1+4 on the left and 2+3 on the right, because those pairs come out about the same height. Desktop (≥760px): `.wcol` becomes `display:contents` and the photos run 1–4 in one staggered row (`order`).
   - Each photo has its own tilt (`--r`), height offset (`--y`), sideways nudge (`--x`, using `left`, because the scroll-in animation owns `translate`) and width (`--w` on phones, `flex-grow` on desktop). They swing into their tilt one after another as they scroll into view, and straighten and lift on hover.
6. **กำหนดการ (schedule)**:
   - Header: `img/header-half.svg` mirrored left and right, joined by a gold-bordered box that stretches to fit the title (`.hd`). On phones the outer swirls run off the screen edges on purpose.
   - Timeline: 07:09 พิธีสงฆ์, 09:09 แห่ขันหมาก, 10:19 พิธีผูกแขน, 11:30 รวมรับประทานอาหาร.
   - Markers (`knot.svg`) face the event text, and even-numbered rows are flipped. The khaki vertical line is drawn in pieces (`.ev::before`/`::after`) so it joins each marker exactly where it does on the printed card. Don't put a background colour behind the markers.
7. **สถานที่จัดงาน (venue)**: the same header frame, then the transparent watercolour of the venue (`venue.webp`, faded at the top), extra space, the venue name, and the map QR code with a Google Maps link.
8. **Dress code**: a divider above (knot up) and below (knot down). Only "เดรสโค้ด / ชุดพื้นเมือง / #YOWBOOMม่วนใจ๋". No description text and no colour dots (the owner removed both).
9. **RSVP**: a card with a button only, no form; the owner wants the brother to control the questions in Google Forms.
   `#rsvp-link` points to the real Google Form (set on 19 Oct 2026). If it is ever set back to the `YOUR-FORM-LINK` placeholder, tapping it shows "ลิงก์ตอบรับจะเปิดให้ใช้เร็ว ๆ นี้" (the RSVP link is coming soon).
10. **Footer**: "ขออภัยหากมิได้มาเรียนเชิญด้วยตัวเอง" (sorry we couldn't invite you in person), and a link to open the letter again.
11. **Song player (`.song`)**: ปะแล๊ดปะแล่น (เบื๊อก ft. เดอะเพอะ), which the bride asked for, played from `img/song.mp3` with an `<audio>` element. It replaced the earlier Spotify embed.
    - `song.mp3` was made from the owner's MP3. It starts at **0:59.6**, where Spotify's preview starts (found by matching the preview audio). It's turned down about 14 dB (the owner asked for it quieter twice), fades in over 2 s, and loops.
      The quietness is baked into the file because iPhones ignore `audio.volume`. To make it quieter or louder, re-encode it from the original MP3 (not from `song.mp3`) with ffmpeg's `volume=` filter (now 0.2).
    - The song starts when the guest taps the seal or "ข้ามไปที่การ์ด" (`playSong()`). Browsers block sound before any tap, so true on-load autoplay isn't possible.
    - The card fades in 1.8 s after the site opens. It shows `monogram.svg` (the envelope's wax-seal logo; the owner chose it over `amp.png`, which isn't used), the title, and a green play/pause button (SVG icons, because ▶ turns into an emoji on iPhones).
    - It tucks into a small pill once the guest scrolls past 380px, and the music keeps playing. The pill keeps the green play/pause button, so guests can pause in one tap, plus ♪ to open the card again. The tuck is a smooth 0.6 s morph, which the owner asked for (it used to snap).
      The card is anchored by `right` in both states, so width, position and corners can animate. Its inner row keeps the open width, so the title and play button slide out of view behind the ♪ and fade. Avoid `display:none` or `width:auto` there; they can't animate.
    - **Not yet verified on a real iPhone or on the live Netlify site.**

## Side decoration

- Sunflowers (`sf1/sf2/sf3/bud.png`) peek in from the left and right page edges in each section (`.edge.sway`). The owner did **not** like vines and swirls floating on the page sides.
- Sparkles are scattered by a small seeded script (`.spk-free`), mostly toward the sides.
- Sections fade up as they scroll into view (IntersectionObserver adds `.rv` / `.in`, using the `translate` property so it doesn't fight the `transform` flips).
- `prefers-reduced-motion` turns animations off.

## Design rules the owner cares about

- Match the printed card: positions, which way ornaments face, and where vines start.
- Any vine or swirl must start **on** a frame line, never floating. That's now baked into `frame.svg`, but keep it in mind for new ornaments.
- Palette: Lanna green `#005546`, card green `#0d7a4c`, foil gold `#dbb05f`, khaki `#a49969`, ink `#1d3b44`, dawn `#f8e2ba` / `#fcf2de`, paper `#fffdf7`, sage `#eef2e6`.
- Check changes at phone (390px) and desktop widths. Desktop and mobile layouts can differ, and the owner has asked for that.

## Source artwork

The printed card PDF and the SVG ornaments are in the owner's folder: `…/My Work/bro wedding/SVG` (iCloud).
