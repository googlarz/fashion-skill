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

## How It Works

The skill runs entirely locally. All data lives in JSON files in the same directory as the skill. Nothing leaves your machine except shopping search queries (Zalando/Amazon links).

```
fashion/
├── SKILL.md                      # The skill itself — Claude reads this
├── profile.json                  # Your body, color system, fit rules, lifestyle
├── inventory.json                # Every item you own with metadata
├── feedback.json                 # Outfit logs + learnings from wear
├── recommendations-history.json  # Past recommendations with occasion + date
└── evals.json                    # Test cases for quality validation
```

Claude reads all files at session start and writes updates immediately after each confirmation — no batching, no lost data.

---

## Installation

### Prerequisites
- [Claude Code](https://claude.ai/code) (CLI or desktop app)

### Install the skill

```bash
# Clone the repo
git clone https://github.com/YOUR_USERNAME/fashion-skill.git

# Copy the skill to Claude's local skills directory
mkdir -p ~/.claude/skills/fashion
cp fashion-skill/SKILL.md ~/.claude/skills/fashion/SKILL.md
```

The data files (`profile.json`, `inventory.json`, etc.) should live alongside `SKILL.md` so Claude can find them relative to the skill directory. Copy them too, or let Claude create them fresh during onboarding.

```bash
cp fashion-skill/profile.json ~/.claude/skills/fashion/
cp fashion-skill/inventory.json ~/.claude/skills/fashion/
cp fashion-skill/feedback.json ~/.claude/skills/fashion/
cp fashion-skill/recommendations-history.json ~/.claude/skills/fashion/
```

### Verify installation

Start Claude Code and check that `fashion` appears in the available skills list. Then trigger it:

```
/fashion
```

or just describe what you need and Claude will pick it up automatically:

> "What should I wear to a client meeting tomorrow?"

---

## First Session — Onboarding

On first run, the skill opens with a warm stylist intro and guides you through five blocks:

1. **The Basics** — height, weight, measurements, fit challenges, sweating
2. **Show Me You** — body photos for proportions, silhouette, and color system analysis
3. **Your Life** — city, work, clients, occasions, lifestyle
4. **Your Style** — 3-word style, trusted brands, style icons, what you refuse to wear
5. **Budget** — basics / mid-range / investment tiers

Each block is conversational. Claude reacts to answers before asking the next question. It does not dump all questions at once.

After onboarding: your profile is written to `profile.json` and you can immediately start adding wardrobe items or getting recommendations.

---

## Key Behaviors

### Photo identification protocol
When you share a photo of any item, Claude always:
1. Analyzes brand logo, design details, sole/stitch/collar construction, colorway
2. States its best guess: *"This looks like [Brand] [Model] in [colorway] — does that sound right?"*
3. **Never logs until you confirm** — no phantom inventory
4. Assesses fit if you're wearing it

### Outfit recommendations
Claude asks for occasion + vibe + weather + time of day first, then builds 2–3 complete outfits from your inventory. If your inventory is sparse, it leads with the specific gaps and Zalando links for them, then shows what's possible with what you have.

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

### Feedback loop
Tell Claude what you wore, how it went, what got compliments. It logs everything to `feedback.json` and updates item assessments in `inventory.json`. Over time it builds a picture of what actually works vs. what looks good in theory.

---

## Data Schemas

### profile.json
Stores body measurements, color system, fit rules, lifestyle, style direction, and learned feedback. The `body` section is the most detailed — it captures proportions, posture, face shape, skin tone, and precise fit rules derived from those measurements.

### inventory.json
Each item has:
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

### feedback.json
```json
{
  "entries": [
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
  ]
}
```

---

## Purchase Import via Chrome

The skill can connect to your Zalando or Amazon account through Chrome and import your order history directly — no manual entry required.

```
You: import my Zalando purchases
Claude: Opens Zalando order history in Chrome, reads your orders back 3 months (or however far you want), then walks through each item:

📦 Found: Uniqlo Airism T-Shirt — Black — ordered 2026-03-12
Is this still in your wardrobe? Yes / No / Skip
```

It only logs items you confirm. Returned or donated items are skipped. After import, it assesses what you now have and flags wardrobe gaps.

**Requires:** Claude Code desktop app or Claude Cowork with Chrome access. You must be already logged into the store — the skill never handles your login credentials.

---

## Berlin-Specific Tuning

The skill is calibrated for Berlin lifestyle and climate:

- **Spring** (Mar–May): 8–18°C — transitional layering
- **Summer** (Jun–Aug): 20–32°C — breathable fabrics, lighter shoes
- **Autumn** (Sep–Nov): 8–16°C — layering season, boots
- **Winter** (Dec–Feb): -2–6°C — heavy coats, thermal base layers

Trend guidance leans toward Berlin's aesthetic: technical minimalism (Arc'teryx, Stone Island, C.P. Company), all-black or near-black as default for all genders, sneakers acceptable in most professional settings, quality over trend-chasing.

---

## Trend Coverage (2025–2026)

**All genders:**
- Technical outerwear as statement piece
- Dark monochromatic dressing
- Outdoor-urban hybrid (trail shoes with smart casual)
- Quality basics over fast fashion

**Menswear:** Relaxed tailoring, merino knitwear, wide-leg trousers. Fading: skinny jeans, slim-fit everything, logo-forward pieces.

**Womenswear:** Quiet luxury (Toteme / The Row aesthetic), ballet flats and Mary Janes, oversized tailoring with fitted basics. Fading: micro-mini, very visible logos.

The skill filters all trends through your specific body type, gender expression, and style — it will tell you which ones don't suit you and why.

---

## Running Evals

The `evals.json` file contains 7 test cases covering the main skill behaviors. To run them:

```bash
# From the skill-creator directory (if you have it)
python3 -m scripts.run_eval --eval-set /path/to/Fashion/evals.json \
  --skill-path ~/.claude/skills/fashion \
  --model claude-sonnet-4-6
```

All 7 evals pass with 43/43 assertions in the current version.

---

## Version History

| Version | Date | Notes |
|---------|------|-------|
| 1.1.0 | 2026-04-19 | Gender-neutral, Claude Cowork support, Chrome purchase import from Zalando/Amazon, womenswear trends added |
| 1.0.0 | 2026-04-19 | Initial release — full onboarding, photo protocol, Berlin seasons, 2025–26 trends, Zalando links, feedback loop |

---

## License

MIT — use it, fork it, adapt it for your own body and city.

---

## Contributing

This is a personal skill — the data files (`profile.json`, `inventory.json`) contain one person's specific measurements and wardrobe. If you fork it:

1. Delete or clear `profile.json` and `inventory.json` before your first run
2. Let the onboarding build your profile from scratch
3. The skill works for any man's body — the AI adapts all recommendations to your specific measurements and color system

Pull requests for skill improvements (better instructions, new behavior patterns, bug fixes) are welcome.
