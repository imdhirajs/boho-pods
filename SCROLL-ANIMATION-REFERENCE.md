# Scroll-Driven Hero Video — Reference Guide
## The technique that works. Use this every time.

---

## The Problem We Kept Hitting

Every attempt using canvas frame extraction + `window.scrollY` failed because:

- **Canvas frame extraction** takes 30–60 seconds to preload and has timing issues with Lenis
- **`window.scrollY`** with Lenis is unreliable — Lenis animates scroll position internally and `window.scrollY` lags behind
- **`window.addEventListener('scroll', ...)`** with Lenis doesn't fire reliably because Lenis intercepts the native wheel event

---

## The Solution That Works

**Direct `video.currentTime` scrubbing using `getBoundingClientRect()`**

This works because:
- `getBoundingClientRect()` reads the actual DOM layout position — always correct regardless of Lenis
- We hook into **both** `window.addEventListener('scroll')` AND `lenis.on('scroll', ...)` so either path triggers the update
- We use `requestAnimationFrame` with a `ticking` flag so we never spam updates
- We use an `initialized` flag so we never run before the video is ready

---

## Step 1 — HTML Structure

Wrap the existing hero in a scroll track. The hero itself stays sticky.

```html
<!-- Outer wrapper: tall enough to drive the full video duration -->
<div id="hero-wrap" class="hero-scroll-track">

  <!-- Inner sticky section: stays pinned while page scrolls through the wrapper -->
  <section class="hero" id="home">

    <!-- Video: visible, full cover, no autoplay -->
    <video id="source-video"
           src="./your-video.mp4"
           muted
           playsinline
           preload="auto">
    </video>

    <!-- All your text overlays, buttons, etc. stay here — untouched -->
    <div class="hero-scrim"></div>
    <div id="hero-content"></div>

  </section>
</div>
```

---

## Step 2 — CSS (minimal, add only what's missing)

```css
/* Scroll track: tall enough for the full video scroll range */
.hero-scroll-track {
  position: relative;
  height: 500vh; /* adjust to taste — 400vh to 600vh */
}

/* Sticky hero: pins to top while scroll track scrolls past */
.hero {
  position: sticky;
  top: 0;
  height: 100vh;
  overflow: hidden;
}

/* Video: full cover, no autoplay, GPU accelerated */
#source-video {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  will-change: transform;
  transform: translateZ(0);
}
```

---

## Step 3 — JavaScript (copy this exactly)

```js
(() => {
  // 1. Find the scroll track and video
  const track = document.querySelector('.hero-scroll-track');
  if (!track) { console.warn('[hero] .hero-scroll-track not found'); return; }

  const video = track.querySelector('video');
  if (!video) { console.warn('[hero] video not found'); return; }

  // 2. Skip if user prefers reduced motion
  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) return;

  let ticking     = false;
  let duration    = 0;
  let initialized = false;

  const clamp = (v, lo, hi) => Math.min(Math.max(v, lo), hi);

  // 3. Core update function — runs inside rAF
  function update() {
    if (!initialized || !duration || !Number.isFinite(duration)) return;

    const total    = track.offsetHeight - window.innerHeight;
    const rect     = track.getBoundingClientRect();
    const passed   = clamp(-rect.top, 0, total);
    const progress = total > 0 ? passed / total : 0;
    const targetTime = clamp(progress * duration, 0, duration);

    if (video.readyState >= 2) {
      video.currentTime = targetTime;
    }

    // Update any text phases based on progress
    // updatePhases(progress); ← wire your phase logic here
  }

  // 4. rAF-throttled tick — prevents scroll event spam
  function requestTick() {
    if (ticking) return;
    ticking = true;
    requestAnimationFrame(() => { update(); ticking = false; });
  }

  // 5. Init — called once video is seekable
  function initScrub() {
    duration = video.duration;
    if (!duration || !Number.isFinite(duration)) {
      console.warn('[hero] invalid duration:', duration);
      return;
    }
    video.pause();           // explicitly stop any autoplay
    video.currentTime = 0;  // start from first frame
    initialized = true;

    // Hide loading screen if you have one
    const ls = document.getElementById('loading-screen');
    if (ls) { ls.classList.add('fade-out'); setTimeout(() => ls.style.display = 'none', 850); }

    // Register all scroll listeners
    window.addEventListener('scroll', requestTick, { passive: true });
    window.addEventListener('resize', requestTick);
    window.addEventListener('load',   requestTick);

    // CRITICAL for Lenis: hook Lenis's own scroll event
    // lenis.on('scroll', ...) fires every frame during Lenis animation
    // window.addEventListener('scroll') alone is not enough with Lenis
    if (typeof lenis !== 'undefined') {
      lenis.on('scroll', requestTick);
    }

    requestTick();
  }

  // 6. iOS/Safari unlock — mobile browsers block seeking until user interaction
  window.addEventListener('touchstart', () => {
    video.play().then(() => { video.pause(); video.currentTime = 0; }).catch(() => {});
  }, { once: true, passive: true });

  // 7. Start immediately if already ready, otherwise wait for load events
  if (video.readyState >= 2) {
    initScrub();
  } else {
    video.addEventListener('loadedmetadata', initScrub, { once: true });
    video.addEventListener('loadeddata',     initScrub, { once: true });
    video.addEventListener('canplaythrough', initScrub, { once: true });
    video.load();
  }
})();
```

