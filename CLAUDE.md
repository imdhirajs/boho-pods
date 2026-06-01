# BOHO PODS — Claude Code Project Context
# Read this file before doing ANYTHING in this project.

---

## WHO IS THE CLIENT

**Client:** Vikram Dangi
**Brand:** Boho Pods
**Product:** Premium prefab glamping pods — luxury portable structures
placed on hilltops, beaches, and forests across India.
**Target Audience:** Men aged 40–60, business owners, resort developers,
land owners. Conservative, trust-driven, financially capable.
**Agency building this:** SlaqAI (Dhiraj)

---

## CRITICAL RULES — NEVER BREAK THESE

1. **BRIGHT WARM COLORS ONLY** — No dark backgrounds except
   the Buying Options section and Footer. Client explicitly
   rejected dark color schemes.

2. **VERTICAL SCROLL ONLY** — Zero horizontal scrolling anywhere
   on the page. Not in gallery. Not in add-ons. Nowhere.

3. **HERO = SCROLL-CONTROLLED VIDEO** — The video
   bohopods-hero-journey.mp4 plays frame by frame as user
   scrolls down. Scroll down = video plays forward.
   Scroll up = video rewinds. This is non-negotiable.

4. **SINGLE FILE** — Everything in one index.html.
   No build tools, no npm, no server required.
   Must open directly in browser via file:// protocol.

5. **READABLE FOR 40-60 YEAR OLDS** — Body text minimum 16px.
   Line height minimum 1.8. High contrast. Clear CTAs.
   No trendy gimmicks that confuse older users.

---

## TECH STACK

- Vanilla HTML + CSS + JS — single index.html
- GSAP 3.12.5 + ScrollTrigger (CDN)
- Lenis 1.3.23 smooth scroll (CDN) synced with GSAP ticker
- Google Fonts: Cormorant Garant + Plus Jakarta Sans
- No frameworks. No build step. No npm.

### CDN Imports (exact order, always use these):
```html
<link rel="stylesheet" href="https://unpkg.com/lenis@1.3.23/dist/lenis.css">
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js"></script>
<script src="https://unpkg.com/lenis@1.3.23/dist/lenis.min.js"></script>
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garant:ital,wght@0,300;0,400;0,600;0,700;1,300;1,400;1,600&family=Plus+Jakarta+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
```

### Lenis Setup (always use this exact code):
```js
const lenis = new Lenis({ lerp: 0.075, smoothWheel: true });
lenis.on('scroll', ScrollTrigger.update);
gsap.ticker.add((time) => { lenis.raf(time * 1000); });
gsap.ticker.lagSmoothing(0);
```

---

## DESIGN SYSTEM

### Color Palette (CSS Variables — always use var()):
```css
:root {
  --white:        #FFFFFF;
  --bg:           #F8F5EF;   /* warm off-white — main background */
  --bg-alt:       #EFF4EA;   /* soft sage green — alternate sections */
  --bg-dark:      #1C2B1A;   /* deep forest green — navbar, buying options, footer */
  --text:         #1A2410;   /* near-black dark green — all headings + body */
  --text-mid:     #4A5E40;   /* mid green — subtext, descriptions */
  --text-muted:   #8A9E80;   /* muted — captions, labels, placeholders */
  --green:        #3D6B35;   /* primary forest green — CTAs, accents, borders */
  --green-light:  #EFF4EA;   /* light green tint — card hover, pill backgrounds */
  --amber:        #C17F3E;   /* warm amber — secondary accent, tags, highlights */
  --amber-light:  #FDF3E7;   /* amber tint */
  --border:       rgba(61,107,53,0.15);
  --shadow:       rgba(26,36,16,0.08);
  --serif:        'Cormorant Garant', serif;
  --sans:         'Plus Jakarta Sans', sans-serif;
}
```

### Typography Rules:
- **Display headings:** Cormorant Garant, 52–96px, weight 300–600
- **Section headings:** Cormorant Garant, 52–72px
- **Body text:** Plus Jakarta Sans, 16–18px, line-height 1.8–2.0
- **Labels/tags:** Plus Jakarta Sans, 10–12px, letter-spacing 3–4px, UPPERCASE
- **Minimum font size:** 14px — never go below this
- **Heading line-height:** 0.95–1.1 (tight)
- **Body line-height:** 1.8–2.0 (loose)
- **Never use:** Inter, Roboto, Arial, Space Grotesk, or system fonts

