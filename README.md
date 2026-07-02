# Yasir Garments — Inventory & Billing (PWA)

Free, installable app (desktop + mobile), offline-first, with automatic Google Sheets sync.
**Cost: Rs 0** — Google Sheet aap ka free database hai, Apps Script free backend, GitHub Pages free hosting.

## Package mein kya hai
| File | Kaam |
|------|------|
| `index.html` | Pura app (UI + logic) |
| `manifest.json` | App ki identity (naam, icon) — install ke liye |
| `service-worker.js` | Offline support (app shell cache) |
| `icon-192.png`, `icon-512.png` | App icons |
| `Code.gs` | Google Apps Script backend (Sheet ko API banata hai) |

> Sab files **ek hi folder** mein rehni chahiye. File names change na karein.

---

## STEP 1 — App ko free host karein (GitHub Pages)

PWA install + offline ke liye app ko HTTPS pe hona zaroori hai (sirf file kholne se install nahi hoti).

1. [github.com](https://github.com) pe free account banayein.
2. Naya **repository** banayein (e.g. `yasir-garments`), Public rakhein.
3. In 6 files ko upload karein (drag & drop): `index.html`, `manifest.json`, `service-worker.js`, `icon-192.png`, `icon-512.png`.
   (`Code.gs` upload karne ki zarurat nahi — woh Sheet ke andar jata hai, Step 3.)
4. Repo → **Settings → Pages** → *Branch:* `main`, *Folder:* `/ (root)` → **Save**.
5. 1–2 min baad URL milega: `https://<username>.github.io/yasir-garments/`
6. Woh URL phone/desktop pe kholein.

*(Netlify Drop bhi free alternative hai: [app.netlify.com/drop](https://app.netlify.com/drop) pe folder drag karein.)*

---

## STEP 2 — Install karein

- **Desktop (Chrome/Edge):** address bar mein "Install" icon ⊕ → Install.
- **Android (Chrome):** menu ⋮ → **Add to Home screen / Install app**.
- **iPhone (Safari):** Share → **Add to Home Screen**.

Ab desktop/phone pe ek alag app ki tarah khulegi, aur **offline bhi chalegi**.

---

## STEP 3 — Google Sheets backend (auto-sync)

1. Nai **Google Sheet** banayein.
2. **Extensions → Apps Script**.
3. `Code.gs` ka pura code paste karein → **Save** (💾).
4. **Deploy → New deployment** → gear ⚙️ → **Web app**.
5. *Execute as:* **Me** · *Who has access:* **Anyone** → **Deploy** → permissions allow karein.
6. Jo **Web app URL** (`…/exec`) mile, copy karein.
7. App kholein → **Settings** → URL paste karein → **Sync to Sheets**.

Tabs (Inventory / Sales / Returns / Settings) khud ban jayenge.

---

## Offline + Sync kaise kaam karta hai

- Saara data pehle **device pe local** save hota hai → app turant chalti hai, net ho ya na ho.
- Online hote hi changes **auto-sync** ho jate hain (debounced).
- **Offline** mein kaam karein → neeche pill **"Offline · saved locally"** dikhata hai. Net wapas aate hi khud sync ("Pending changes" → "Online · Synced").
- App khulte waqt: agar local changes pending hain to woh upload, warna Sheet se latest **download** ho jata hai (taake dusre device ka data mil jaye).
- Neeche-dayein pill tap karke kabhi bhi manually sync kar sakte hain.

### Note (multi-device)
Yeh snapshot-sync single shop ke liye perfect hai. Agar **do device ek hi waqt** edit karein (dono offline), to baad mein sync karne wala device pehle wale ko overwrite kar sakta hai. Aksar single owner ke liye masla nahi — lekin agar 2 log saath bechte hain, to baad mein merge-based sync (per-transaction) pe upgrade kar sakte hain.
