# Tung Chi Birthday — Project Guide

## Overview
A birthday photo-sharing landing page for **Tùng Chi**.
Visitors upload photos with a name + wish, photos appear in a shared real-time gallery.
A special **"🎁 GIFT FOR CHI"** page animates uploaded photos into the letters **C · H · I**.

---

## Stack
| Layer    | Technology |
|----------|-----------|
| Frontend | Plain HTML / CSS / JS (no framework) |
| Database | Supabase (PostgreSQL) — table: `photos` |
| Storage  | Supabase Storage — bucket: `photos` (public) |
| Hosting  | Vercel |
| Repo     | GitHub — `vundaclaude/tung-chi-birthday` |

---

## Live URLs
- **Main page:** https://tung-chi-birthday.vercel.app
- **Gift page:** https://tung-chi-birthday.vercel.app/show.html
- **GitHub:** https://github.com/vundaclaude/tung-chi-birthday

---

## Supabase Project
- **Project ID:** `wotqkykbpnoxjkfthbkn`
- **URL:** `https://wotqkykbpnoxjkfthbkn.supabase.co`
- **Anon key:** `sb_publishable_rzafCHztg-THj0YRnSpD0w_DXB1_vde`
- **Personal access token:** `sbp_539a0c52227282d069a4489c1533d8e66fdc0245`
- **Storage bucket:** `photos` (public)

### Database schema
```sql
create table photos (
  id         bigint generated always as identity primary key,
  name       text not null,
  message    text,
  url        text not null,
  created_at timestamptz default now()
);
```

### RLS Policies (photos table)
- `Anyone can read`  — SELECT for anon
- `Anyone can insert` — INSERT for anon
- `Anyone can delete` — DELETE for anon

### Storage Policies (storage.objects)
- `storage_insert` — INSERT to anon for bucket `photos`
- `storage_select` — SELECT for anon for bucket `photos`
- `storage_delete` — DELETE for anon for bucket `photos`

---

## Files
| File         | Purpose |
|--------------|---------|
| `index.html` | Main landing page — upload form + real-time gallery |
| `show.html`  | Gift page — photos animate into letters C · H · I |

---

## Deploy
```bash
# Deploy to Vercel (run from project folder)
vercel --token <VERCEL_TOKEN> --yes --prod --scope vundaclaudes-projects

# Push to GitHub
git add . && git commit -m "message" && git push
```

---

## Key Features

### index.html
- Side-by-side layout (form left, gallery right) — stacks on mobile
- Floating-label inputs with icons
- Drag-and-drop image upload to Supabase Storage
- **Optimistic UI** — photo appears in gallery instantly after send
- Real-time gallery via Supabase Realtime
- **Delete with password** — hover card → 🗑️ → modal → password: `123abc`
- Stats strip: total photos + unique contributors
- Lightbox on photo click

### show.html (Gift For Chi)
- Dark starry background
- Photos fill pixel-art letters **C · H · I**
- **C** animates by tracing its shape (top arc → left side → bottom arc) with smooth easing
- **H** and **I** animate top-to-bottom with spring pop
- Auto-swap: photos rotate through all cells (uses all photos before repeating)
- **Shuffle** button — reassigns all photos instantly
- **Auto / Stop** toggle — cycles one cell per second
- Hover tooltip shows contributor name + wish
- Click cell → lightbox

---

## Credentials (keep private)
- Delete password: `123abc`
- Vercel token: stored locally (vercel.com/account/tokens)
- Supabase personal token: stored locally (supabase.com/dashboard/account/tokens)