### Luxury Design Rules:
- Gold/amber used on maximum 3 elements per section
- All borders: rgba with 0.12–0.20 opacity max
- Section padding: 100px–120px vertical
- No drop shadows except subtle: 0 4px 24px var(--shadow)
- Sharp card corners OR very subtle border-radius (4–8px max)
- Left-aligned headings preferred over centered (except hero + testimonials)
- White backgrounds = trust. Use them for card surfaces.

---

## FILES IN THIS PROJECT

All files are in the ROOT FOLDER alongside index.html.
Reference them exactly as named below:

### Video:
- `bohopods-hero-journey.mp4` — 15s cinematic journey video:
  aerial bird's eye → descends to pod → enters bedroom →
  interior panoramic → exits + orbits pod → rises back to aerial.
  This is the hero scroll video. 38.4MB.

### Images:
- `bohopods-3d-isometric-floorplan-pod-layout.png` (987KB)
- `bohopods-aerial-exterior-beach-tropical-firepit-lily-pond.png` (3.3MB)
- `bohopods-aerial-exterior-hilltop-terraced-garden-misty-valley.png` (2.8MB)
- `bohopods-deck-beachfront-lounge-palm-sunset.png` (3.4MB)
- `bohopods-exterior-forest-stone-firepit-misty-mountains.png` (2.6MB)
- `bohopods-exterior-mountain-sunset-terrace-outdoor-seating.png` (3.8MB)
- `bohopods-exterior-zen-rock-garden-mountain-backdrop.png` (2.7MB)
- `bohopods-interior-bedroom-curved-glass-misty-mountain-valley.png` (1.9MB)
- `bohopods-interior-bedroom-mountain-valley-view-reading-chair.png` (3.6MB)
- `bohopods-interior-bedroom-zebra-headboard-wooden-desk.png` (3MB)
- `bohopods-interior-bedroom-zen-garden-pendant-lights-white-bed.png` (2.7MB)

### Add-on images (use existing images mapped below):
- Swimming Pool → `bohopods-aerial-exterior-beach-tropical-firepit-lily-pond.png`
- Plunge Pool → `bohopods-deck-beachfront-lounge-palm-sunset.png`
- Jacuzzi → `bohopods-deck-beachfront-lounge-palm-sunset.png`
- Pergola → `bohopods-exterior-zen-rock-garden-mountain-backdrop.png`
- Sit-Out → `bohopods-exterior-mountain-sunset-terrace-outdoor-seating.png`
- Fire Pit → `bohopods-exterior-forest-stone-firepit-misty-mountains.png`
- Dark Paneling → `bohopods-interior-bedroom-mountain-valley-view-reading-chair.png`
- Light Paneling → `bohopods-interior-bedroom-zen-garden-pendant-lights-white-bed.png`

---

## PAGE STRUCTURE (exact section order)

1. Sticky Navbar
2. Hero — Scroll-controlled video (550vh wrapper)
3. Brand Statement — Quote + stats
4. Pod Gallery — 2-column vertical grid (6 pods)
5. Architecture — Isometric 3D floating image
6. Buying Options — 3 cards (DARK section)
7. Process — Animated vertical timeline
8. Add-Ons — Hover reveals image on right
9. Testimonials — 3 cards
10. Full-bleed Parallax Statement
11. Founder Story
12. Contact Form (DARK section)
13. Footer (DARK)

---

## SECTION SPECIFICATIONS

### NAVBAR
- Background: var(--bg-dark), height 68px, fixed top 0, z-index 1000
- On scroll >80px: backdrop-filter blur(20px)
- Left: "BOHO PODS" — Cormorant 20px, letter-spacing 5px, white
- Center: Home | Pods | Add-Ons | Process | Buy Now | Contact
  Plus Jakarta Sans 13px, letter-spacing 1px, rgba(255,255,255,0.75)
- Right: "CONTACT US" ghost button + WhatsApp green button
- Mobile: hide center links, keep both right buttons

