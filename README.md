# ALONE Audio Studio — Fixed & Vercel-Ready

A cyberpunk-aesthetic web DAW with waveform editor, 10-band EQ, multi-track mixer, FX rack, and export (WAV/MP3).

## 🚀 Deploy to Vercel (Frontend Only)

The frontend is fully self-contained and works without the backend.  
AI stem separation requires the optional Python backend.

### One-click deploy

1. Push the `frontend/` folder to a GitHub repo (or the whole project)
2. In Vercel → New Project → select repo
3. **Root Directory**: set to `frontend`
4. Framework: **Next.js** (auto-detected)
5. Click **Deploy**

No environment variables required for basic use.

### Optional: Backend for AI Separation

Set `NEXT_PUBLIC_API_URL` to your backend URL in Vercel environment variables.

---

## 🛠 Local Development

```bash
cd frontend
npm install
npm run dev
# → http://localhost:3000
```

## 📋 Bugs Fixed

| # | Bug | Fix |
|---|-----|-----|
| 1 | `tone@^15` doesn't exist on npm | Downgraded to stable `tone@^14.7.77` |
| 2 | SSR crash — WaveSurfer/Tone.js imported at module level | Wrapped all audio imports with `dynamic()` + `ssr: false` |
| 3 | `useAudioStore()` called with whole state object (performance + stale renders) | Switched to individual selectors throughout |
| 4 | `FXKnob` had local state out-of-sync with store | Removed local state — uses parent value directly |
| 5 | `ADD REGION` button enabled when no audio loaded | Disabled unless `wsReady === true` |
| 6 | Memory leak — blob URLs never revoked on track remove | `URL.revokeObjectURL()` in `removeTrack` |
| 7 | `btn-cyber-solid` class missing CSS definition | Added full CSS rule in globals.css |
| 8 | `.scanlines` div was `<div>` not `<div className="scanlines">` pseudo | Already correct, fixed `::after` z-index overlap |
| 9 | Vertical range slider gradient didn't follow value | Added `to top` gradient for vertical sliders |
| 10 | Firefox range track unstyled | Added `-moz-range-track/progress/thumb` CSS |
| 11 | Export download `<a href>` blocked by COEP headers | Changed to programmatic `document.createElement("a")` |
| 12 | `lamejs` import pattern broken (ESM vs CJS) | Fixed with `.default?.Mp3Encoder ?? .Mp3Encoder` fallback |
| 13 | `QuickControl` used destructured store actions (stale closure) | Uses `useAudioStore.getState()` inside handler |
| 14 | `separateTrack` error silently cleared — user sees nothing | Shows error label for 3s before clearing |
| 15 | Missing favicon/icon | Added uploaded logo as `/icon.png` in metadata + header |
| 16 | `eslint` config missing — breaks `next build` | Added `.eslintrc.json` |
| 17 | `WaveSurfer finish` event not handled | Calls `stop()` on finish to sync play state |
| 18 | `Tone.Reverb` constructor API mismatch in v14 | Changed to `new Tone.Reverb({ decay, wet })` |
