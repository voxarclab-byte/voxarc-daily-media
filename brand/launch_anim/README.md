# Voxarc Feed launch animation options (prepared Nov 14, 2026)

App code has not been changed. Nothing here was published.

| Option | File | Length | Notes |
|---|---|---|---|
| 0, Alan's (adapted) | option_0_alan_adapted_1080x1920_30fps.mp4 | 2.5 s | Frames 0-17 of Alan's clip are untouched: the 3D VA mark over the globe. The stepped wave section and the ~1 s crossfade that ghosted the mark are gone. They're replaced by a 0.5 s scale-and-slide of his 3D mark onto the exact position of his final mark, then a short aligned dissolve into his own final lockup, held to 2.5 s. |
| 0, Alan's original | option_0_alan_original_Voxarc_Intro_Global_Network_9x16.mp4 | 2.7 s | 1080x1920, 30 fps, H.264 High, no audio. Frames 12-40 are 5-6 frame holds (a ~5 fps slideshow), and frames 41-55 crossfade with a dip. |
| A, Signal lock | option_A_signal_lock_1080x1920_60fps.mp4 | 3.2 s | Orange data points converge into a network that snaps into the VA mark, with a flash and spring. The mark then scales and slides left while a light sweep reveals "voxarc" and "lab" fades in. |
| B, Globe ignite | option_B_globe_ignite_1080x1920_60fps.mp4 | 3.3 s | A dark wireframe globe tilted to the North Atlantic and Arctic (Toronto, Nuuk, Iqaluit, Oslo, London...). Network arcs ignite, the camera dives in, an orange flare births the VA mark, then the same slide into the lockup. |

All options end on the same still: the VA mark, then lowercase "voxarc" with "lab" beneath, on black.

- GIF previews: option_*_preview.gif (240 px wide, 15 fps)
- Side-by-side comparison: comparison_alan_A_B.mp4 (1080x720)
- Key frames: contact_sheet_alan_A_B.png
- App-sized encodes: app_ready/*_720x1280_30fps.mp4 (120-490 KB)
- Source: src/anim.html is the canvas renderer for A and B. Render with `python3 src/render.py A 3.2 60` (or B 3.3 60), then run ffmpeg. src/option0_fix.py rebuilds Option 0.

## Wiring (Capacitor app, not Flutter)

The app is Capacitor: a `www/` web app inside native iOS and Android shells.

- **Native splash.** Today the native splash is #120804, and iOS LaunchScreen shows welcome.jpg. Make it plain black (#000) with no image, so the first animation frame (black) continues it without a jump. Set SplashScreen `launchAutoHide:false`, then call `SplashScreen.hide({fadeOutDuration:0})` once the intro's first frame is painted.
- **A or B as native web canvas (recommended).** Port `render(opt,t)` from src/anim.html into `www/js/intro.js` and drive it with requestAnimationFrame. That's about 15 KB of JS plus fonts already bundled (Montserrat would need adding, about 200 KB variable font, or use a static Bold and Light subset). It stays crisp at every screen size and decodes no video. Use the 60 fps rendering path.
- **Video (needed for Option 0).** Use a full-screen `<video autoplay muted playsinline>` above `#welcome`, with app_ready/option_0_alan_adapted_720x1280_30fps.mp4 (120 KB). Use `object-fit:cover` on a black background. On `ended`, fade out over about 250 ms to the welcome screen. The same approach works for A and B (335 and 490 KB).
- **Lottie.** Not recommended. A's particles and B's 3D globe would be heavy, and it would mean a re-author plus a new dependency.
- **Always.** Play only on cold start, tap to skip, honour `prefers-reduced-motion` (show the end still for 0.6 s instead), and cap the time to welcome at about 3.3 s.
- **Handoff colour.** The animations end on #000. The welcome screen's base colour is #120804, so do a short crossfade or set the canvas background to #120804 (one constant, `BG`, in anim.html).