### HERO (most important section)
- Wrapper height: 550vh
- Inner: position sticky, top 0, height 100vh
- Video: absolute, inset 0, object-fit cover, PAUSED on load
- Overlay gradient: dark green top → transparent middle →
  fades to var(--bg) at very bottom (seamless into next section)
- Scroll JS: heroVideo.currentTime = progress × heroVideo.duration
  Uses ScrollTrigger scrub: 0.5

5 text phases (bottom 18%, left 8%):
- Phase 1 (0–18%): "Where Nature / Becomes Home." + subtext
- Phase 2 (18–36%): "Your Land. / Your Legacy."
- Phase 3 (36–54%): "Wake Up / To This."
- Phase 4 (54–72%): "Precision. / In Every Panel."
- Phase 5 (72–90%): "The Landscape / Is Yours." + amber CTA button

Phase transitions: outgoing y→-15 opacity→0 (0.35s),
incoming y:20→0 opacity:0→1 (0.55s)

Scroll cue: "SCROLL TO EXPLORE" + bouncing chevron, fades at 8%

### BRAND STATEMENT
- Background: var(--white)
- Left: large italic quote + attribution + amber line
- Right: image with clip-path reveal
- Bottom: 3 stat counters (12 Models, 30 Days, 100% Prefab)
- Stats countUp from 0 on scroll into view

### POD GALLERY
- Background: var(--bg-alt)
- 2-column CSS grid, gap 32px — VERTICAL SCROLL
- 6 cards, each 520px tall
- Card: white background, 8px radius, subtle shadow
- Image top 65%, content bottom 35%
- Hover: image reverse-zoom (scale 1.05→1.0)
- Cards stagger in: y:60 opacity:0 → y:0 opacity:1

### ARCHITECTURE (Isometric)
- Background: var(--white), two columns
- Left: heading + body + two green stat pills
- Right: isometric image with mix-blend-mode: multiply
  (removes white background naturally)
  Enters with rotateX(20deg) rotateY(-15deg) scale(0.8)→
  rotateX(8deg) rotateY(-5deg) scale(1)
  Then continuous float: y oscillates ±12px, 3.5s sine loop

### BUYING OPTIONS (only intentionally dark section)
- Background: var(--bg-dark)
- 3 cards: ghost dark style with amber hover border
- Card 2 (EMI): featured — 3px solid amber top border
- Each card: tag chip + icon + title + description + features + CTA
- Cards stagger up on scroll

### PROCESS (Animated Timeline)
- Background: var(--bg)
- Vertical line on left draws itself with ScrollTrigger scrub
- 5 steps: dot fills green as line reaches each step
- Text reveals staggered per step
- Steps: Consultation → Site Survey → Site Preparation →
  Pod Construction → Handover

### ADD-ONS (Hover Reveal)
- Background: var(--white), two columns
- Left (45%): 8 add-on names stacked, hover → name turns green,
  arrow appears, background turns var(--green-light)
- Right (55%): single image that crossfades on each hover
- Default image: exterior mountain sunset
- JS: mouseenter → gsap fade out → swap src → fade in

### TESTIMONIALS
- Background: var(--bg-alt)
- 3 white cards, subtle shadow
- ★★★★★ stars in amber
- Placeholder content (client to replace)

### FULL-BLEED PARALLAX
- Background image: aerial hilltop terraced garden
- Overlay: dark green semi-transparent
- Centered Cormorant italic 88px white text
- backgroundPosition scrolls at 0.4x speed
- Amber CTA button

### FOUNDER STORY
- Background: var(--white), two columns
- Left: placeholder rectangle var(--bg-alt) for founder photo
- Right: italic Cormorant heading + body + green signature

### CONTACT FORM (dark section)
- Background: var(--bg-dark)
- Fields: First Name, Mobile Number, Message (textarea)
- WhatsApp consent checkbox (accent-color: var(--amber))
- Submit: "REQUEST A CONSULTATION" — amber background
- WhatsApp link below form
- On submit: button → "✓ We'll be in touch within 24 hours."
- NO backend, NO email field (client specified)

### FOOTER
- Background: #111D10 (darker than bg-dark)
- Amber top border line
- 4 columns: brand | links | legal | contact
- Copyright: "© 2025 Boho Pods. Built by SlaqAI."

---

