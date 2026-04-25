# Fashion — Personal AI Stylist

A skill for **[Claude Code](https://claude.ai/code)** and **[Claude Cowork](https://claude.ai/cowork)** that acts as a deeply personalized fashion advisor — for any gender, any style direction. It learns your body, builds a wardrobe inventory, recommends outfits by occasion, runs pre-purchase checks, imports your purchases directly from Zalando and Amazon via Chrome, and gets smarter every time you wear something.

> **Works in:** Claude Code CLI · Claude Code desktop app · Claude Cowork

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
| **In-store mode** | Quick verdict mode when you're physically shopping — photo → identify → buy/skip in one response |
| **Feedback loop** | Logs what you wore, what worked, and what got compliments — learns over time |
| **Wardrobe audit** | Monthly check: flags unworn items, low-rotation pieces, sell/donate candidates |
| **Inspiration & wishlist** | Share photos or Pinterest boards — extracts style patterns, builds a prioritised wishlist |
| **Shopping links** | Every suggestion includes a Zalando.de or Amazon.de link. One click, right size |
| **Brand sizing** | Tracks your size per brand — no more guessing between brands |
| **Weather-aware** | Checks current weather automatically at session start — no need to tell it |
| **Calendar integration** | Reads your next 7 days at session start — surfaces upcoming occasions and proactively plans outfits |
| **Week-ahead planning** | "Plan my week" → full day-by-day outfit plan with weather for each event |
| **Wardrobe sprint** | Fast inventory-building mode — photo or describe items, logged immediately, no friction |
| **Weekly reminder** | Optional Sunday evening nudge to plan the week — opt-in during onboarding |
| **Seasonal planning** | City-calibrated seasons, wardrobe rotation prompts, gap identification |
| **Trend awareness** | 2025–2026 directions (menswear + womenswear) filtered through your body type and city |
| **Profile refresh** | Quarterly stylist-style check-in on body changes, life context, style drift |
| **Outfit repeat detection** | Flags if you wore the same combo to the same occasion type recently — offers a variation |
| **Vinted resell** | Generates ready-to-paste Vinted listings for low-rotation items flagged in wardrobe audit |
| **Zalando price alerts** | Optional: weekly remote agent monitors wishlist items on Zalando and messages you if price drops |
| **Wardrobe visualization** | Renders your full inventory as a visual HTML grid — dark UI, color chips, category breakdown |
| **Honest opinions** | Direct, not harsh. Will tell you why something doesn't work and what to do instead |

---

## Installation

### Step 1 — Install Claude Code or use Claude Cowork

This skill works in two environments:
- **[Claude Code](https://claude.ai/code)** — CLI or desktop app, install locally
- **[Claude Cowork](https://claude.ai/cowork)** — browser-based, no install needed; Chrome-based features (purchase import) require the Chrome extension

It does not work on claude.ai web or mobile alone — see the [Mobile](#mobile--limitations-and-workaround) section below.

### Step 2 — Clone and install the skill

```bash
git clone https://github.com/googlarz/fashion-skill.git

mkdir -p ~/.claude/skills/fashion
cp fashion-skill/SKILL.md ~/.claude/skills/fashion/SKILL.md

# Also register as slash command (optional but recommended)
mkdir -p ~/.claude/commands
cp fashion-skill/SKILL.md ~/.claude/commands/fashion.md
```

### Step 3 — Set up your data directory

Your data (body profile, wardrobe, feedback) lives in local JSON files. Point the skill to wherever you want to store them — a synced folder (Dropbox, iCloud, Google Drive, ProtonDrive) works well if you want access across machines.

```bash
# Add to your shell profile (~/.zshrc or ~/.bashrc):
export FASHION_DATA_DIR="/path/to/your/folder"
```

Then reload: `source ~/.zshrc`

If you skip this, the skill stores data next to the SKILL.md file.

### Step 4 — Verify

Start Claude Code and type:

```
/fashion
```

or just ask naturally:

> "What should I wear to a client meeting tomorrow?"

Claude will load the skill and start onboarding if it's your first session.

---

## Mobile — Limitations and Workaround

**Claude Code has no mobile app.** On your phone, you're using claude.ai — which doesn't load skills.

### What this means in practice
- Full skill (outfit recommendations, purchase import, wardrobe logging) → Claude Code on desktop only
- On mobile → you get a plain Claude session with no knowledge of your profile or wardrobe

### The workaround: claude.ai Project

Create a [Project on claude.ai](https://claude.ai) with a condensed system prompt that includes your key data. This gives you a functional stylist on mobile — good for quick in-store decisions and pre-purchase checks.

**What works on mobile via Project:**
- In-store mode: photo → identify → buy/skip verdict
- Pre-purchase check
- Outfit ideas (if you paste or upload your inventory snapshot)

**What doesn't work on mobile:**
- Chrome purchase import
- Automatic wardrobe logging back to your files
- Auto weather fetch

### Keeping the mobile Project updated

The skill handles this for you. At the end of sessions where your profile or wardrobe changes significantly, Claude will ask:

> *"Want me to generate an updated mobile summary? You can paste it into your claude.ai Project to keep your phone in sync."*

You can also trigger it manually:
> "Generate a mobile profile summary for my claude.ai Project"

Claude will produce a compact block — your body profile, color system, sizing, and wardrobe snapshot — ready to paste into Project Instructions.

---

## First Session — Onboarding

On first run, the skill opens with a warm stylist intro and guides you through five blocks:

1. **The Basics** — height, weight, measurements, brand sizes, fit challenges, sweating
2. **Show Me You** — body photos for proportions, silhouette, and color system analysis
3. **Your Life** — city, work, clients, occasions, lifestyle
4. **Your Style** — 3-word style, trusted brands, style icons, what you refuse to wear
5. **Budget** — basics / mid-range / investment tiers

Each block is conversational. Claude reacts to answers before asking the next question. It does not dump all questions at once. Profile is saved immediately after each block.

---

## Key Behaviors

### Photo identification protocol
When you share a photo of any item, Claude always:
1. Analyzes brand logo, design details, sole/stitch/collar construction, colorway
2. States its best guess: *"This looks like [Brand] [Model] in [colorway] — does that sound right?"*
3. **Never logs until you confirm** — no phantom inventory
4. Assesses fit if you're wearing it

### Outfit recommendations
Claude checks weather automatically, then asks for occasion + vibe + time of day. Builds 2–3 complete outfits from your inventory with specific styling notes. If inventory is sparse, leads with gap identification and Zalando links.

### In-store mode
Say "I'm in a store" or "quick — should I get this?" and Claude switches to fast mode: verdict first, one sentence of reasoning, one alternative link if skipping. No five questions before answering.

### Shopping links format
```
🛍️ Item name
Brand: X
Why it works for you: [body/style specific reason]
Size to look for: [your confirmed size for this brand, or best estimate]
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
Tell Claude what you wore, how it went, what got compliments. It logs everything and updates item assessments. Over time it builds a picture of what actually works vs. what looks good in theory.

### Wardrobe audit
Once a month, Claude proactively surfaces items not worn in 90+ days, flags low-rotation pieces, and suggests what to store or sell before the next season.

---

## Data Schema

All data lives in four local JSON files in your `FASHION_DATA_DIR`:

| File | Contents |
|------|----------|
| `profile.json` | Body measurements, color system, fit rules, brand sizes, lifestyle, style direction, wishlist |
| `inventory.json` | All wardrobe items with metadata, seasonal tags, fit assessments |
| `feedback.json` | Outfit logs with occasion, sentiment, compliments, learnings |
| `recommendations-history.json` | Past outfit recommendations with date and occasion |

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

## City & Climate Awareness

During onboarding the skill asks where you live and calibrates to your local climate and fashion scene. It checks live weather at session start. It knows seasonal temperature ranges, what's locally acceptable in professional settings, and which trends are relevant to your city's aesthetic.

Example cities it handles well: Berlin, London, Paris, Warsaw, Amsterdam, NYC, Tokyo — or just describe your climate and it adapts.

---

## Trend Coverage (2025–2026)

**All genders:**
- Technical outerwear as statement piece (Arc'teryx, C.P. Company, Veilance)
- Dark monochromatic dressing
- Outdoor-urban hybrid (trail shoes with smart casual)
- Quality basics over fast fashion

**Menswear:** Relaxed tailoring, merino knitwear, wide-leg trousers. Fading: skinny jeans, slim-fit everything, logo-forward pieces.

**Womenswear:** Quiet luxury (Toteme / The Row aesthetic), ballet flats and Mary Janes, oversized tailoring. Fading: micro-mini, very visible logos.

The skill filters all trends through your specific body type, gender expression, and city. It also acknowledges its training cutoff and asks you once per season if you've spotted anything new worth integrating.

---

## Version History

| Version | Date | Notes |
|---------|------|-------|
| 1.3.0 | 2026-04-25 | Outfit repeat detection, Vinted resell, Zalando price alerts, wardrobe visualization |
| 1.2.0 | 2026-04-25 | Calendar integration, week-ahead planning, wardrobe sprint, weekly reminder opt-in |
| 1.1.0 | 2026-04-19 | In-store mode, inspiration/wishlist, weather, brand sizing, wardrobe audit, profile refresh |
| 1.0.0 | 2026-04-19 | Initial release |
| 1.4.0 | 2026-04-19 | Local JSON storage, weather, brand sizing, wardrobe audit, trend freshness |
| 1.3.0 | 2026-04-19 | City-aware seasons, expanded trends, inspiration/wishlist, in-store mode |
| 1.2.0 | 2026-04-19 | Gender-neutral, Claude Cowork support, Chrome purchase import |
| 1.1.0 | 2026-04-19 | Slash command support, description optimization |
| 1.0.0 | 2026-04-19 | Initial release |

---

## License

MIT — use it, fork it, adapt it for your own body and city.

---

## Contributing

Pull requests welcome — especially for:
- New city climate profiles
- Additional shopping sites beyond Zalando/Amazon
- Improved body analysis prompts

If you fork this for your own use: the onboarding will build your profile from scratch on first run. No need to copy anyone else's data.
