# Fashion — Personal AI Stylist

A skill for **[Claude Code](https://claude.ai/code)** and **[Claude Cowork](https://claude.ai/cowork)** that acts as a deeply personalized fashion advisor — for any gender, any style direction. It learns your body, builds a wardrobe inventory, recommends outfits by occasion, runs pre-purchase checks, and gets smarter every time you wear something.

> **Works in:** Claude Code CLI · Claude Code desktop app · Claude Cowork

Built for people who care about looking sharp without spending hours thinking about it.

---

## What It Does

| Capability | Description |
|------------|-------------|
| **Profile onboarding** | Guided 5-block interview (body, photos, lifestyle, style, budget) — conversational, not a form |
| **Body analysis** | Derives silhouette, color system (Cool Winter / Warm Autumn / etc.), and fit rules from photos — any body shape, any gender |
| **Wardrobe inventory** | Photo-based item logging with brand/model identification before logging — no guessing |
| **Purchase import** | Connects to Zalando.de or Amazon.de via Chrome, pulls order history, walks through each item to confirm |
| **Outfit recommendations** | 2–3 complete looks per occasion with WHY each piece works for your specific body |
| **Outfit repeat detection** | Flags if you wore the same combo to the same occasion type recently — offers a variation |
| **Pre-purchase check** | Photo in → honest Buy / Skip / Only if verdict with outfit combos and a Zalando link |
| **In-store mode** | Quick verdict when physically shopping — photo → identify → buy/skip in one response |
| **Calendar integration** | Reads your next 7 days at session start — surfaces upcoming occasions, proactively plans outfits |
| **Week-ahead planning** | "Plan my week" → full day-by-day outfit plan with per-day weather |
| **Wardrobe sprint** | Fast inventory-building mode — photo or describe items, logged immediately, no friction |
| **Wardrobe visualization** | "Show my wardrobe" → HTML visual grid with color chips, seasons, categories, gap summary |
| **Feedback loop** | Logs what you wore, what worked, what got compliments — learns over time |
| **Wardrobe audit** | Monthly: flags unworn items (90d+), low-rotation pieces, sell/donate candidates |
| **Vinted resell** | Generates ready-to-paste Vinted listings for flagged items — title, description, price guidance |
| **Inspiration & wishlist** | Share photos or Pinterest boards — extracts style patterns, builds a prioritised wishlist |
| **Zalando price alerts** | Optional weekly remote agent that monitors wishlist items and messages you on a price drop |
| **Shopping links** | Every suggestion includes a Zalando.de or Amazon.de link. One click, right size |
| **Brand sizing** | Tracks your confirmed size per brand — no more guessing |
| **Weather-aware** | Checks current weather automatically at session start |
| **Seasonal planning** | City-calibrated seasons, rotation prompts, gap identification |
| **Trend awareness** | 2025–2026 directions filtered through your body type, city, and style |
| **Profile refresh** | Quarterly stylist-style check-in — body changes, life context, style drift |
| **Weekly reminder** | Optional Sunday evening nudge to plan your week — opt-in during onboarding |
| **Mobile summary** | Generates a compact profile snapshot to paste into a claude.ai Project for on-the-go use |
| **Honest opinions** | Direct, not harsh. Will tell you why something doesn't work and what to do instead |

---

## Installation

### Step 1 — Install Claude Code or use Claude Cowork

This skill works in two environments:
- **[Claude Code](https://claude.ai/code)** — CLI or desktop app, install locally
- **[Claude Cowork](https://claude.ai/cowork)** — browser-based, no install needed; Chrome-based features require the Chrome extension

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

Your data (body profile, wardrobe, feedback) lives in local JSON files. Point the skill to wherever you want to store them — a synced folder (Dropbox, iCloud, Google Drive, ProtonDrive) works well for multi-machine access.

```bash
# Add to your shell profile (~/.zshrc or ~/.bashrc):
export FASHION_DATA_DIR="/path/to/your/folder"
```

Then reload: `source ~/.zshrc`

If you skip this, the skill stores data next to the SKILL.md file.

### Step 4 — Verify

Start Claude Code and type `/fashion` or ask naturally:

> "What should I wear to a client meeting tomorrow?"

Claude will load the skill and start onboarding if it's your first session.

---

## Mobile — Limitations and Workaround

**Claude Code has no mobile app.** On your phone, claude.ai doesn't load skills.

### What works / doesn't work on mobile

| Feature | Desktop (Claude Code) | Mobile (claude.ai) |
|---|---|---|
| Outfit recommendations | ✅ Full | ⚠️ With Project setup |
| In-store / pre-purchase check | ✅ | ⚠️ With Project setup |
| Wardrobe logging | ✅ | ❌ |
| Chrome purchase import | ✅ | ❌ |
| Auto weather fetch | ✅ | ❌ |
| Calendar integration | ✅ | ❌ |

### The workaround: claude.ai Project

Create a Project on claude.ai with a condensed system prompt containing your key data. Good for quick in-store decisions and pre-purchase checks on the go.

The skill generates this for you automatically after sessions where your data changes significantly:

> *"Your profile changed — want me to generate an updated mobile summary? Paste it into your claude.ai Project to keep your phone in sync."*

Or trigger manually: *"Generate a mobile profile summary"*

---

## First Session — Onboarding

Five conversational blocks — Claude reacts to each answer before asking the next question:

1. **The Basics** — height, weight, measurements, brand sizes, fit challenges, sweating
2. **Show Me You** — body photos for proportions, silhouette, and color system analysis
3. **Your Life** — city, work, clients, occasions, lifestyle
4. **Your Style** — 3-word style, trusted brands, style icons, what you refuse to wear
5. **Budget** — basics / mid-range / investment tiers + optional weekly reminder setup

Profile is saved after each block. Never all at once.

---

## Key Behaviors

### Photo identification
Photo in → analyze brand logo, design details, construction → *"This looks like [Brand] [Model] in [colorway] — right?"* → never logs until you confirm.

### Outfit recommendations
Reads calendar + weather automatically. Asks for occasion + vibe + time of day. Builds 2–3 complete outfits from inventory with body-specific reasoning. Flags repeats if you wore that combo recently.

### In-store mode
*"I'm in a store"* or *"quick — should I get this?"* → verdict first, one sentence of reasoning, one alternative link if skipping.

### Week planning
*"Plan my week"* → reads 7-day calendar, checks per-day weather, generates a clean daily outfit plan with gaps flagged inline.

### Wardrobe visualization
*"Show my wardrobe"* → renders an HTML visual grid: dark UI, color chips, season/occasion tags, stats header, key gaps below. Wishlist ghost cards optional.

### Vinted resell
After monthly audit flags low-rotation items, generates ready-to-paste Vinted listings: title, description, condition, category, size, suggested price based on brand tier.

### Zalando price alerts
Add high-priority item to wishlist → *"Want a price alert?"* → if yes, guides through setting up a weekly remote agent (Chrome + messaging MCP) that messages you when the price drops.

### Purchase import via Chrome
```
You: import my Zalando purchases
Claude: Opens Zalando order history, reads your orders, walks through each:

📦 Found: Uniqlo Airism T-Shirt — Black — ordered 2026-03-12
Is this still in your wardrobe? Yes / No / Skip
```
Requires Claude Code desktop or Cowork with Chrome access.

---

## Data Schema

Four local JSON files in `FASHION_DATA_DIR`:

| File | Contents |
|------|----------|
| `profile.json` | Body measurements, color system, fit rules, brand sizes, lifestyle, style direction, wishlist, inspiration |
| `inventory.json` | All wardrobe items with metadata, seasonal tags, fit assessments, wear history |
| `feedback.json` | Outfit logs with occasion type, sentiment, compliments, learnings |
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
  "occasion": "client dinner",
  "occasion_type": "smart_casual",
  "sentiment": "confident",
  "compliments": [{"on": "watch", "from": "colleague"}],
  "didnt_work": "",
  "learnings": "All-black monochrome works well"
}
```

---

## City & Climate Awareness

Onboarding asks where you live. The skill calibrates seasons, trend recommendations, and professional dress norms to your city. Checks live weather at every session start.

Works well for: Berlin, London, Paris, Warsaw, Amsterdam, NYC, Tokyo — or describe your climate and it adapts.

---

## Trend Coverage (2025–2026)

**All genders:** Technical outerwear, dark monochromatic dressing, outdoor-urban hybrid, quality basics over fast fashion.

**Menswear:** Relaxed tailoring, merino knitwear, wide-leg trousers. Fading: skinny jeans, slim-fit everything, logo-forward pieces.

**Womenswear:** Quiet luxury (Toteme / The Row aesthetic), ballet flats, oversized tailoring. Fading: micro-mini, very visible logos.

Filtered through your body type, gender expression, and city. Acknowledges training cutoff — asks once per season if you've spotted anything new worth integrating.

---

## Version History

| Version | Date | Notes |
|---------|------|-------|
| 1.3.0 | 2026-04-25 | Outfit repeat detection, Vinted resell, Zalando price alerts, wardrobe visualization |
| 1.2.0 | 2026-04-25 | Calendar integration, week-ahead planning, wardrobe sprint, weekly reminder |
| 1.1.0 | 2026-04-25 | In-store mode, inspiration/wishlist, weather, brand sizing, wardrobe audit, profile refresh |
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
- New resell platform integrations (Depop, eBay, etc.)

If you fork this for your own use: onboarding builds your profile from scratch on first run. No need to copy anyone else's data.