## GLOBAL ANIMATIONS (apply to entire page)

### Word-by-word heading reveal (all h2, h3):
```js
function revealWords(el) {
  const words = el.innerHTML.split(' ');
  el.innerHTML = words.map(w =>
    `<span style="overflow:hidden;display:inline-block;vertical-align:bottom">
      <span class="word" style="display:inline-block;transform:translateY(105%)">${w}</span>
    </span>`
  ).join(' ');
  ScrollTrigger.create({
    trigger: el, start: "top 88%",
    onEnter: () => gsap.to(el.querySelectorAll('.word'), {
      y: 0, duration: 0.85, stagger: 0.055, ease: "power4.out"
    })
  });
}
document.querySelectorAll('h2, h3').forEach(revealWords);
```

### Image clip-path reveal (all section images):
```js
// Wrap each img in overflow:hidden div, then:
gsap.fromTo(img,
  { clipPath: "inset(100% 0 0 0)", scale: 1.08 },
  { clipPath: "inset(0% 0 0 0)", scale: 1,
    duration: 1.1, ease: "power3.inOut",
    scrollTrigger: { trigger: img, start: "top 85%" }
  }
);
```

### Body text fade-up:
All p, li, label elements:
y:30 opacity:0 → y:0 opacity:1, duration 0.8s ease power2.out
ScrollTrigger start: "top 88%"

---

## CUSTOM CURSOR
- #cursor: 10px solid circle, var(--green), fixed, z-index 9999
- #cursor-ring: 36px hollow circle, 1.5px border var(--green)
- Ring follows with lerp 0.1
- On button/link hover: cursor shrinks, ring expands + fills green tint

---

## PAGE LOAD
- Body starts opacity: 0
- After 100ms: fade in over 0.8s
- Navbar: slides down from y:-68 over 0.6s
- Hero Phase 1 text: y:20→0 opacity:0→1 after 0.4s delay

---

## WHAT TO DO IF SOMETHING IS UNCLEAR
- Always default to BRIGHT and WARM (not dark)
- Always default to VERTICAL scroll (not horizontal)
- Always use the exact filenames listed above
- When in doubt about color: use var(--bg) background, var(--text) text
- Body text always Plus Jakarta Sans, headings always Cormorant Garant
- If an image needs a white background removed: mix-blend-mode: multiply

---

## HERO SCROLL VIDEO — CRITICAL IMPLEMENTATION NOTES

**See SCROLL-ANIMATION-REFERENCE.md for the full guide.**

The hero video scroll effect uses **direct `video.currentTime` scrubbing** — NOT canvas frame extraction.
Canvas frame extraction was tried and failed repeatedly. Do not suggest it again.

### Required HTML structure:
- Outer wrapper `#hero-wrap` must have class `hero-scroll-track` and height `550vh`
- Inner `.hero` section must be `position:sticky; top:0; height:100vh; overflow:hidden`
- `#source-video` must be `position:absolute; inset:0; object-fit:cover; display:block`
- `#video-canvas` must be `display:none` (not used)

### Required JS pattern:
```js
// getBoundingClientRect() for progress — not window.scrollY (lags with Lenis)
const rect     = track.getBoundingClientRect();
const total    = track.offsetHeight - window.innerHeight;
const passed   = Math.min(Math.max(-rect.top, 0), total);
const progress = total > 0 ? passed / total : 0;
video.currentTime = progress * duration;

// Hook BOTH native scroll and Lenis scroll:
window.addEventListener('scroll', requestTick, { passive: true });
lenis.on('scroll', requestTick); // REQUIRED — window scroll alone is not enough with Lenis
```

### Required: video must be re-encoded with dense keyframes
Normal MP4 exports stutter on `video.currentTime` seeking. Always encode with:
```bash
ffmpeg -i input.mp4 -vf scale=960:-1 -movflags faststart -vcodec libx264 -crf 20 -g 1 -pix_fmt yuv420p output-scrub.mp4
```

### Why window.scrollY fails with Lenis:
Lenis v1.x animates scroll via its own RAF loop and updates `window.scrollY` asynchronously.
By the time our scroll handler reads `window.scrollY`, it's stale. `getBoundingClientRect()` always
reads the actual current DOM position and is always correct.
