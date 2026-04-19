# Fashion — Personal AI Stylist

A [Claude Code](https://claude.ai/code) skill that acts as a deeply personalized fashion advisor — for any gender, any style direction. It learns your body, builds a wardrobe inventory, recommends outfits by occasion, runs pre-purchase checks, imports your purchases directly from Zalando and Amazon via Chrome, and gets smarter every time you wear something.

**Works in:** Claude Code CLI, Claude Code desktop app, and Claude Cowork.

Built for people who care about looking sharp without spending hours thinking about it.

---

## What It Does

| Capability | Description |
|------------|-------------|
| **Profile onboarding** | Guided 5-block interview (body, photos, lifestyle, style, budget) — conversational, not a form |
| **Body analysis** | Derives silhouette, color system (Cool Winter / Warm Autumn / etc.), and precise fit rules from photos — works for any body shape and gender |
| **Wardrobe inventory** | Photo-based item logging with brand/model identification before logging — no guessing |
| **Purchase import** | Connects to Zalando.de or Amazon.de via Chrome, pulls your order history, and walks you through each item to confirm before logging |
| **Outfit recommendations** | 2–3 complete looks per occasion with WHY each piece works for your specific body |
| **Pre-purchase check** | Photo in → honest Buy / Skip / Only if verdict with outfit combos and a Zalando link |
| **Feedback loop** | Logs what you wore, what worked, and what got compliments — learns over time |
| **Shopping links** | Every suggestion includes a Zalando.de or Amazon.de link. One click, right size |
| **Seasonal planning** | Berlin-calibrated seasons (Mar–Feb), wardrobe rotation prompts, gap identification |
| **Trend awareness** | 2025–2026 directions (menswear + womenswear) filtered through your body type and Berlin context |
| **Honest opinions** | Direct, not harsh. Will tell you why something doesn't work and what to do instead |

---

## Installation

### Step 1 — Install Claude Code

Download [Claude Code](https://claude.ai/code) (CLI or desktop app). This skill requires Claude Code — it does not work on claude.ai web or mobile alone.

### Step 2 — Clone and install the skill

```bash
git clone https://github.com/googlarz/fashion-skill.git

mkdir -p ~/.claude/skills/fashion
cp fashion-skill/SKILL.md ~/.claude/skills/fashion/SKILL.md

# Also register as slash command (optional but recommended)
mkdir -p ~/.claude/commands
cp fashion-skill/SKILL.md ~/.claude/commands/fashion.md
```

### Step 3 — Set up your data backend (Supabase)

Your data (body profile, wardrobe, feedback) lives in a free Supabase database so it's available across all your devices.

**3a. Create a free Supabase project** at [supabase.com](https://supabase.com)
- Choose the region closest to you
- Free tier is more than enough — you'll never hit the limits

**3b. Run this SQL** in the Supabase SQL Editor (Dashboard → SQL Editor → New query):

```sql
create table if not exists fashion_store (
  key text primary key,
  data jsonb not null,
  updated_at timestamptz default now()
);

alter table fashion_store disable row level security;

insert into fashion_store (key, data) values
  ('profile', '{}'::jsonb),
  ('inventory', '{}'::jsonb),
  ('feedback', '{"entries":[]}'::jsonb),
  ('recommendations', '{"recommendations":[]}'::jsonb)
on conflict (key) do nothing;
```

**3c. Get your credentials** from Supabase Dashboard → Settings → API:
- `Project URL` (looks like `https://xxxx.supabase.co`)
- `anon public` key

**3d. Add to your shell profile** (`~/.zshrc` or `~/.bashrc`):

```bash
export FASHION_SUPABASE_URL="https://your-project.supabase.co"
export FASHION_SUPABASE_KEY="your-anon-key"
```

Then reload: `source ~/.zshrc`

### Step 4 — Verify

Start Claude Code and type:

```
/fashion
```

or just ask naturally:

> "What should I wear to a client meeting tomorrow?"

Claude will load the skill, connect to your Supabase, and start onboarding if it's your first session.

---

## First Session — Onboarding

On first run, the skill opens with a warm stylist intro and guides you through five blocks:

1. **The Basics** — height, weight, measurements, fit challenges, sweating
2. **Show Me You** — body photos for proportions, silhouette, and color system analysis
3. **Your Life** — city, work, clients, occasions, lifestyle
4. **Your Style** — 3-word style, trusted brands, style icons, what you refuse to wear
5. **Budget** — basics / mid-range / investment tiers + Supabase setup if not done yet

Each block is conversational. Claude reacts to answers before asking the next question. It does not dump all questions at once. After onboarding your profile is saved to Supabase immediately.

---

## Key Behaviors

### Photo identification protocol
When you share a photo of any item, Claude always:
1. Analyzes brand logo, design details, sole/stitch/collar construction, colorway
2. States its best guess: *"This looks like [Brand] [Model] in [colorway] — does that sound right?"*
3. **Never logs until you confirm** — no phantom inventory
4. Assesses fit if you're wearing it

### Outfit recommendations
Claude asks for occasion + vibe + weather + time of day first, then builds 2–3 complete outfits from your inventory. If your inventory is sparse, it leads with the specific gaps and Zalando links, then shows what's possible with what you have.

### Shopping links format
```
🛍️ Item name
Brand: X
Why it works for you: [body/style specific reason]
Size to look for: [your exact size]
Price range: ~€X
→ Zalando: https://www.zalando.de/search/?q=item+name
→ Amazon: https://www.amazon.de/s?k=item+name
```

### Purchase import via Chrome
```
You: import my Zalando purchases
Claude: Opens Zalando order history in Chrome, reads your orders, then walks through each item:

📦 Found: Uniqlo Airism T-Shirt — Black — ordered 2026-03-12
Is this still in your wardrobe? Yes / No / Skip
```
Only confirmed items are logged. Requires Claude Code desktop app with Chrome access.

### Feedback loop
Tell Claude what you wore, how it went, what got compliments. It logs everything to Supabase and updates item assessments. Over time it builds a picture of what actually works vs. what looks good in theory.

---

## Data Schema

All data lives in a single Supabase table (`fashion_store`) with four rows:

| key | data |
|-----|------|
| `profile` | Body measurements, color system, fit rules, lifestyle, style direction |
| `inventory` | All wardrobe items with metadata, seasonal tags, fit assessments |
| `feedback` | Outfit logs with occasion, sentiment, compliments, learnings |
| `recommendations` | Past outfit recommendations with date and occasion |

### inventory item schema
```json
{
  "id": "top_001",
  "category": "tops",
  "subcategory": "t-shirts",
  "name": "Uniqlo Airism T-shirt",
  "brand": "Uniqlo",
  "color": "black",
  "size": "L",
  "seasons": ["spring", "summer", "autumn"],
  "occasions": ["everyday", "work casual"],
  "fit_assessment": "...",
  "verdict": "...",
  "confirmed_by_user": true,
  "condition": "good",
  "added_date": "2026-04-18"
}
```

### feedback entry schema
```json
{
  "id": "fb_001",
  "date": "2026-04-19",
  "items_worn": ["top_001", "acc_001"],
  "occasion": "work meeting",
  "sentiment": "confident",
  "compliments": [{"on": "watch", "from": "colleague"}],
  "didnt_work": "",
  "surprises": "",
  "learnings": "All-black monochrome works well; watch reads as a statement piece"
}
```

---

## Alternative Data Backends

Supabase is the recommended backend but any REST-accessible store works. Adapt the read/write calls in SKILL.md to match:

- **PocketBase** — self-hosted alternative, same REST pattern
- **Firebase Firestore** — works via REST API
- **Notion API** — possible but slower
- **Local files** — works on Mac/PC only (no cross-device sync); remove Supabase section from SKILL.md and store JSON files locally

---

## Trend Coverage (2025–2026)

**All genders:**
- Technical outerwear as statement piece (Arc'teryx, C.P. Company, Veilance)
- Dark monochromatic dressing
- Outdoor-urban hybrid (trail shoes with smart casual)
- Quality basics over fast fashion

**Menswear:** Relaxed tailoring, merino knitwear, wide-leg trousers. Fading: skinny jeans, slim-fit everything, logo-forward pieces.

**Womenswear:** Quiet luxury (Toteme / The Row aesthetic), ballet flats and Mary Janes, oversized tailoring. Fading: micro-mini, very visible logos.

The skill filters all trends through your specific body type, gender expression, and city.

---

## Version History

| Version | Date | Notes |
|---------|------|-------|
| 1.2.0 | 2026-04-19 | Supabase backend — data syncs across all devices |
| 1.1.0 | 2026-04-19 | Gender-neutral, Claude Cowork support, Chrome purchase import |
| 1.0.0 | 2026-04-19 | Initial release |

---

## License

MIT — use it, fork it, adapt it for your own body and city.

---

## Contributing

Pull requests welcome — especially for:
- New city climate profiles (currently Berlin-tuned)
- Additional shopping sites beyond Zalando/Amazon
- New backend integrations
- Improved body analysis prompts

If you fork this for your own use: the onboarding will build your profile from scratch on first run. No need to copy anyone else's data.