---

## Step 4 — Re-encode the MP4 (REQUIRED for smooth scrubbing)

Normal MP4 exports from Premiere / DaVinci / After Effects have sparse keyframes (every 2–10 seconds). This makes `video.currentTime` seeking stutter or freeze between keyframes.

**Run this FFmpeg command before deploying:**

```bash
ffmpeg -i your-video.mp4 \
  -vf scale=960:-1 \
  -movflags faststart \
  -vcodec libx264 \
  -crf 20 \
  -g 1 \
  -pix_fmt yuv420p \
  your-video-scrub.mp4
```

| Flag | What it does |
|------|-------------|
| `-g 1` | Every frame is a keyframe — enables frame-accurate seeking |
| `-movflags faststart` | Moves metadata to the front — video starts faster |
| `-vf scale=960:-1` | Reduces resolution to keep file size manageable |
| `-crf 20` | Quality setting (18=best, 28=smallest) |

If `-g 1` makes the file too large, try `-g 5` or `-g 10`. Even `-g 30` is much better than the default.

---

## Why Lenis Breaks the Simple Approach

Lenis smooth scroll (v1.x) intercepts native wheel events and animates scroll position via its own RAF loop. This breaks two common approaches:

| Approach | Why it fails with Lenis |
|----------|------------------------|
| `window.scrollY` | Updates asynchronously — lags 1–2 frames behind Lenis |
| `window.addEventListener('scroll')` alone | Fires but `window.scrollY` value is stale |
| `lenis.scroll` property | Works but only if read at the right RAF timing |
| **`getBoundingClientRect()` + `lenis.on('scroll')`** | ✅ **Always correct — reads actual DOM position** |

The fix: hook **both** `window.addEventListener('scroll')` and `lenis.on('scroll', requestTick)`. Then inside `update()`, use `track.getBoundingClientRect().top` which always reflects the current rendered position.

---

## Debugging Checklist

Add these `console.log` calls if it stops working:

```js
// In update():
console.log({
  readyState: video.readyState,
  duration,
  progress,
  targetTime,
  currentTime: video.currentTime
});
```

| What you see | What it means |
|-------------|---------------|
| `duration: NaN` | `loadedmetadata` never fired — video file not loading |
| `progress` not changing | Scroll events not firing — check Lenis hookup |
| `readyState: 1` | Video loaded metadata but not data — wait for `loadeddata` |
| `readyState: 4`, progress changes, but video frozen | **MP4 needs re-encoding** with `-g 1` |
| Works on Chrome, broken on Safari | iOS touch unlock missing — add the `touchstart` pattern |

---

## Project-Specific Notes (Boho Pods)

- Hero wrapper: `#hero-wrap` with class `hero-scroll-track`, height `550vh`
- Hero section: `.hero` — sticky, `100vh`
- Video: `#source-video` — `src="./bohopods-hero-journey.mp4"`
- Lenis version: `1.3.23` with GSAP ticker integration
- Phase system: 5 phases driven by `BREAKS = [0, 0.18, 0.36, 0.54, 0.72]`
- Loading screen: `#loading-screen` — fades out in `initScrub()` after `loadedmetadata`
